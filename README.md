
"When we launched the TSRD application today, we did not encounter a client certificate pop-up. We are unsure how the client certificate is being passed to the server, as this portion of the code is not within our control. To gain a better understanding of this process, further investigation is required. If we need to enable the client certificate pop-up for our web applications, like MarsNet and others, we would need to configure the client certificate settings in IIS where the MarsNet web app is hosted."



"When the MET application is launched, the user is prompted with a client certificate pop-up. The user selects a certificate, and the application retrieves the PID and other necessary details from the subject of the selected certificate. Additionally, the application utilizes the Windows NTID, obtained through Windows authentication, to query Active Directory for further information."

Does that sound good, or would you like any adjustments?





flowchart TD
    A[Application Initialization] --> B[Get User's Name and Domain]
    B --> C[Retrieve PID using LDAP from AD]
    C --> F[User app permissions are fetched by passing User's Windows NTId To Security Server]
    F --> D[API Calls Mars, UDM, Myriad]
    A --> E[Open X509 Certificate Store]

    subgraph "Step 1: Application Initialization (All Apps)"
    A --> A1[Initialize Application]
    A1 --> A2[Set Up Logging]
    A2 --> A3[Load Configuration]
    end

    subgraph "Step 2: Get User's Name and Domain (Only Windows app)"
    B --> B1[Environment.UserName]
    B --> B2[Environment.UserDomainName]
    end

    subgraph "Step 3: Retrieve PID using LDAP from AD (Only Windows app)"
    C --> C1[Connect to Active Directory]
    C1 --> C2[Perform LDAP Query using NTID]
    C2 --> C3[Extract PID from LDAP Result]
    end

    subgraph "Step 4: Fetch User Access"
    F --> F1[Connect to Security Server]
    F1 --> F2[Send UserName i.e., Windows NTID to Security Server]
    F2 --> F3[Receive User App Permissions]
    end

    subgraph "Step 5: API Calls Mars, UDM, Myriad"
    D --> D1[Create HTTP GET/POST Request]
    D1 --> D2[Add Client Certificate]
    D2 --> D3[Configure Request Properties]
    D3 --> D4[Set Content-Type & Request Body]
    D4 --> D5[Enable TCP Keep-Alive]
    end

    subgraph "Step 6: Open X509 Certificate Store"
    E --> E1[Search for Valid Certificates]
    E1 --> E2[Prompt User to Select Certificate]
    E2 --> E3[Retrieve Selected Certificate]
    E3 --> E4[Close Certificate Store]
    end














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
