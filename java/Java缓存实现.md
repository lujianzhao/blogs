### 问题背景
由于报表需要长时间的查询展现，无疑是个致命的客户体验。

### 初步思路
用`key-value`数据库`redis`做缓存，查询过的结果缓存到`redis`中，同样的条件查询时直接去`redis`中读取缓存数据展现即可。
初步想法：使用`Java`自定义注解实现，通过`Spring-AOP`处理缓存逻辑。

自定义注解
```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface AutoCache {

}
```

Spring-AOP实现
```java
@Aspect
@Component
public class SystemCacheAspect {

    @Resource
    private JedisPool jedisPool; 

    @Pointcut("@annotation(com.cache.annotation.AutoCache)")
    public void pointCut() {

    }

    @Before("pointCut()")
    public void doBefore(JoinPoint jp) {
        System.out.println("doBefore..." + jp);
    }

    @Around(value="pointCut()")
    public Object doAround(ProceedingJoinPoint pjp) {
        System.out.println(pjp.getKind());
        System.out.println(pjp.getTarget());
        System.out.println(pjp.getThis());
        System.out.println(pjp.getClass());
        System.out.println(pjp.getSignature().getDeclaringTypeName());
        System.out.println(pjp.getSignature().getName());
        System.out.println(pjp.getSourceLocation());
        System.out.println(pjp.getStaticPart());

        System.out.println("doAround start ");
        Object result = null;
        try {
            System.out.println();
            System.out.println(genearteCacheKey(pjp.getArgs()));

            System.out.println();

            result = pjp.proceed();
        } catch (Throwable e) {
            result = "{\"total\":0,\"rows\":[]}";
            e.printStackTrace();
        }
        System.out.println("doAround end " + result);
        return result;
    }

    /**
     * 根据参数值生成唯一缓存key
     * @方法名:genearteCacheKey 
     * @参数 @param objs
     * @参数 @return
     * @参数 @throws Exception  
     * @返回类型 String
     */
    private String genearteCacheKey(Object[] objs) throws Exception {
        StringBuffer cacheKey = new StringBuffer();
        if (null != objs && objs.length > 0) {
            for (Object obj : objs) {
                BeanInfo beanInfo = Introspector.getBeanInfo(obj.getClass());
                PropertyDescriptor[] properties = beanInfo.getPropertyDescriptors();
                for (PropertyDescriptor desc : properties) {
                    if (!StringUtils.equals("class", desc.getName())) {
                        String value = BeanUtils.getProperty(obj,
                                desc.getName());
                        if (StringUtils.isNotBlank(value)) {
                            cacheKey.append(value);
                        }
                    }
                }
            }
        }

        return cacheKey.toString();
    }

    @After("pointCut()")
    public void doAfter(JoinPoint jp) {
        System.out.println("doAfter..." + jp);
    }

    @AfterReturning(pointcut="pointCut()", argNames="result", returning="result")
    public void doAfterReturn(JoinPoint jp, Object reuslt) {
        System.out.println("AfterReturning resutl :" + reuslt);
    }

    @AfterThrowing(value="pointCut()", throwing="e")
    public void doAfterThrowing(JoinPoint jp, Throwable e) {
        System.out.println("doAfterThrowing..." + jp);
    }
}
```

Spring配置文件
```xml
<!-- 缓存AOP配置 -->
<aop:aspectj-autoproxy proxy-target-class="true"/>
```
碰到问题如下：
* 缺少jar包：aspectjrt-1.6.9.jar，aspectjweaver-1.6.9.jar，去http://cn.jarfire.org/ 查找下载即可
* @AfterReturning写法：
```java
@AfterReturning正确写法，3个必须都要有
@AfterReturning(pointcut="pointCut()", argNames="result", returning="result")
缺少returning="result"错误如下：
java.lang.IllegalArgumentException: error at ::0 formal unbound in pointcut 
缺少argNames="result"错误如下：
java.lang.IllegalStateException: Returning argument name 'result' was not bound in advice arguments
```

AOP实现类中的执行顺序：
```java
// 未抛出Exception时
doAround start 
doBefore...execution(Object com.power.bank.khjldj.cl.action.OperateItemAction.getOperateItemInfo(ColumnParamBean))
doAround end {"total":0,"rows":[]}
doAfter...execution(Object com.power.bank.khjldj.cl.action.OperateItemAction.getOperateItemInfo(ColumnParamBean))
AfterReturning resutl :{"total":0,"rows":[]}
```

// 抛出Exception时
```java
doAround start 
doBefore...execution(Object com.power.bank.khjldj.cl.action.OperateItemAction.getOperateItemInfo(ColumnParamBean))
doAfter...execution(Object com.power.bank.khjldj.cl.action.OperateItemAction.getOperateItemInfo(ColumnParamBean))
doAfterThrowing...execution(Object com.power.bank.khjldj.cl.action.OperateItemAction.getOperateItemInfo(ColumnParamBean))
```