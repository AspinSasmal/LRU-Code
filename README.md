# LRU-Code
Cache Optimization

	
This code is an implementation of a simple in-memory cache using a Least Recently Used (LRU) eviction policy. Let's go through it step-by-step:

The CacheItem<T> class is a generic class that represents an item in the cache. It has two properties: Mem, which represents the key of the item, and Value, which represents the value of the item. The Value property is initialized in the constructor and cannot be null.

The MemoryCache class is the main cache implementation. It is a singleton class, meaning there can only be one instance of it.

The MemoryCache class has a private constructor that takes a capacity parameter. The capacity specifies the maximum number of items that can be stored in the cache.

The MemoryCache class has three private fields:

cacheMap: A dictionary that maps keys to LinkedList nodes. This allows for quick lookup and removal of items from the cache.
lruList: A doubly linked list that keeps track of the recently used items, with the most recently used item at the head of the list.
capacity: The maximum capacity of the cache.
The MemoryCache class has a public static property Instance that returns the singleton instance of the cache. This ensures that there is only one instance of the cache throughout the program.

The MemoryCache class has two public methods: Get<T> and Add<T>. Both methods are thread-safe and have a lock to ensure thread-safety.

The Get<T> method takes a key Mem and returns the value associated with that key if it exists in the cache. It follows the LRU eviction policy by moving the accessed item to the head of the lruList to mark it as the most recently used. If the item is not found, it returns the default value for type T.

The Add<T> method takes a key Mem and a value of type T and adds it to the cache. If the key already exists in the cache, it updates the value and moves the item to the head of the lruList. If the cache is full, it removes the least recently used item from both the cacheMap and the lruList. Then, it creates a new CacheItem<T> object with the key and value, creates a new LinkedListNode<object> with the CacheItem as the value, and adds it to the head of the lruList. Finally, it updates the cacheMap with the new key-value mapping.

The Program class contains a Main method that demonstrates the usage of the MemoryCache class. It creates an instance of MemoryCache, adds three items to the cache with different types (int, string, and DateTime), retrieves the items using the Get method with their respective types, and displays the retrieved values.

Overall, this code provides a basic implementation of an in-memory cache with a LRU eviction policy. It allows you to store and retrieve items based on their keys, and it automatically evicts the least recently used items when the cache reaches its capacity.
