//# LRU-Code
//Cache Optimization
using System;

namespace CacheMan
{
    public class CacheItem<T>
    {
        public string Mem { get; }
        public T Value { get; }

        public CacheItem(string mem, T value)
        {
            Mem = mem;
            Value = value ?? throw new ArgumentNullException(nameof(value));
        }
    }

    public class MemoryCache
    {
        private static readonly MemoryCache instance = new MemoryCache(100); // Set your desired capacity here

        private static readonly object lockObject = new object();

        private readonly System.Collections.Generic.Dictionary<string, LinkedListNode<object>> cacheMap;
        private readonly LinkedList<object> lruList;
        private readonly int capacity;

        private MemoryCache(int capacity)
        {
            this.capacity = capacity;
            cacheMap = new System.Collections.Generic.Dictionary<string, LinkedListNode<object>>();
            lruList = new LinkedList<object>();
        }

        public static MemoryCache Instance => instance;

        public T? Get<T>(string Mem)
        {
            lock (lockObject)
            {
                if (cacheMap.TryGetValue(Mem, out var node))
                {
                    lruList.Remove(node);
                    lruList.AddFirst(node);
                    return ((CacheItem<T>)node.Value).Value;
                }

                return default; // Simplified expression using 'default'
            }
        }

        public void Add<T>(string Mem, T value)
        {
            lock (lockObject)
            {
                if (cacheMap.TryGetValue(Mem, out var node))
                {
                    lruList.Remove(node);
                }
                else if (cacheMap.Count >= capacity)
                {
                    var lastNode = lruList.Last;
                    if (lastNode != null)
                    {
                        cacheMap.Remove(((CacheItem<T>)lastNode.Value).Mem);
                        lruList.RemoveLast();
                    }
                }

                var cacheItem = new CacheItem<T>(Mem, value);
                var newNode = new LinkedListNode<object>(cacheItem);
                lruList.AddFirst(newNode);
                cacheMap[Mem] = newNode;
            }
        }
    }

    public class Program
    {
        public static void Main()
        {
            // Sample usage of MemoryCache
            var cache = MemoryCache.Instance;

            // Adding items to the cache
            cache.Add("M1", DateTime.MinValue);
            cache.Add("M2", DateTime.MaxValue);
            cache.Add("M3", DateTime.Now);

            // Retrieving items from the cache
            DateTime? value1 = cache.Get<DateTime>("M1");
            DateTime? value2 = cache.Get<DateTime>("M2");
            DateTime? value3 = cache.Get<DateTime>("M3");

            // Displaying the retrieved values
            Console.WriteLine($"Value1: {value1}");
            Console.WriteLine($"Value2: {value2}");
            Console.WriteLine($"Value3: {value3}");
        }
    }
}
