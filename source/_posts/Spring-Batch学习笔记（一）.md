---
title: Spring Batch学习笔记（一）
date: 2021-03-19 11:09:52
tags: 
	- Spring Batch
	- 批处理

categories: 
	[后端,Java]

---

## Spring Batch 简介

Spring Batch 提供了可重复使用的功能，用来处理大量数据。包括记录、跟踪，事务管理，作业处理统计，作业重启，跳过和资源管理。
此外还提供了更高级的技术服务和功能，通过优化和分区技术实现极高容量和高性能的批处理作业。

## Spring Batch 使用场景

(1)一般的批处理程序

·从数据库，文件或者队列中读取大量数据
·处理读取到的数据
·将处理完成的数据保存到文件，数据库

(2)业务场景

·定期提交批处理
·并发批处理，并行处理 Job
·分阶段的企业消息驱动处理
·大规模并行批处理
·失败后手动或预定重启
·依赖步骤的顺序处理
·部分处理，跳过记录
·整批交易，适用于批量较小或现有存储过程/脚本的情况

## Spring Batch 主要组成部分

·JobRepository，用来注册 job 的容器
·JobLauncher，用来启动 job 的接口
·Job，实际执行的任务，包含一个或多个 Step
·Step，step 包含 ItemReader、ItemProcessor 和 ItemWriter
·ItemReader，用来读取数据的接口
·ItemProcessor，用来处理数据的接口
·ItemWriter，用来输出数据的接口
以上 Spring Batch 的主要组成部分只需要注册成 Spring 的 Bean 即可。批处理的配置类上需要使用@EnabelBatchProcessing。

## 代码

(1)监听器 JobListener

```java
 @Component
 public class JobListener implements JobExecutionListener{
  @Override
  public void beforeJob(JobExecution jobExecution){
   // Job执行前需要执行的操作
  }

  @Override
  public void afterJob(JobExecution jobExecution){
   // Job执行完成后需要执行的操作
  }
 }
```

(2)配置类 DataBatchConfiguration

```java
 @Configuration
 @EnableBatchProcessing
 public class DataBatchConfiguration{
  // 用于构建Job
  @Resource
  private JobBuilderFactory jobBuilderFactory;

  // 用于构建Step
  @Resource
  private StepBuilderFactory stepBuilderFactory;

  // 监听器
  @Resource
  private JobListener jobListener;

  // ItemReader(使用的其中一种读取方式)
  @Autowired
  private RepositoryItemReader readerData;

  // ItemWriter
  @Autowired
  private ItemReader writerData;

  // Job
  @Bean
  public Job dataHandleJob(){
   return jobBuilderFactory.get("dataHandleJob").incrementer(new RunIdIncrementer()).start(getDataStep())
    .listener(jobListener).build();
  }

  // Step
  // User:要处理的对象
  @Bean
  public Step getTDistSellOut() {
  return stepBuilderFactory.get("getData").<User, User>chunk(10000) // 一次commit数据的数量
    .faultTolerant().retryLimit(3)
    .retry(Exception.class)
    .skipLimit(100)            // 发生异常时，允许重试的次数
    .skip(Exception.class)
    .reader(readerData)              // reader
    .writer(writerData).build();        // writer
  }
 }
```

(3)读取类 ReaderStep

```java
 @Component
 public class ReaderStep{
  @Resource
  private EntityManagerFactory emf;

  @Autowired
  private UserRepository userRepository;

  @Bean RepositoryItemReader<User> readerData(){
   // 排序map(读取数据按照ID进行正序排列)
   Map<String,Sort.Direction> map = new HashMap<>();
   map.put("id",sort.Direction.ASC);
   // SQL语句所需参数LIST
   List<String> params = new ArrayList<>();
   params.add("2019-03-20");
   RepositoryItemReader<User> repositoryItemReader = new new RepositoryItemReader<>();
   // Set Repository
   repositoryItemReader.setRepository(userRepository);
   // Set PageSize(没有会报错)
   repositoryItemReader.setPageSize(5);
   // Set Repository Method
   repositoryItemReader.setMethodName("findByDateLike");
   // Set 参数List
   repositoryItemReader.setArguments(params);
   // Set 排序Map
   repositoryItemReader.setSort(map);
   return repositoryItemReader;
  }
 }
```

(4)写出类 WriterStep

```java
 @Component
 public class WriterStep {
  @Resource
  private UserRepository userRepository;

  @Bean
  public RepositoryItemWriter<User> writerData(){
   RepositoryItemWriter<User> repositoryItemWriter = new RepositoryItemWriter<>();
   repositoryItemWriter.setRepository(userRepository);
   repositoryItemWriter.setMethodName("save");
   return repositoryItemWriter;
  }
 }
```

## 参考网址

<https://www.cnblogs.com/ealenxie/p/9647703.html>
