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

    private Object[] grow(int minCapacity) {
        int oldCapacity = elementData.length;
        //<2> 如果原容量大于0，或者数组不是DEFAULT_EMPTY_ELEMENTDATA时，计算新的数组大小
        if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA){
            int newCapacity = ArraySupport.newLength(oldCapacitry, minCapacity - oldCapacity, oldCapacity >> 1);
            return elementData = Arrays.copyOf(elementData, newCapacity);
            //<3> 如果是DEFAULTCAPACITY_EMPTY_ELEMENTDATA数组，直接创建新的数组即可
        } else {
            return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
        }
    }

minCapacity指满足本次扩容的最小容量；
<2>处，说明如果数组中有元素或者不是利用无参构造方法创建的，那么计算新的数组的大小，计算方法简单的可以理解为max(minCapacity - oldCapacity, oldCapacity >> 1) + oldCapacity，即前者指最小满足本次扩容的容量减去原来的容量，就是最小需要扩容容量；后者指原来的容量除以2；
对于max操作来说，一般都是后者更大，有两种情况除外：
1.oldCapacity为0，那么除以2还是为0，而minCapacity最小为1（因为只有添加元素时才会触发扩容）；
2.批量添加元素时（addAll），此时minCapacity需要的最小扩容容量可能会比原来容量的一半要大；
<3>则说明是无参构造方法创建的，取默认容量（10）和最小满足要求容量的最大值。所以一般用无参方法构建的数组默认容量为0，是空数组，只有在添加元素时，容量才有可能为10.

### 缩容

    public void trimToSize() {
        //增加修改次数
        modCount++;
        //如果有多余的空间，则进行缩容
        if (size < elementData.length) {
            elementData = (size == 0)
                ? EMPTY_ELEMENTDATA //大小为0时，直接使用EMPTY_ELEMENTDADA
                : Arrays.copyOf(elementData, size);
        }
    }

## 多线程场景下如何使用ArrayList

1. 通过Collections的synchronizedList方法将其转换成线程安全的容器后再使用

        List<String> synchronizedList = Collections.synchronizedList(list);
        synchronizedList.add("aaa");
        synchronizedList.add("bbb");
        for (int i = 0; i < synchronizedList.size(); i++) {
            System.out.println(synchronizedList.get(i));
        }

2. 使用CopyOnWriteArrayList

## HashMap和HashTable区别

### 线程安全

HashMap非线程安全，HashTable线程安全

### 效率

HashTable因为线程安全，有额外的同步操作，HashMap的效率更高

### 初始容量和扩容策略

- HashMap默认初始化小大为16，若给定大小，则会将其扩充为2的幂次方大小；每次扩容为原来的2倍
- HashTable默认初始化大小为11，若给定大小，则直接使用给定的大小；每次扩容变为原来的2n + 1

### 对Null key和Null value支持

- HashMap支持Null作为键和值
- HashTable不允许有null键和值，否则会抛出NullPointerException

原因

- Hashtable使用Enumeration迭代器是安全失败机制（fail-safe），在使用过程中允许别的线程修改数据，如果存了null会存在二义性
- 而HashMap是fail-fast机制，遍历过程中不允许修改，不然报异常Concurrent Modification Exception；Hashtable使用Iterator迭代器是fail-fast机制

### 哈希地址

hashtable没有加扰动函数直接用key的hashcode，hashmap加了扰动函数

### 底层数据结构

JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制，一直都是数组+链表的形式

## HashMap和HashSet区别

HashSet底层就是基于HashMap实现的，HashSet的源码非常非常少，因为除了clone() 、 writeObject() 、 readObject() 是 HashSet自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法

| HashMap | HashSet |
| --- | --- |
| 实现了Map接口 | 实现了Set接口 |
| 存储键值对 | 仅存储对象 |
| 调用put()向map中添加元素 | 调用add()方法添加元素 |
| HashMap**使用键（Key）** 计算hashcode | HashSet**使用成员对象** 来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性 |

## HashSet实现原理

HashSet 是基于 HashMap 实现的，HashSet的值存放于HashMap的key上，HashMap的value统一为
PRESENT(new Object())，因此 HashSet 的实现比较简单，相关 HashSet 的操作，基本上都是直接调用底层
HashMap 的相关方法来完成，HashSet 不允许重复的值

## HashSet如何检查重复

当你把对象加入 HashSet 时， HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与其他加入的对象的 hashcode 值作比较，如果没有相符的 hashcode ， HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals() 方法来检查hashcode 相等的对象是否真的相同。如果两者相同， HashSet 就不会让加入操作成功

## HashMap底层实现

### JDK1.8之前

JDK1.8之前HashMap底层是数组和链表结合在一起使用也就是链表散列。HashMap通过key的hashCode经过扰动函数处理过后得到hash值，然后通过(n - 1) & hash判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的hash值以及key是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突

所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法也就是扰动函数是为了防止一些实现比较差的 hashCode() 方法，换句话说使用扰动函数之后可以减少碰撞

JDK1.8的hash方法相比于JDK1.7的hash方法更加简化，但是原理不变

    static final int hash(Object key) {
        int h;
        // key.hashCode(): 返回散列值也就是hashcode
        // ^: 按位异或
        // >>>: 无符号右移
        return (key == null) ? 0 : (h = key.hashcode()) ^ (h >>> 16)
    }

