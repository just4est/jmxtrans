# Introduction #

Some sample JSON, to help you monitor quickly various metrics in :

  * Sun/Oracle Hotspot heap, memory-pools, threads
  * Apache Tomcat and JBoss App servers AJP/HTTP Pools and DataSources.

# Graphite Samples #

  * In the following samples, JVM under monitoring is on an hypothetical machine named **mysys.mydomain**, where JMX is listening port on **8004**.
  * A Graphite server is installed on **mygraphite.mydomain** where its carbon-cache collect metrics on port **2003**

## System (Hotspot) ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain"
        }
      } ],
      "obj" : "java.lang:type=OperatingSystem",
      "attr" : [ "SystemLoadAverage", "AvailableProcessors", "TotalPhysicalMemorySize", "FreePhysicalMemorySize", "TotalSwapSp
aceSize", "FreeSwapSpaceSize", "OpenFileDescriptorCount", "MaxFileDescriptorCount" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## Heap (Hostpot) ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain"
        }
      } ],
      "obj" : "java.lang:type=Memory",
      "resultAlias": "heap",
      "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ]
    }, {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain"
        }
      } ],
      "obj" : "java.lang:name=CMS Old Gen,type=MemoryPool",
      "resultAlias": "cmsoldgen",
      "attr" : [ "Usage" ]
    }, {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "java.lang:type=GarbageCollector,name=*",
      "resultAlias": "gc",
      "attr" : [ "CollectionCount", "CollectionTime" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## Memory Pool (Hostpot) ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "java.lang:type=MemoryPool,name=*",
      "resultAlias": "memorypool",
      "attr" : [ "Usage" ]
    } ],
    "numQueryThreads" : 2
  } ]
```

## Threads (Hostpot) ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain"
        }
      } ],
      "obj" : "java.lang:type=Threading",
      "resultAlias": "threads",
      "attr" : [ "DaemonThreadCount", "PeakThreadCount", "ThreadCount", "TotalStartedThreadCount" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## JBoss AJP/HTTP Pools ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "jboss.web:type=ThreadPool,name=*",
      "resultAlias": "connectors",
      "attr" : [ "currentThreadCount", "currentThreadsBusy", "" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## JBoss HTTP Requests ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "jboss.web:type=GlobalRequestProcessor,name=*",
      "resultAlias": "requests",
      "attr" : [ "bytesReceived", "bytesSent", "errorCount", "maxTime", "processingTime", "requestCount"  ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## JBoss DataSources ##

```

{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
	  "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "jboss.jca:service=ManagedConnectionPool,name=*",
      "resultAlias": "datasources",
      "attr" : [ "ConnectionCount", "ConnectionCreatedCount", "InUseConnectionCount", "MaxConnectionsInUseCount", "AvailableConnectionCount" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## Apache Tomcat AJP/HTTP Pools ##

```
{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "username" : "monitor",
    "password" : "monitor!",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "Catalina:type=ThreadPool,name=*",
      "resultAlias": "connectors",
      "attr" : [ "currentThreadCount", "currentThreadsBusy", "" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```

## Apache Tomcat DataSources ##

```

{
  "servers" : [ {
    "port" : "8004",
    "host" : "mysys.mydomain",
    "username" : "monitor",
    "password" : "monitor!",
    "queries" : [ {
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "mygraphite.mydomain",
          "typeNames" : [ "name" ]
        }
      } ],
      "obj" : "Catalina:type=DataSource,class=javax.sql.DataSource,name=*",
      "resultAlias": "datasources",
      "attr" : [ "numActive", "numIdle" ]
    } ],
    "numQueryThreads" : 2
  } ]
}
```