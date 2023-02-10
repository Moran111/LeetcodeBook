# Singleton

ways to create an Object, this class only have one instance

需要频繁的进行创建和销毁的对象，创建对象时耗时过多或者耗费过多资源，单用经常用到的对象，工具类对象，频繁访问数据库或者文件的对象

## Method 1: Using Static Variable to Create a Singlton Class (Eager Loading)

```
class Singleton {
    // 1. private constructor, cannot new it outside
    private Singleton() {
    }
    // 2. create a new instance inside this class
    private final static Singleton instance = new Singleton();

    // 3. private a static method, return this instance
    public static Singleton getInstance() {
        return instance;
    }
}
```

```
class Singleton {
    // 1. private constructor, cannot new it outside
    private Singleton() {
    }
    
    private static Singleton instance;
    
    static { //在静态代码块中，创建singlton对象
        instance = new Singleton();
    }

    // 3. private a static method, return this instance
    public static Singleton getInstance() {
        return instance;
    }
}
```

对程序本身没有问题，可以用。但是如果你这么写了却没有用这个的话，会造成内存浪费&#x20;

thread safe but not acheive lazy loading&#x20;

Pros: easy to implement, initiate the instance of the class when class loading. That will avoid the multithreading problem (**thread safe**)

Corns: because of that, it cannot achieve **lazy loading**. We don’t call getInstance() but this class has already been initialized. So waste memory.&#x20;

## Method 2:  lazy loading&#x20;

**not thread safe**

```
class Singleton { 
    private static Singleton instance;
    
    private Singleton() {
    }

    // 提供一个静态的共有方法，当时用到该方法时，才去创建instance
    public static Singleton getInstance() {
        if (instance == null) {         -- multithread go into here
            instance = new Singleton();
        }
        return instance;
    }
}
```

only when we call getInstance(), we will create a new instance of it, it is called lazy loading

Pros: lazy loading, only can be uased in single thread

why? when one thread go into if statement, run instance = new Singleton(). before return, another thread also go into if statement. Then two instance will be created.&#x20;

&#x20;实际开发中不要使用这种模式

**thread safe 1 (synchronized getInstance() method)**

```
class Singleton { 
    private static Singleton instance;
    
    private Singleton() {
    }

    // 提供一个静态的共有方法，当时用到该方法时，才去创建instance
    // thread will wait at this method
    // 只有一个thread可以进来执行这个方法
    public static synchronized Singleton getInstance() {
        if (instance == null) {         
            instance = new Singleton();
        }
        return instance;
    }
}
```

corns: 效率低，肯定会比较多的用getInstance() 方法

实际开发中不要使用这种模式,效率低, 底下这个不能保证thread safe

```
class Singleton { 
    private static Singleton instance;
    
    private Singleton() {
    }

    public static synchronized Singleton getInstance() {
        if (instance == null) {   
           // 有人把这个放在if里面，但是这样并没有什么用，not thread safe
           // 因为只要2 threads进入if了，那么就会thread1执行完，建立一个新的，
           // 然后thread2接着执行，再搞一个新的
            synchorized (Singleton.class) {
                instance = new Singleton();
            }      
        }
        return instance;
    }
}
```

这个不能用

**thread safe 2**

lazy loading + thread safe,可以保证效率, 双重检查代

```
volatile: 当instance的值改了，立刻更新
```

```
class Singleton { 
    private static volatile Singleton instance;
    
    private Singleton() {
    }

    public static synchronized Singleton getInstance() {
        if (instance == null) { // thread a here , thread b here 
            synchorized (Singleton.class) { // a go into this block, new instance(), finished. then b go into this block, will check if instance == null
                if (instance == null) {
                    instance = new Singleton();
                }
            }      
        }
        return instance;
    }
}
```

推荐使用

## Method 3: 静态内部类

1. 外部类被装载的时候，静态内部类不会立即被实力话, lazy loading
2. 在我们加载静态内部类的时候，这个才会被new instance，这个线程是安全的
3. lazy loading + thread safe

```
class Singleton { 
    private static Singleton instance;
    
    // private constructor -> avoid others new it
    private Singleton() {
    }
    
    // static inner class with a static field
    private static class SingletonInstance {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    //提供一个静态的共有方法，直接返回SingletonInstance.INSTANCE
    public static Singleton getInstance() {
        return SingletonInstance.INSTANCE;
    }
}
```

when we call getInstance() -> class load static inner class SingltonInstance -> so this is lazy loading&#x20;

It is thread safe when JVM do class loading (JVM提供的机制保证Singleton INSTANCE = new Singleton()是安全的）

推荐使用

## Method 4: Using Enum to implement it

使用枚举 可以避免多线程，而且可以推荐反序列话重新创建新的对象

```
enum Singleton {
    INSTANCE; //attribute
}

// use
Singleton instance = Singleton.INSTANCE;
Singleton instance2 = Singleton.INSTANCE;

```

推荐使用
