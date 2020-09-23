# 一、IDEA

## 1、修改代码运行不生效问题

![image-20200917101714467](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200917101714467.png)

# 

## 2、配置Docker插件一键部署

### （1）、在服务器上修改docker.service,开启2375端口

```
vim /usr/lib/systemd/system/docker.service
```



### （2）、在ExecStart末尾增加 ：按"i"插入，按"Esc"退出，输入":wq" 完成文件写入

```
# 允许所有客户端连接
-H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

![image-20200915142014343](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142014343.png)



### （3）、重新加载配置文件和重启Docker

```
systemctl daemon-reload 
systemctl restart docker 
```



### （4）、查看进程

```
netstat -tulp
```

![image-20200915141919627](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915141919627.png)

```
netstat -nplt |grep 2375
```

![image-20200915141929116](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915141929116.png)



### （5）、防火墙开放2375端口/重启防火墙

```
firewall-cmd --zone=public --add-port=2375/tcp --permanent
firewall-cmd --reload
```

<!--*注意：虚拟机或与服务器的安全组也要开放2375端口，否者依然无法访问！*-->



### （6）、IDEA安装docker插件

![image-20200915142625759](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142625759.png)



### （7）、配置连接ip和port

![image-20200915142824418](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142824418.png)

### （8）、编写Dockfile放在项目根目录下

```
FROM java:8
VOLUME /tmp
COPY target/k12_classroom-0.0.1-SNAPSHOT.jar k12_classroom.jar
RUN bash -c "touch /k12_classroom.jar"
EXPOSE 9001
ENTRYPOINT ["java","-jar","k12_classroom.jar"]
```



### （9）、配置Run/Debug Configurations



![image-20200917103425755](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200917103425755.png)

![image-20200917111017524](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200917111017524.png)



### （10）、一键编译部署

![image-20200917111141795](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200917111141795.png)



# 二、CODE

## 1、反序列化获取泛型参数类型

### （1）Jackson

```
com.fasterxml.jackson.core.type.TypeReference


public abstract class TypeReference<T> implements Comparable<TypeReference<T>> {
        protected final Type _type;

        protected TypeReference() {
            Type superClass = getClass().getGenericSuperclass();
            if (superClass instanceof Class<?>) { // sanity check, should never happen
                throw new IllegalArgumentException("Internal error: TypeReference constructed without actual type information");
            }
            /* 22-Dec-2008, tatu: Not sure if this case is safe -- I suspect
             *   it is possible to make it fail?
             *   But let's deal with specific
             *   case when we know an actual use case, and thereby suitable
             *   workarounds for valid case(s) and/or error to throw
             *   on invalid one(s).
             */
            _type = ((ParameterizedType) superClass).getActualTypeArguments()[0];
        }

        public Type getType() {
            return _type;
        }

        /**
         * The only reason we define this method (and require implementation
         * of <code>Comparable</code>) is to prevent constructing a
         * reference without type information.
         */
        @Override
        public int compareTo(TypeReference<T> o) {
            return 0;
        }
        // just need an implementation, not a good one... hence ^^^
    }
```

### （2）Fastjson

```
com.alibaba.fastjson.TypeReference

