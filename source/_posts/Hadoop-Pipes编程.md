---
title: Hadoop Pipes编程
date: 2023-12-10 14:14:05
tags: Hadoop
categories: Hadoop Pipes
---
# 1、Hadoop Pipes简介

Hadoop Pipes是Hadoop MapReduce的C++接口代称。不同于使用标准输入和输出来实现的map代码和reduce代码之间的Streaming编程，Pipes使用Socket作为TaskTracker与C++进程之间数据传输的通道，数据传输为字节流。

# 2、Hadoop Pipes编程初探

Hadoop Pipes可供开发者编写RecordReader、Mapper、Partitioner、Reducer、RecordWriter五个组件，当然，也可以自定义Combiner。

WordCount.cc 示例，也可以参考该git项目https://github.com/alexanderkoumis/hadoop-wordcount-cpp/tree/master：

<!--more-->

```c++
#include "/usr/local/hadoop/include/Pipes.hh"
#include "/usr/local/hadoop/include/TemplateFactory.hh"
#include "/usr/local/hadoop/include/StringUtils.hh"

const std::string WORDCOUNT = "WORDCOUNT";
const std::string INPUT_WORDS = "INPUT_WORDS";
const std::string OUTPUT_WORDS = "OUTPUT_WORDS";

class WordCountMap: public HadoopPipes::Mapper {
public:
  HadoopPipes::TaskContext::Counter* inputWords;
  
  WordCountMap(HadoopPipes::TaskContext& context) {
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);
  }
  
  void map(HadoopPipes::MapContext& context) {
    std::vector<std::string> words = 
      HadoopUtils::splitString(context.getInputValue(), " ");
    for(unsigned int i=0; i < words.size(); ++i) {
      context.emit(words[i], "1");
    }
    context.incrementCounter(inputWords, words.size());
  }
};

class WordCountReduce: public HadoopPipes::Reducer {
public:
  HadoopPipes::TaskContext::Counter* outputWords;

  WordCountReduce(HadoopPipes::TaskContext& context) {
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);
  }

  void reduce(HadoopPipes::ReduceContext& context) {
    int sum = 0;
    while (context.nextValue()) {
      sum += HadoopUtils::toInt(context.getInputValue());
    }
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));
    context.incrementCounter(outputWords, 1); 
  }
};
int main(int argc, char *argv[]) {
  return HadoopPipes::runTask(HadoopPipes::TemplateFactory<WordCountMap, WordCountReduce>());
}
```

注意与hadoop pipes 相关的文件放在了目录：
```shell
/usr/local/hadoop/include/
```

主要的文件为Pipes.hh，该头文件定义了一些抽象类，除去开发者需要编写的五大组件之外，还有JobConf、TaskContext、Closeable、Factory四个。

```c++
/**
 * Task context provides the information about the task and job.
 */
class TaskContext {
public:
  /**
   * Counter to keep track of a property and its value.
   */
  class Counter {
  private:
    int id;
  public:
    Counter(int counterId) : id(counterId) {}
    Counter(const Counter& counter) : id(counter.id) {}

    int getId() const { return id; }
  };

  /**
   * Get the JobConf for the current task.
   */
  virtual const JobConf* getJobConf() = 0;

  /**
   * Get the current key.
   * @return the current key
   */
  virtual const std::string& getInputKey() = 0;

  /**
   * Get the current value.
   * @return the current value
   */
  virtual const std::string& getInputValue() = 0;

  /**
   * Generate an output record
   */
  virtual void emit(const std::string& key, const std::string& value) = 0;

  /**
   * Mark your task as having made progress without changing the status
   * message.
   */
  virtual void progress() = 0;

  /**
   * Set the status message and call progress.
   */
  virtual void setStatus(const std::string& status) = 0;

  /**
   * Register a counter with the given group and name.
   */
  virtual Counter*
    getCounter(const std::string& group, const std::string& name) = 0;

  /**
   * Increment the value of the counter with the given amount.
   */
  virtual void incrementCounter(const Counter* counter, uint64_t amount) = 0;

  virtual ~TaskContext() {}
};


class MapContext: public TaskContext {
public:

  /**
   * Access the InputSplit of the mapper.
   */
  virtual const std::string& getInputSplit() = 0;

  /**
   * Get the name of the key class of the input to this task.
   */
  virtual const std::string& getInputKeyClass() = 0;

  /**
   * Get the name of the value class of the input to this task.
   */
  virtual const std::string& getInputValueClass() = 0;

};

class ReduceContext: public TaskContext {
public:
  /**
   * Advance to the next value.
   */
  virtual bool nextValue() = 0;
};

```

