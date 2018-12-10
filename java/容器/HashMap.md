# JDK1.7 hashMap
### 成员变量
```angularjs
    /** 初始容量，默认16 */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

    /** 最大初始容量，2^30 */
    static final int MAXIMUM_CAPACITY = 1 << 30;

    /** 负载因子，默认0.75，负载因子越小，hash冲突机率越低 */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;

    /** 初始化一个Entry的空数组 */
    static final Entry<?,?>[] EMPTY_TABLE = {};

    /** 将初始化好的空数组赋值给table，table数组是HashMap实际存储数据的地方，并不在EMPTY_TABLE数组中 */
    transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;

    /** HashMap实际存储的元素个数 */
    transient int size;

    /** 临界值（HashMap 实际能存储的大小）,公式为(threshold = capacity * loadFactor) */
    int threshold;

    /** 负载因子 */
    final float loadFactor;

    /** HashMap的结构被修改的次数，用于迭代器 */
    transient int modCount;
```
### 构造方法
```angularjs
 public HashMap(int initialCapacity, float loadFactor) {
        // 判断设置的容量和负载因子合不合理
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        // 设置负载因子，临界值此时为容量大小，后面第一次put时由inflateTable(int toSize)方法计算设置
        this.loadFactor = loadFactor;
        threshold = initialCapacity;
        init();
    }

    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    public HashMap() {
        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR);
    }

    public HashMap(Map<? extends K, ? extends V> m) {
        this(Math.max((int) (m.size() / DEFAULT_LOAD_FACTOR) + 1,
                      DEFAULT_INITIAL_CAPACITY), DEFAULT_LOAD_FACTOR);
        inflateTable(threshold);
        putAllForCreate(m);
    }
```
### put方法
```angularjs
public V put(K key, V value) {  
    // 如果table引用指向成员变量EMPTY_TABLE，那么初始化HashMap（设置容量、临界值，新的Entry数组引用）
    if (table == EMPTY_TABLE) {
        inflateTable(threshold);
    }
    // 若“key为null”，则将该键值对添加到table[0]处，遍历该链表，如果有key为null，则将value替换。没有就创建新Entry对象放在链表表头
    // 所以table[0]的位置上，永远最多存储1个Entry对象，形成不了链表。key为null的Entry存在这里 
    if (key == null)  
        return putForNullKey(value);  
    // 若“key不为null”，则计算该key的哈希值
    int hash = hash(key);  
    // 搜索指定hash值在对应table中的索引
    int i = indexFor(hash, table.length);  
    // 循环遍历table数组上的Entry对象，判断该位置上key是否已存在
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {  
        Object k;  
        // 哈希值相同并且对象相同
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {  
            // 如果这个key对应的键值对已经存在，就用新的value代替老的value，然后退出！
            V oldValue = e.value;  
            e.value = value;  
            e.recordAccess(this);  
            return oldValue;  
        }  
    }  
    // 修改次数+1
    modCount++;
    // table数组中没有key对应的键值对，就将key-value添加到table[i]处 
    addEntry(hash, key, value, i);  
    return null;  
}  
```
### hash算法
```angularjs
    static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
        return h & (length-1);
    }
```
### 计算hash的方法
```angularjs
    static final int hash(Object key) {
        int h;
        /**
         * key 的 hash　值的计算是通过hashCode()的高16位异或低16位实现的：
         * (h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，
         * 这么做可以在数组table的length比较小的时候，
         * 也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销
         */
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```
### Entry数据结构源码如下（HashMap内部类）：
```angularjs
    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        /** 指向下一个元素的引用 */
        Entry<K,V> next;
        int hash;
        /**
         * 构造方法为Entry赋值
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }
        ...
        ...
 }
```
### 单链表的核心代码
```angularjs
    /**
     * 将Entry添加到数组bucketIndex位置对应的哈希桶中，并判断数组是否需要扩容
     */
    void addEntry(int hash, K key, V value, int bucketIndex) {
        // 扩容的机制为数组长度大于容量*负载因子并且存在hash冲突
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }
        createEntry(hash, key, value, bucketIndex);
    }

    /**
     * 在链表中添加一个新的Entry对象在链表的表头
     */
    void createEntry(int hash, K key, V value, int bucketIndex) {
        Entry<K,V> e = table[bucketIndex];
        table[bucketIndex] = new Entry<>(hash, key, value, e);
        size++;
    }
```
### 扩容机制
当HashMapde的长度超出了加载因子与当前容量的乘积（默认16*0.75=12）时，通过调用resize方法重新创建一个原来HashMap大小的两倍的newTable数组，最大扩容到2^30+1，
并将原先table的元素全部移到newTable里面，重新计算hash，然后再重新根据hash分配位置。这个过程叫作rehash，因为它调用hash方法找到新的bucket位置。
```angularjs
    void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        // 如果之前的HashMap已经扩充打最大了，那么就将临界值threshold设置为最大的int值
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }

        // 根据新传入的newCapacity创建新Entry数组
        Entry[] newTable = new Entry[newCapacity];
        // 用来将原先table的元素全部移到newTable里面，重新计算hash，然后再重新根据hash分配位置
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        // 再将newTable赋值给table
        table = newTable;
        // 重新计算临界值，扩容公式在这儿（newCapacity * loadFactor）
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }
    
    void transfer(Entry[] newTable, boolean rehash) {
            int newCapacity = newTable.length;
            for (Entry<K,V> e : table) {
                while(null != e) {
                    Entry<K,V> next = e.next;
                    if (rehash) {
                        e.hash = null == e.key ? 0 : hash(e.key);
                    }
                    int i = indexFor(e.hash, newCapacity);
                    e.next = newTable[i];
                    newTable[i] = e;
                    e = next;
                }
            }
        }
```
重新调整HashMap大小，当多线程的情况下可能产生条件竞争。因为如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。
在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的bucket位置的时候，
HashMap并不会将元素放在链表的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了。