static ConcurrentMap<Type, Type> classTypeCache
            = new ConcurrentHashMap<Type, Type>(16, 0.75f, 1);

    protected final Type type;

    /**
     * Constructs a new type literal. Derives represented class from type
     * parameter.
     *
     * <p>Clients create an empty anonymous subclass. Doing so embeds the type
     * parameter in the anonymous class's type hierarchy so we can reconstitute it
     * at runtime despite erasure.
     */
    protected BaseCallback(){
        Type superClass = getClass().getGenericSuperclass();

        Type type = ((ParameterizedType) superClass).getActualTypeArguments()[0];

        Type cachedType = classTypeCache.get(type);
        if (cachedType == null) {
            classTypeCache.putIfAbsent(type, type);
            cachedType = classTypeCache.get(type);
        }

        this.type = cachedType;
    }

    /**
     * @since 1.2.9
     * @param actualTypeArguments
     */
    protected BaseCallback(Type... actualTypeArguments){
        Class<?> thisClass = this.getClass();
        Type superClass = thisClass.getGenericSuperclass();

        ParameterizedType argType = (ParameterizedType) ((ParameterizedType) superClass).getActualTypeArguments()[0];
        Type rawType = argType.getRawType();
        Type[] argTypes = argType.getActualTypeArguments();

        int actualIndex = 0;
        for (int i = 0; i < argTypes.length; ++i) {
            if (argTypes[i] instanceof TypeVariable &&
                    actualIndex < actualTypeArguments.length) {
                argTypes[i] = actualTypeArguments[actualIndex++];
            }
            // fix for openjdk and android env
            if (argTypes[i] instanceof GenericArrayType) {
                argTypes[i] = TypeUtils.checkPrimitiveArray(
                        (GenericArrayType) argTypes[i]);
            }

            // 如果有多层泛型且该泛型已经注明实现的情况下，判断该泛型下一层是否还有泛型
            if(argTypes[i] instanceof ParameterizedType) {
                argTypes[i] = handlerParameterizedType((ParameterizedType) argTypes[i], actualTypeArguments, actualIndex);
            }
        }

        Type key = new ParameterizedTypeImpl(argTypes, thisClass, rawType);
        Type cachedType = classTypeCache.get(key);
        if (cachedType == null) {
            classTypeCache.putIfAbsent(key, key);
            cachedType = classTypeCache.get(key);
        }

        type = cachedType;

    }

    private Type handlerParameterizedType(ParameterizedType type, Type[] actualTypeArguments, int actualIndex) {
        Class<?> thisClass = this.getClass();
        Type rawType = type.getRawType();
        Type[] argTypes = type.getActualTypeArguments();

        for(int i = 0; i < argTypes.length; ++i) {
            if (argTypes[i] instanceof TypeVariable && actualIndex < actualTypeArguments.length) {
                argTypes[i] = actualTypeArguments[actualIndex++];
            }

            // fix for openjdk and android env
            if (argTypes[i] instanceof GenericArrayType) {
                argTypes[i] = TypeUtils.checkPrimitiveArray(
                        (GenericArrayType) argTypes[i]);
            }

            // 如果有多层泛型且该泛型已经注明实现的情况下，判断该泛型下一层是否还有泛型
            if(argTypes[i] instanceof ParameterizedType) {
                return handlerParameterizedType((ParameterizedType) argTypes[i], actualTypeArguments, actualIndex);
            }
        }

        Type key = new ParameterizedTypeImpl(argTypes, thisClass, rawType);
        return key;
    }

    /**
     * Gets underlying {@code Type} instance.
     */
    public Type getType() {
        return type;
    }

```

### （2）Gson

```
com.google.gson.reflect.TypeToken

/**
* Returns the type from super class's type parameter in {@link $Gson$Types#canonicalize
* canonical form}.
*/
static Type getSuperclassTypeParameter(Class<?> subclass) {
Type superclass = subclass.getGenericSuperclass();
if (superclass instanceof Class) {
    throw new RuntimeException("Missing type parameter.");
}
ParameterizedType parameterized = (ParameterizedType) superclass;
return $Gson$Types.canonicalize(parameterized.getActualTypeArguments()[0]);
}
```



## 2、常用单例模式

### （1）静态内部类加载-线程安全

```

public class SingletonDemo {
    private static class SingletonHolder{
        private static SingletonDemo instance=new SingletonDemo();
    }
    private SingletonDemo(){
        System.out.println("Singleton has loaded");
    }
    public static SingletonDemo getInstance(){
        return SingletonHolder.instance;
    }
}
使用内部类的好处是，静态内部类不会在单例加载时就加载，而是在调用getInstance()方法时才进行加载，达到了类似懒汉模式的效果，而这种方法又是线程安全的。
```



### （2）枚举方法-线程安全

```
enum SingletonDemo{
    INSTANCE;
    public void otherMethods(){
        System.out.println("Something");
    }
}

运用：
SingletonDemo.INSTANCE.otherMethods();

特点：
简洁，无偿地提供了串行化机制，绝对防止对此实例化，即使是在面对复杂的串行化或者反射攻击的时候。虽然这中方法还没有广泛采用，但是单元素的枚举类型已经成为实现Singleton的最佳方法。
```

### （3）双重校验锁+volatile关键字

```
public class Singleton{
    private volatile static Singleton singleton = null;
    private Singleton()  {    }
    public static Singleton getInstance()   {
        if (singleton== null)  {
            synchronized (Singleton.class) {
                if (singleton== null)  {
                    singleton= new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