JobConf：开发者可以通过获得任务的属性
```c++
class JobConf {  
public:  
  virtual bool hasKey(const std::string& key) const = 0;  
  virtual const std::string& get(const std::string& key) const = 0;  
  virtual int getInt(const std::string& key) const = 0;  
  virtual float getFloat(const std::string& key) const = 0;  
  virtual bool getBoolean(const std::string&key) const = 0;  
  virtual ~JobConf() {}  
};
```

Closeable：这个抽象类Mapper、Reducer、RecordReader、RecordWriter的基类，只有两个方法，一个close()，一个析构函数。

```c++
class Closable {
public:
  virtual void close() {}
  virtual ~Closable() {}
};

/**
 * The application's mapper class to do map.
 */
class Mapper: public Closable {
public:
  virtual void map(MapContext& context) = 0;
};

/**
 * The application's reducer class to do reduce.
 */
class Reducer: public Closable {
public:
  virtual void reduce(ReduceContext& context) = 0;
};

/**
 * For applications that want to read the input directly for the map function
 * they can define RecordReaders in C++.
 */
class RecordReader: public Closable {
public:
  virtual bool next(std::string& key, std::string& value) = 0;

  /**
   * The progress of the record reader through the split as a value between
   * 0.0 and 1.0.
   */
  virtual float getProgress() = 0;
};

/**
 * An object to write key/value pairs as they are emited from the reduce.
 */
class RecordWriter: public Closable {
public:
  virtual void emit(const std::string& key,
                    const std::string& value) = 0;
};
```

Factory：一个抽象工厂，用来创建各个组件，是模板工厂的基类，具体的可以参见TemplateFactory.hh。开发者在调用runTask时，创建相应的Factory传入即可。
```c++
/**
 * A factory to create the necessary application objects.
 */
class Factory {
public:
  virtual Mapper* createMapper(MapContext& context) const = 0;
  virtual Reducer* createReducer(ReduceContext& context) const = 0;

  /**
   * Create a combiner, if this application has one.
   * @return the new combiner or NULL, if one is not needed
   */
  virtual Reducer* createCombiner(MapContext& context) const {
    return NULL;
  }

  /**
   * Create an application partitioner object.
   * @return the new partitioner or NULL, if the default partitioner should be
   *     used.
   */
  virtual Partitioner* createPartitioner(MapContext& context) const {
    return NULL;
  }

  /**
   * Create an application record reader.
   * @return the new RecordReader or NULL, if the Java RecordReader should be
   *    used.
   */
  virtual RecordReader* createRecordReader(MapContext& context) const {
    return NULL;
  }

  /**
   * Create an application record writer.
   * @return the new RecordWriter or NULL, if the Java RecordWriter should be
   *    used.
   */
  virtual RecordWriter* createRecordWriter(ReduceContext& context) const {
    return NULL;
  }

  virtual ~Factory() {}
};

/**
 * Run the assigned task in the framework.
 * The user's main function should set the various functions using the
 * set* functions above and then call this.
 * @return true, if the task succeeded.
 */
bool runTask(const Factory& factory);

}
```

# 3、Hadoop Pipes编程
有了以上的基础知识，就可以开始编写MapReduce任务了。我们可以直接从examples着手，先来看看wordcount.cc。
```c++
// wordcount-simple.cc -> Mapper & Reducer
class WordCountMap: public HadoopPipes::Mapper {  
public:  
  HadoopPipes::TaskContext::Counter* inputWords;  

  WordCountMap(HadoopPipes::TaskContext& context) {  
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);  
  }  

  void map(HadoopPipes::MapContext& context) {  
    std::vector<std::string> words =   
      HadoopUtils::splitString(context.getInputValue(), " ");  
    for(unsigned int i=0; i < words.size(); ++i) {  
      context.emit(words[i], "1");  
    }  
    context.incrementCounter(inputWords, words.size());  
  }  
};  

class WordCountReduce: public HadoopPipes::Reducer {  
public:  
  HadoopPipes::TaskContext::Counter* outputWords;  

  WordCountReduce(HadoopPipes::TaskContext& context) {  
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);  
  }  

  void reduce(HadoopPipes::ReduceContext& context) {  
    int sum = 0;  
    while (context.nextValue()) {  
      sum += HadoopUtils::toInt(context.getInputValue());  
    }  
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));  
    context.incrementCounter(outputWords, 1);   
  }  
};
```  

该任务编写了两个主要组件，mapper与reducer。要实现这两个组件需要继承相应的基类。基类声明如下：

