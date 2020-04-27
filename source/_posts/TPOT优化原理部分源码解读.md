---
title: TPOT优化原理部分源码解读
date: 2020-03-28 22:24:35
tags: 机器学习
categories: 机器学习
---

TPOT的涉及到的建模过程，以TPOT light为例主要有3块：Classifiers、Preprocesssors、Selectors，TPOT的遗传算法优化是以pipeline为基础的，也就是说pipeline就相当于基因序列。通过构建初始pipeline，再经过遗传算法中的交叉、变异最终生成符合条件的模型效果较好的pipeline。我们从最优的那一代中选取其中建模效果最好的pipeline即可，TPOT是基于scikit-learn框架的，它本身不去实现我们常用的分类回归等算法。而是通过遗传算法优化pipeline。也就是从中选出最优的数据处理、特征选择、分类算法的组合。
<!--more-->
一个简单的TPOT的使用实例：

```python
#!/usr/local/bin/python
# -*- coding:utf-8 -*-
from tpot import TPOTClassifier
from sklearn.datasets import load_iris
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split
 
iris = load_digits()
iris.data[0:5], iris.target
print iris
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target,train_size=0.75, test_size=0.25)
X_train.shape, X_test.shape, y_train.shape, y_test.shape
 
tpot = TPOTClassifier(verbosity=2, max_time_mins=2,config_dict="TPOT light",population_size=10,mutation_rate=0.9,crossover_rate=0.1)
tpot.fit(X_train, y_train)
print(tpot.score(X_test, y_test))
```

运行结果如下：

![](1.png)

### 部分核心源码解读

#### 1、初始群体的生成
这段代码可以看出，pipeline的构建是通过生成不同深度的树类构建初始pipeline的，大致过程是先确定树的深度，从Classifiers中选择一个分类器放入树的顶端，接下来构建分类器的参数，以上均为随机选取。如果树的深度大于1，则还需要从Preprocesssors、Selectors中选择算子加入到pipeline中，还要进行参数初始化。

```python
    @_pre_test
    def _generate(self, pset, min_, max_, condition, type_=None):
        """Generate a Tree as a list of lists.
        The tree is build from the root to the leaves, and it stop growing when
        the condition is fulfilled.
        Parameters
        ----------
        pset: PrimitiveSetTyped
            Primitive set from which primitives are selected.
        min_: int
            Minimum height of the produced trees.
        max_: int
            Maximum Height of the produced trees.
        condition: function
            The condition is a function that takes two arguments,
            the height of the tree to build and the current
            depth in the tree.
        type_: class
            The type that should return the tree when called, when
            :obj:None (default) no return type is enforced.
        Returns
        -------
        individual: list
            A grown tree with leaves at possibly different depths
            dependending on the condition function.
        """
        if type_ is None:
            type_ = pset.ret
        expr = []
        height = np.random.randint(min_, max_)
        stack = [(0, type_)]
        while len(stack) != 0:
            depth, type_ = stack.pop()

            # We've added a type_ parameter to the condition function
            if condition(height, depth, type_):
                try:
                    term = np.random.choice(pset.terminals[type_])
                except IndexError:
                    _, _, traceback = sys.exc_info()
                    raise IndexError(
                        'The gp.generate function tried to add '
                        'a terminal of type {}, but there is'
                        'none available. {}'.format(type_, traceback)
                    )
                if inspect.isclass(term):
                    term = term()
                expr.append(term)
            else:
                try:
                    prim = np.random.choice(pset.primitives[type_])
                except IndexError:
                    _, _, traceback = sys.exc_info()
                    raise IndexError(
                        'The gp.generate function tried to add '
                        'a primitive of type {}, but there is'
                        'none available. {}'.format(type_, traceback)
                    )
                expr.append(prim)
                for arg in reversed(prim.args):
                    stack.append((depth + 1, arg))
        return expr
```

#### 2、适应性值评估检测

