# GangliaWriter #

[Ganglia](http://ganglia.sourceforge.net) is another monitoring / graphing tool that is similar to Graphite in that it produces graphs of metrics. By default it includes the collection of a lot of server metrics such as cpu load and memory usage. Ganglia uses rrd as the backend storage format. It also uses a series of daemons to help spread the graphing load around.

jmxtrans has a writer that connects to a gmond (not gmetad!) process and writes data directly to it using UDP. One nice feature of Ganglia is that you do not need to configure it when you add new hosts.

![http://jmxtrans.googlecode.com/svn/wiki/ganglia.png](http://jmxtrans.googlecode.com/svn/wiki/ganglia.png)

As described in the (borrowed) image above, you can see that jmxtrans is writing data to a gmond instance. Since I support host spoofing, whatever is set as `host` or optionally `alias` is what is sent to Ganglia as the host. Thus, jmxtrans is effectively acting like a gmond instance itself. This also means that jmxtrans doesn't have to run on the same machine as the gmond instance.

If there is demand, future versions of this plugin can support writing to multicast just like the other gmond instances do. That said, looking at Hadoop's support of Ganglia, it is just writing straight UDP, so I suspect this isn't going to be a high demand feature.

# Example Configuration #

```
{
  "servers" : [ {
    "host" : "w2",
    "alias" : "w2.fullname.com",
    "port" : "1099",
    "queries" : [ {
      "obj" : "java.lang:type=GarbageCollector,name=ConcurrentMarkSweep",
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GangliaWriter",
        "settings" : {
          "groupName" : "memory",
          "host" : "10.0.3.16",
          "port" : 8649
        }
      } ]
    } ]
  } ]
}
```

Configuration attributes:

  * **host** - The hostname for the machine running gmond.
  * **port** - The port that gmond is accepting UDP requests on.
  * **groupName** - How you want your graphs groups in the Ganglia interface.