```c++
// wordcount-simple.cc -> Mapper & Reducer
class WordCountMap: public HadoopPipes::Mapper {  
public:  
  HadoopPipes::TaskContext::Counter* inputWords;  

  WordCountMap(HadoopPipes::TaskContext& context) {  
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);  
  }  

  void map(HadoopPipes::MapContext& context) {  
    std::vector<std::string> words =   
      HadoopUtils::splitString(context.getInputValue(), " ");  
    for(unsigned int i=0; i < words.size(); ++i) {  
      context.emit(words[i], "1");  
    }  
    context.incrementCounter(inputWords, words.size());  
  }  
};  

class WordCountReduce: public HadoopPipes::Reducer {  
public:  
  HadoopPipes::TaskContext::Counter* outputWords;  

  WordCountReduce(HadoopPipes::TaskContext& context) {  
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);  
  }  

  void reduce(HadoopPipes::ReduceContext& context) {  
    int sum = 0;  
    while (context.nextValue()) {  
      sum += HadoopUtils::toInt(context.getInputValue());  
    }  
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));  
    context.incrementCounter(outputWords, 1);   
  }  
};
```  

该任务编写了两个主要组件，mapper与reducer。要实现这两个组件需要继承相应的基类。基类声明如下：

```c++
// wordcount-simple.cc -> Mapper & Reducer
class WordCountMap: public HadoopPipes::Mapper {  
public:  
  HadoopPipes::TaskContext::Counter* inputWords;  

  WordCountMap(HadoopPipes::TaskContext& context) {  
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);  
  }  

  void map(HadoopPipes::MapContext& context) {  
    std::vector<std::string> words =   
      HadoopUtils::splitString(context.getInputValue(), " ");  
    for(unsigned int i=0; i < words.size(); ++i) {  
      context.emit(words[i], "1");  
    }  
    context.incrementCounter(inputWords, words.size());  
  }  
};  

class WordCountReduce: public HadoopPipes::Reducer {  
public:  
  HadoopPipes::TaskContext::Counter* outputWords;  

  WordCountReduce(HadoopPipes::TaskContext& context) {  
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);  
  }  

  void reduce(HadoopPipes::ReduceContext& context) {  
    int sum = 0;  
    while (context.nextValue()) {  
      sum += HadoopUtils::toInt(context.getInputValue());  
    }  
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));  
    context.incrementCounter(outputWords, 1);   
  }  
};
```  

该任务编写了两个主要组件，mapper与reducer。要实现这两个组件需要继承相应的基类。基类声明如下：

```c++
// wordcount-simple.cc -> Mapper & Reducer
class WordCountMap: public HadoopPipes::Mapper {  
public:  
  HadoopPipes::TaskContext::Counter* inputWords;  

  WordCountMap(HadoopPipes::TaskContext& context) {  
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);  
  }  

  void map(HadoopPipes::MapContext& context) {  
    std::vector<std::string> words =   
      HadoopUtils::splitString(context.getInputValue(), " ");  
    for(unsigned int i=0; i < words.size(); ++i) {  
      context.emit(words[i], "1");  
    }  
    context.incrementCounter(inputWords, words.size());  
  }  
};  

class WordCountReduce: public HadoopPipes::Reducer {  
public:  
  HadoopPipes::TaskContext::Counter* outputWords;  

  WordCountReduce(HadoopPipes::TaskContext& context) {  
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);  
  }  

  void reduce(HadoopPipes::ReduceContext& context) {  
    int sum = 0;  
    while (context.nextValue()) {  
      sum += HadoopUtils::toInt(context.getInputValue());  
    }  
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));  
    context.incrementCounter(outputWords, 1);   
  }  
};
```  

该任务编写了两个主要组件，mapper与reducer。要实现这两个组件需要继承相应的基类。基类声明如下：

