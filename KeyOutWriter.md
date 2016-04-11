# Key Out Writer #

This simple writer shares a lot of the code with the GraphiteWriter. Instead of writing data to Graphite, it writes the same data to the specified outputFile in tab delimited format. This way you can write another script which will monitor the data as it is written to the file. There are three settings for this writer:

  * **outputFile** the file to write to
  * **maxLogFileSize** the max size before rolling the file
  * **maxLogBackupFiles** max number of rolled files

```
{
  "servers" : [ {
    "port" : "1099",
    "host" : "w2",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.KeyOutWriter",
        "settings" : {
          "outputFile" : "/tmp/keyout2.txt",
          "maxLogFileSize" : "10MB",
          "maxLogBackupFiles" : 200,
          "debug" : true,
          "typeNames" : ["name"]
        }
      } ],
      "obj" : "net.sf.ehcache:type=CacheStatistics,*",
      "attr" : [ "CacheHits", "CacheMisses", "ObjectCount" ]
    }
     ]
  } ]
}
```

Example output from the configuration above:

```
w2_1099.net_sf_ehcache_management_CacheStatistics.longTermCache.CacheHits	487239	1308855201
w2_1099.net_sf_ehcache_management_CacheStatistics.longTermCache.CacheMisses	30164	1308855201
w2_1099.net_sf_ehcache_management_CacheStatistics.longTermCache.ObjectCount	4270	1308855201
```

One thing to mention here is the use of the typeNames key in the configuration. What this does is it narrows a query down to the essentials. What I mean is that the obj= portion of the query has a wildcard `*` in it. The `*` represents all of the various cache names. So, by using a typeName of 'name', we are selecting out the value of that type name and using it as part of the key. Thus, "longTermCache" is the value of of the `*`.