对比一下JDK1.7的HashMap的hash方法源码

    static int hash(int h){
        //This function ensures that hashCodes that differ only by constant multiplesat each bit pisition
        //have a bounded number of collisions(approximately 8 at default load factor)

        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }

### JDK1.8之后 :heart: :heart: :heart

---
相比于之前的版本， JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间

**解决哈希冲突的方法有哪些？HashMap用的哪种**  
解决Hash冲突方法有:开放定址法、再哈希法、链地址法（拉链法）、建立公共溢出区。HashMap
中采用的是 链地址法

**为什么在解决 hash 冲突的时候，不直接用红黑树？而选择先用链表，再转红黑树**  
因为红黑树需要进行左旋，右旋，变色这些操作来保持平衡，而单链表不需要。当元素小于 8 个的时候，此时做查询操作，链表结构已经能保证查询性能。当元素大于 8 个的时候， 红黑树搜索时间复杂度是 O(logn)，而链表是 O(n)，此时需要红黑树来加快查询速度，但是新增节点的效率变慢了。当链表长度到达 8 的概率是 0.00000006，但还是有这种情况，所以我们转换为红黑树效率会来得高

**为什么不使用平衡树而使用红黑树**  
虽然平衡树解决了二叉查找树退化为近似链表的缺点，能够把查找时间控制在 O(logn)，不过却不是最佳的，因为平衡树要求每个节点的左子树和右子树的高度差至多等于1，这个要求实在是太严了，导致每次进行插入/删除节点的时候，几乎都会破坏平衡树的第二个规则，进而我们都需要通过左旋和右旋来进行调整，使之再次成为一颗符合要求的平衡树。显然，如果在那种插入、删除很频繁的场景中，平衡树需要频繁着进行调整，这会使平衡树的性能大打折扣，为了解决这个问题，于是有了红黑树

**红黑树特点**

1. 具有二叉查找树的特点  
2. 根节点是黑色的  
3. 每个叶子节点都是黑色的空节点，也就是说，叶子节点不存数据  
4. 任何相邻的节点都不能同时为红色，也就是说，红色节点是被黑色节点隔开的  
5. 每个节点，从该节点到达其可达的叶子节点的所有路径，都包含相同数目的黑色节点

**HashMap存值的总体过程** :blue_heart: :purple_heart: :heart:

1. 在put的时候判断数组是否存在，如果不存在则用 resize 方法创建默认长度为16的数组  
2. 根据key计算hash值，确定在数组中的位置  
3. 判断该位置是否有元素，如果没有直接创建一个 Node 存入  
4. 如果有元素，通过equals方法判断 key 是否相同，如果相同则覆盖，并且将原来的值直接返回  
5. 如果 key 不相同，在原 Node 基础上添加新的 Node，判断该位置是链表还是红黑树  
    5.1 若为红黑树，则将结点存入树中  
    5.2 若为链表，遍历链表；链表中存在则复用，没有则新建结点加入；判断是否需要转为红黑树  
6. 判断是否需要数组扩容

**HashMap扩容（resize（））** :blue_heart: :purple_heart: :heart:

两倍扩容 HashMap，实际上，在构造方法中，table 数组并未初始化，它是在 resize() 方法中进行初始化，所以这是该方法的另外一个作用：初始化数组

:star2:**过程**:star2:

step1: 计算新的容量和阈值，并创建新的数组

1. 果容量大于最大容量2^30，则不允许扩容；Int最大值为2^31 - 1，所以最大容量为2^30
2. 如果旧的容量大于0，说明非空表，进行两倍扩容，newCap=oldCap << 1, newThr=oldThr << 1(阈值和容量都扩大一倍)
3. 否则如果阈值不为0，说明创建时传入了容量大小（非无参构造），那么newCap=oldThr
4. 否则如果1、2都不符合，则说明是初始化操作，newCap = DEFAULT_INITIAL_CAPACITY, newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);默认容量为16，默认负载因子为0.75
5. 如果上述操作没有计算出newThr,则newThr=newCap * loadFactor

step2: 将老的table复制到新的table中

1. 如果旧数组不为空，遍历旧数组
2. 如果数组中当前结点不为空
3. 将该结点复制给局部变量，且将旧数组此位置置NULL
4. 如果该结点没有next==null，则直接复制到新数组，下标计算方式为：hash&（newCap-1）
5. 如果next != null  
   5.1 如果是红黑树结点，则通过红黑树分裂处理  
   5.2 如果是链表，则遍历链表中的元素，分配到新数组中。链表中的元素有且只会被分配到新数组的两个位置（1.原下标；2.原下标 + 旧数组的长度）；hash & oldCap == 0 ? 位置1 ： 位置2

**为什么HashMap中的数组容量要为2的幂次方**

这与计算散列到数组中的哪个位置的计算方式有关。按照正常逻辑，计算散列到数组的哪个下标应该为hash % (len - 1)；而当len为2的幂次方时，可以用&操作来等效替代，即hash & (len - 1)，&在计算机中速度比%快。

**HashMap缩容**

暂时未提供缩容操作，想要了解参照Redis中hash的缩容

---

### HashMap默认加载因子是多少？为什么是0.75，不是0.6或者0.8

默认的loadFactor是0.75，0.75是对空间和时间效率的一个平衡选择，一般不要修改，除非在时间和空间比较特殊的情况下 ：1.如果内存空间很多而又对时间效率要求很高，可以降低负载因子Load factor的值；2.如果内存空间紧张而对时间效率要求不高，可以增加负载因子loadFactor的值，这个值可以大于1
