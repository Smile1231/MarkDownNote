# `Spring`手动回滚事务

## 手动回滚事务

有时我们需要捕获一些错误信息，又需要进行事务回滚，这时我们就需要用到`Spring`提供的事务切面支持类`TransactionAspectSupport。`

```java
@Transactional(rollbackFor = Exception.class)
@Override
public void saveEntity() throws Exception{
    try {
        userDao.saveUser();
        studentDao.saveStudent();
    }catch (Exception e){
        System.out.println("异常了=====" + e);
        //手动强制回滚事务，这里一定要第一时间处理
        TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
    }
}
```
手动回滚事务一定要加上`@Transactional`，不然会报以下错误：
```java
org.springframework.transaction.NoTransactionException: No transaction aspect-managed TransactionStatus in scope
```
想想也是，不开启事务，何来手动回滚，所以`@Transactional`必不可少。


## 回滚部分异常

使用`Object savePoint = TransactionAspectSupport.currentTransactionStatus().createSavepoint()`; 设置回滚点。
使用`TransactionAspectSupport.currentTransactionStatus().rollbackToSavepoint(savePoint);` 回滚到`savePoint`。

```java
@Transactional(rollbackFor = Exception.class)
@Override
public void saveEntity() throws Exception{
    Object savePoint = null;
    try {
        userDao.saveUser();
        //设置回滚点
        savePoint = TransactionAspectSupport.currentTransactionStatus().createSavepoint();
        studentDao.saveStudent(); //执行成功
        int a = 10/0; //这里因为除数0会报异常,进入catch块
    }catch (Exception e){
        System.out.println("异常了=====" + e);
        //手工回滚异常
        TransactionAspectSupport.currentTransactionStatus().rollbackToSavepoint(savePoint);
    }
}
```

## `DataSourceTransactionManager`

`spring` 开启事务以及手动提交事务，可以在服务类上加上两个注解。
```java
@Autowired
DataSourceTransactionManager dataSourceTransactionManager;
@Autowired
TransactionDefinition transactionDefinition;
```
> 手动开启事务

`TransactionStatus transactionStatus = dataSourceTransactionManager.getTransaction(transactionDefinition);`

> 手动提交事务

`dataSourceTransactionManager.commit(transactionStatus);`//提交

> 手动回滚事务

`dataSourceTransactionManager.rollback(transactionStatus);`//最好是放在`catch` 里面,防止程序异常而事务一直卡在哪里未提交



