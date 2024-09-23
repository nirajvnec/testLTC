

using Microsoft.Extensions.Caching.Memory;
using System;

public static class CacheService
{
    // Static IMemoryCache instance
    private static IMemoryCache _cache = new MemoryCache(new MemoryCacheOptions());

    // Static method to cache a value without expiration
    public static void CacheValue(string key, string value)
    {
        _cache.Set(key, value);
    }

    // Static method to retrieve a cached value
    public static string GetCachedValue(string key)
    {
        _cache.TryGetValue(key, out string cachedValue);
        return cachedValue;
    }

    // Static method to remove a cached value
    public static void RemoveCachedValue(string key)
    {
        _cache.Remove(key);
    }
}




using Microsoft.Extensions.Caching.Memory;
using System;

public class CacheService
{
    private IMemoryCache _cache;

    public CacheService()
    {
        _cache = new MemoryCache(new MemoryCacheOptions());
    }

    // Method to cache a value without expiration
    public void CacheValue(string key, string value)
    {
        // Cache the value without setting any expiration
        _cache.Set(key, value);
    }

    // Method to retrieve a cached value
    public string GetCachedValue(string key)
    {
        _cache.TryGetValue(key, out string cachedValue);
        return cachedValue;
    }

    // Method to remove a cached value
    public void RemoveCachedValue(string key)
    {
        _cache.Remove(key);
    }
}





Install-Package Microsoft.Extensions.Caching.Memory


Install-Package Microsoft.Extensions.Options
Install-Package Microsoft.Extensions.DependencyInjection.Abstractions


using Microsoft.Extensions.Caching.Memory;
using System;

public class CacheService
{
    private IMemoryCache _cache;

    public CacheService()
    {
        _cache = new MemoryCache(new MemoryCacheOptions());
    }

    // Method to cache a value with a sliding expiration time
    public void CacheValue(string key, string value, TimeSpan expirationTime)
    {
        var cacheEntryOptions = new MemoryCacheEntryOptions()
            .SetSlidingExpiration(expirationTime);  // Expire after no access for specified time
        _cache.Set(key, value, cacheEntryOptions);
    }

    // Method to retrieve a cached value
    public string GetCachedValue(string key)
    {
        _cache.TryGetValue(key, out string cachedValue);
        return cachedValue;
    }

    // Method to remove a cached value
    public void RemoveCachedValue(string key)
    {
        _cache.Remove(key);
    }
}