```python
    def _evaluate_individuals(self, individuals, features, target, sample_weight=None, groups=None):
        """Determine the fit of the provided individuals.
        Parameters
        ----------
        individuals: a list of DEAP individual
            One individual is a list of pipeline operators and model parameters that can be
            compiled by DEAP into a callable function
        features: numpy.ndarray {n_samples, n_features}
            A numpy matrix containing the training and testing features for the individual's evaluation
        target: numpy.ndarray {n_samples}
            A numpy matrix containing the training and testing target for the individual's evaluation
        sample_weight: array-like {n_samples}, optional
            List of sample weights to balance (or un-balanace) the dataset target as needed
        groups: array-like {n_samples, }, optional
            Group labels for the samples used while splitting the dataset into train/test set
        Returns
        -------
        fitnesses_ordered: float
            Returns a list of tuple value indicating the individual's fitness
            according to its performance on the provided data
        """
 
        operator_counts, eval_individuals_str, sklearn_pipeline_list, stats_dicts = self._preprocess_individuals(individuals)
 
        # Make the partial function that will be called below
        partial_wrapped_cross_val_score = partial(
            _wrapped_cross_val_score,
            features=features,
            target=target,
            cv=self.cv,
            scoring_function=self.scoring_function,
            sample_weight=sample_weight,
            groups=groups,
            timeout=self.max_eval_time_seconds
        )
 
        result_score_list = []
        # Don't use parallelization if n_jobs==1
        if self.n_jobs == 1:
            for sklearn_pipeline in sklearn_pipeline_list:
                self._stop_by_max_time_mins()
                val = partial_wrapped_cross_val_score(sklearn_pipeline=sklearn_pipeline)
                result_score_list = self._update_val(val, result_score_list)
        else:
            # chunk size for pbar update
            # chunk size is min of cpu_count * 2 and n_jobs * 4
            chunk_size = min(cpu_count()*2, self.n_jobs*4)
            for chunk_idx in range(0, len(sklearn_pipeline_list), chunk_size):
                self._stop_by_max_time_mins()
                parallel = Parallel(n_jobs=self.n_jobs, verbose=0, pre_dispatch='2*n_jobs')
                tmp_result_scores = parallel(delayed(partial_wrapped_cross_val_score)(sklearn_pipeline=sklearn_pipeline)
                                             for sklearn_pipeline in sklearn_pipeline_list[chunk_idx:chunk_idx + chunk_size])
                # update pbar
                for val in tmp_result_scores:
                    result_score_list = self._update_val(val, result_score_list)
 
        self._update_evaluated_individuals_(result_score_list, eval_individuals_str, operator_counts, stats_dicts)
 
        """Look up the operator count and cross validation score to use in the optimization"""
        return [(self.evaluated_individuals_[str(individual)]['operator_count'],
                 self.evaluated_individuals_[str(individual)]['internal_cv_score'])
                for individual in individuals]
```

### 3、变异

变异主要是增加pipeline的内容或更换其中的参数。

```python
    def _random_mutation_operator(self, individual, allow_shrink=True):
        """Perform a replacement, insertion, or shrink mutation on an individual.
        Parameters
        ----------
        individual: DEAP individual
            A list of pipeline operators and model parameters that can be
            compiled by DEAP into a callable function
        allow_shrink: bool (True)
            If True the `mutShrink` operator, which randomly shrinks the pipeline,
            is allowed to be chosen as one of the random mutation operators.
            If False, `mutShrink`  will never be chosen as a mutation operator.
        Returns
        -------
        mut_ind: DEAP individual
            Returns the individual with one of the mutations applied to it
        """
        mutation_techniques = [
            partial(gp.mutInsert, pset=self._pset),
            partial(mutNodeReplacement, pset=self._pset)
        ]
 
        # We can't shrink pipelines with only one primitive, so we only add it if we find more primitives.
        number_of_primitives = sum([isinstance(node, deap.gp.Primitive) for node in individual])
        if number_of_primitives > 1 and allow_shrink:
            mutation_techniques.append(partial(gp.mutShrink))
 
        mutator = np.random.choice(mutation_techniques)
 
        unsuccesful_mutations = 0
        for _ in range(self._max_mut_loops):
            # We have to clone the individual because mutator operators work in-place.
            ind = self._toolbox.clone(individual)
            offspring, = mutator(ind)
            if str(offspring) not in self.evaluated_individuals_:
                # Update statistics
                # crossover_count is kept the same as for the predecessor
                # mutation count is increased by 1
                # predecessor is set to the string representation of the individual before mutation
                # generation is set to 'INVALID' such that we can recognize that it should be updated accordingly
                offspring.statistics['crossover_count'] = individual.statistics['crossover_count']
                offspring.statistics['mutation_count'] = individual.statistics['mutation_count'] + 1
                offspring.statistics['predecessor'] = (str(individual),)
                offspring.statistics['generation'] = 'INVALID'
                break
            else:
                unsuccesful_mutations += 1
 
        # Sometimes you have pipelines for which every shrunk version has already been explored too.
        # To still mutate the individual, one of the two other mutators should be applied instead.
        if ((unsuccesful_mutations == 50) and
                (type(mutator) is partial and mutator.func is gp.mutShrink)):
            offspring, = self._random_mutation_operator(individual, allow_shrink=False)
 
        return offspring,
```

