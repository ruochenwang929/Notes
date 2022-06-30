# Java集合

## List，Set，Map区别

**List：** 存储的元素有序，可重复

**Set：** 存储的元素无序，不可重复

**Map：** 使用键值对存储，Key无序且不可重复，Value无序可重复，每个键最多映射到一个值

## ArrayList和LinkedList区别

### 线程安全

两者都是非线程安全

### 底层数据结构

ArrayList使用Object数组；LinkedList使用双向链表（JDK1.6之前为循环链表，JDK1.7取消了循环）

### 是否支持快速随机访问

ArrayList支持（起始地址+偏移量*元素大小），LinkedList不支持

### 内存占用

ArrayList空间浪费主要体现在list列表结尾会预留一定的容量空间，而LinkedList空间浪费主要在每个元素都需要存一个前驱和后继指针

### 插入和删除是否受元素位置的影响

ArrayList影响主要在于插入前和删除后元素的移动，LinkedList影响主要在于遍历链表找到元素所在的位置

## ArrayList和Vector区别？为什么要用ArrayList取代Vector

- ArrayList底层使用Object数组存储，非线程安全；Vector底层也用Object数组，线程安全；Vector里的方法基本都加了synchronized锁，效率没有ArrayList高
- ArrayList大多数情况下扩容为原来的1.5倍；Vector扩容为原来的两倍；所以ArrayList有利于节省空间

## ArrayList扩容机制

### ArrayList属性

    transient Object[] elementData; //元素数组
    private int size; //元素包含数量

### ArrayList的三种构造方法

#### ArrayList(int initialCapacity)

根据传入的初始化容量，创建ArrayList数组。如果在使用时预先知道数组的大小，一定要使用该构造方法，可以避免数组扩容提升性能，同时也是合理使用内存

    private static final Object[] EMPTY_ELEMENTDATA = {};

    public ArrayList(int initialCapacity){
        //初始化容量大于0时，创建Object数组
        if (initialCapacity > 0){
            this.elementData = new Object[initialCapacity];
        // 初始化容量等于0时，使用EMPTY_ELEMENTDATA对象
        }else if (initialCapacity == 0){
            this.elementData = EMPTY_ELEMENTDATA;
        //初始化容量小于0时，抛出IllegalArguementException
        }else{
            throw new IllegalArguementException("Illegal Capacity: " + initialCapacity)
        }
    }

#### ArrayList(Collection<? extends E> c)

使用传入的c集合，作为ArrayList的elementData

    public ArrayList(Collection<? extends E> c){
        //将c转换成Object数组
        elementData = c.toArray();
        //如果数组大小大于0
        if((size = elementData.length) != 0){
            // defend against c.toArray(incorrectly) not return Object[]
            (see e.g. https://bugs.openjdk.java.net/browse/JDK-6260652)
            // <x> 如果集合元素不是Object[]类型，则会创建新的Object[]数组，并将elementData赋值到其中
            if (elementData.getClass() != Object[].class);
            //如果数组大小等于0，则使用EMPTY_ELEMENTDATA
        } else {
            //replace with empty array
            this.elementData = EMPTY_ELEMENTDATA
        }
    }

#### ArrayList()

无参构造方法，使用的最多的构造方法

    //Default initial capacity
    private static final int DEFAULT_CAPACITY = 10;

    /*
     * 共享的空数组对象，用于{@link #ArrayList()}构造方法
     * 
     * 通过使用该静态变量，和{@link #EMPTY_ELEMENTDATA} 区分开来，在第一次添加元素时
     * 
     * Shared empry array instance used for default sized empty instance. 
     * We distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when element is added.
     * /

    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    //Constructs an empty list with an initial capacity of ten

    public ArrayList(){
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    } 

为什么单独声明了DEFAULTCAPACITY_EMPTY_ELEMENTDATA空数组，而不直接使用EMPTY_ELEMENTDATA呢？因为DEFAULTCAPACITY_EMPTY_ELEMENTDATA首次扩容为10，而EMPTY_ELEMENTDATA按照1.5倍扩容从0开始而不是10

### 扩容

整个的扩容过程，首先创建一个新的更大的数组，一般是 1.5 倍大小，然后将原数组复制到新数组中，最后返回新数组

    private Object[] grow(){
        // <1>
        return grow(size + 1);
    }

minCapacity指满足本次扩容的最小容量；<2>处，说明如果数组中有元素或者不是利用无参构造方法创建的，那么计算新的数组的大小，计算方法简单的可以理解为max(minCapacity - oldCapacity, oldCapacity >> 1) + oldCapacity，即前者指最小满足本次扩容的容量减去原来的容量，就是最小需要扩容容量；后者指原来的容量除以2；对于max操作来说，一般都是后者更大，有两种情况除外：1.oldCapacity为0，那么除以2还是为0，而minCapacity最小为1（因为只有添加元素时才会触发扩容）；2.批量添加元素时（addAll），此时minCapacity需要的最小扩容容量可能会比原来容量的一半要大；<3>则说明是无参构造方法创建的，取默认容量（10）和最小满足要求容量的最大值。所以一般用无参方法构建的数组默认容量为0，是空数组，只有在添加元素时，容量才有可能为10.