# Best Practices #

## JSON Organization ##

The JmxTransformer will attempt to internally optimize the way that you configure the hosts in your json files. If you can imagine, putting the same host in multiple files, with different queries could result in multiple server objects being generated. However, this is **not** the case if the Server objects are all the same, but just have different queries.

That said, in order to have the best experience, I suggest putting all of your queries into a single server block in the json.

## Number of Threads ##

If you notice that it is taking a while to get through the list of Servers and all of the queries, it might make sense to spawn more threads to handle the requests. Obviously, this also means being on a multi-core processor so that the cpu can actually do work with those threads.

## Security ##

JMX can be protected via login/password. This allow you to define different kind of users, some with read-write access, and others with just read access. JMXTrans doesn't need write access (in fact, JMXTrans code is read-only), so it is a good practice to use a dedicated read-only login for it.

In JSON, add **username** and **password** attributes to server, next to **host** and **port** :

```
{
  "servers" : [ {
    "port" : "1099",
    "host" : "w2",
    "username" : "collector",
    "password" : "1234567890",
    "queries" : [ {
      "obj" : "java.lang:type=Memory",
      "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ],
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.RRDToolWriter",
        "settings" : {
            "templateFile" : "heapmemory-rrd-template.xml",
            "outputFile" : "target/heap.rrd",
            "binaryPath" : "/opt/local/bin",
            "debug" : true,
            "generate" : true
        }
      } ]
    } ]
  } ]
}
```

On JMX side, you could set it with **com.sun.management.jmxremote.authenticate**, **com.sun.management.jmxremote.password.file**, **com.sun.management.jmxremote.access.file** parameter values ie :

```
java -Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.ssl=false \
-Dcom.sun.management.jmxremote.authenticate=true \
-Dcom.sun.management.jmxremote.password.file=/opt/myapp/etc/jmx.password \
-Dcom.sun.management.jmxremote.access.file=/opt/myapp/etc/jmx.access  \ com.mycorp.myproduct.MyApp start
```

jmx.access file will contains two roles/accounts, **collector** (read-only) and **administrator** (full-access) :

```
collector   readonly
administrator   readwrite
```

jmx.password file will contains login/password :


```
collector	1234567890
administrator	5c10b5b2cd673a0616d529aa5234b12ee7153808
```