### 4、交叉
选取两个pipeline，对里面的内容进行互换，但是两个pipeline的primitive要相同。

```python
def pick_two_individuals_eligible_for_crossover(population):
    """Pick two individuals from the population which can do crossover, that is, they share a primitive.
    Parameters
    ----------
    population: array of individuals
    Returns
    ----------
    tuple: (individual, individual)
        Two individuals which are not the same, but share at least one primitive.
        Alternatively, if no such pair exists in the population, (None, None) is returned instead.
    """
    primitives_by_ind = [set([node.name for node in ind if isinstance(node, gp.Primitive)])
                         for ind in population]
    pop_as_str = [str(ind) for ind in population]
 
    eligible_pairs = [(i, i+1+j) for i, ind1_prims in enumerate(primitives_by_ind)
                                 for j, ind2_prims in enumerate(primitives_by_ind[i+1:])
                                 if not ind1_prims.isdisjoint(ind2_prims) and
                                    pop_as_str[i] != pop_as_str[i+1+j]]
 
    # Pairs are eligible in both orders, this ensures that both orders are considered
    eligible_pairs += [(j, i) for (i, j) in eligible_pairs]
 
    if not eligible_pairs:
        # If there are no eligible pairs, the caller should decide what to do
        return None, None
 
    pair = np.random.randint(0, len(eligible_pairs))
    idx1, idx2 = eligible_pairs[pair]
 
    return population[idx1], population[idx2]
```

TPOT设置了一个阈值来决定是交叉还是变异，默认情况下变异的概率为0.9，交叉的概率为0.1.我们可以修改这个阈值。

具体的pipeline变换如下：

默认的种群数量为100，为了看源码方便，改为了5个种群数。交叉变异的概率各为0.5.

```python
tpot = TPOTClassifier(verbosity=2, max_time_mins=2,config_dict="TPOT light",population_size=5,mutation_rate=0.5,crossover_rate=0.5)
```

初始种群为5个pipeline，分别为：

```python
0 = {Individual} KNeighborsClassifier(PCA(input_matrix, PCA__iterated_power=6, PCA__svd_solver=randomized), KNeighborsClassifier__n_neighbors=79, KNeighborsClassifier__p=1, KNeighborsClassifier__weights=distance)
1 = {Individual} KNeighborsClassifier(Binarizer(input_matrix, Binarizer__threshold=0.55), KNeighborsClassifier__n_neighbors=84, KNeighborsClassifier__p=2, KNeighborsClassifier__weights=uniform)
2 = {Individual} DecisionTreeClassifier(input_matrix, DecisionTreeClassifier__criterion=gini, DecisionTreeClassifier__max_depth=9, DecisionTreeClassifier__min_samples_leaf=18, DecisionTreeClassifier__min_samples_split=20)
3 = {Individual} LogisticRegression(StandardScaler(input_matrix), LogisticRegression__C=0.0001, LogisticRegression__dual=True, LogisticRegression__penalty=l2)
4 = {Individual} GaussianNB(input_matrix)
```
经过第一轮交叉、变异，结果为：

```python
0 = {Individual} LogisticRegression(PCA(input_matrix, PCA__iterated_power=6, PCA__svd_solver=randomized), LogisticRegression__C=0.1, LogisticRegression__dual=True, LogisticRegression__penalty=l2)
1 = {Individual} KNeighborsClassifier(input_matrix, KNeighborsClassifier__n_neighbors=79, KNeighborsClassifier__p=1, KNeighborsClassifier__weights=distance)
2 = {Individual} KNeighborsClassifier(PCA(input_matrix, PCA__iterated_power=6, PCA__svd_solver=randomized), KNeighborsClassifier__n_neighbors=79, KNeighborsClassifier__p=1, KNeighborsClassifier__weights=distance)
3 = {Individual} GaussianNB(input_matrix)
4 = {Individual} KNeighborsClassifier(PCA(input_matrix, PCA__iterated_power=6, PCA__svd_solver=randomized), KNeighborsClassifier__n_neighbors=84, KNeighborsClassifier__p=1, KNeighborsClassifier__weights=distance)
```

然后对这几个pipeline进行打分评估，和上一轮pipeline一起选出score最高的前5个。

进入下一轮迭代。最后产生5个pipeline，打分，从中选出最优的那个：

```python
Best pipeline: KNeighborsClassifier(input_matrix, n_neighbors=10, p=2, weights=distance)
0.977777777778
```

流程举例如下（具体流程和TPOT代码本身略有区别，但是不影响对遗传算法自动化建模的理解）

![](2.png)

### 参考：

- [TPOT遗传算法](https://blog.csdn.net/hgy0403/article/details/81291307)