```c++
class WordCountMap: public HadoopPipes::Mapper {
public:
  HadoopPipes::TaskContext::Counter* inputWords;
  
  WordCountMap(HadoopPipes::TaskContext& context) {
    inputWords = context.getCounter(WORDCOUNT, INPUT_WORDS);
  }
  
  void map(HadoopPipes::MapContext& context) {
    std::vector<std::string> words = 
      HadoopUtils::splitString(context.getInputValue(), " ");
    for(unsigned int i=0; i < words.size(); ++i) {
      context.emit(words[i], "1");
    }
    context.incrementCounter(inputWords, words.size());
  }
};

class WordCountReduce: public HadoopPipes::Reducer {
public:
  HadoopPipes::TaskContext::Counter* outputWords;

  WordCountReduce(HadoopPipes::TaskContext& context) {
    outputWords = context.getCounter(WORDCOUNT, OUTPUT_WORDS);
  }

  void reduce(HadoopPipes::ReduceContext& context) {
    int sum = 0;
    while (context.nextValue()) {
      sum += HadoopUtils::toInt(context.getInputValue());
    }
    context.emit(context.getInputKey(), HadoopUtils::toString(sum));
    context.incrementCounter(outputWords, 1); 
  }
};
```
该任务编写了两个主要组件，mapper与reducer。要实现这两个组件需要继承相应的基类，继承了相应的基类，就可以大胆的通过context获得key/value实现自己的逻辑了，结果处理完毕后，需要通过context.emit(key, value)将结果发送到下一阶段。

注：

1. 由于Factory创建对象需要传入Context对象，所以还需要实现一个构造函数，参数为TaskContext。

2. Hadoop Pipes内部规定，map与reduce的key/value均为Text类型，在C++中表现为string类型。不过，Hadoop还是做得比较贴心，有专门的方法负责处理string，具体可以参见StringUtils.hh。

3. Counter可以称之为统计器，可供开发者统计一些需要的数据，如读入行数、处理字节数等。任务完毕后，可以在web控制参看结果。

```c++
// wordcount-part.cc -> Partitioner
class WordCountPartitioner: public HadoopPipes::Partitioner {
    public:
    WordCountPartitioner(HadoopPipes::TaskContext& context){}
         virtual int partition(const std::string& key, int numOfReduces) {   
           return 0;
        }
};
```
该实例在提供简单Mapper与Reducer方法的同时，还提供了Partitioner，实例实现较为简单，直接返回了第一个reduce位置。开发者自定义的Partitioner同mapper/reducer一致，需要继承其基类HadoopPipes:: Partitioner，也需要提供一个传入TaskContext的构造函数，它的声明如下：
```c++
class Partitioner {   
   public:   
    virtual int partition(const std::string& key, int numOfReduces) = 0;   
    virtual ~Partitioner() {}   
};
```
Partitioner编写方法与Java的一致，对于partition方法，框架会自动为它传入两个参数，分别为key值和reduce task的个数numOfReduces，用户只需返回一个0~ numOfReduces-1的值即可。

RecordReader/RecordWriter实现较长，这里就不贴了，贴一下这俩的基类：
```c++
class RecordReader: public Closable {   
   public:   
   virtual bool next(std::string& key, std::string& value) = 0;   4.   // 读进度   
      virtual float getProgress() = 0;   
};   
class RecordWriter: public Closable {   
   public:   
   virtual void emit(const std::string& key,
                     const std::string& value) = 0;
};
```
对于RecordReader，用户自定义的构造函数需携带类型为HadoopPipes::MapContext的参数（而不能是TaskContext），通过该参数的getInputSplit()的方法，用户可以获取经过序列化的InpuSplit对象，Java端采用不同的InputFormat可导致InputSplit对象格式不同，但对于大多数InpuSplit对象，它们可以提供至少三个信息：当前要处理的InputSplit所在的文件名，所在文件中的偏移量，它的长度。用户获取这三个信息后，可使用libhdfs库读取文件，以实现next方法。

用户自定的RecordWriter的构造函数需携带参数TaskContext，通过该参数的getJobConf()可获取一个HadoopPipes::JobConf的对象，用户可从该对象中获取该reduce task的各种参数，如：该reduce task的编号（这对于确定输出文件名有用），reduce task的输出目录等。同时实现emit方法，将数据写入文件。

# 4、Hadoop Pipes任务提交
Hadoop Pipes任务提交命令根据Hadoop版本而不一，主体的命令有如下：
```shell
hadoop pipes [-conf ] [-D , , …] [-input ] [-output ] [-jar ] [-inputformat ] [-map ] [-partitioner ] [-reduce ] [-writer ] [-program ]

示例：hadoop pipes -conf word.xml -input input -output output
```

具体可以参考[《Hadoop实战第2版》——3.5节Hadoop Pipes](https://developer.aliyun.com/article/174041)

# 5、小结

本篇博文简要了说了一下Hadoop Pipes的使用方法。

在这里贴一下快手大佬董西成的优化意见：为了提高系能，RecordReader和RecordWriter最好采用Java代码实现（或者重用Hadoop中自带的），这是因为Hadoop自带的C++库libhdfs采用JNI实现，底层还是要调用Java相关接口，效率很低，此外，如果要处理的文件为二进制文件或者其他非文本文件，libhdfs可能不好处理。