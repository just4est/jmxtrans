# Graphite Writer #

[Graphite](http://graphite.wikidot.com/) is a very cool graphing mechanism that is similar to Cacti, but far easier to setup since you don't have to become a RRD expert. All you have to do to configure jmxtrans to use Graphite is to setup the GraphiteWriter with a host and port and the writer will send what ever you want to graph to it.

Here is an example .json file that outputs HeapMemoryUsage and NonHeapMemoryUsage directly to graphite:

```
{
  "servers" : [ {
    "port" : "1099",
    "host" : "w2",
    "queries" : [ {
      "obj" : "java.lang:type=Memory",
      "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ],
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : 2003,
          "host" : "192.168.192.133"
        }
      } ]
    } ]
  } ]
}
```

This produces the following screen shot. It was not necessary to tell graphite anything about the 'tree' that was created as it was generated automatically by GraphiteWriter. [This is the example Java code to generate](http://code.google.com/p/jmxtrans/source/browse/trunk/src/com/googlecode/jmxtrans/example/Graphite.java) the above json.

![http://jmxtrans.googlecode.com/svn/wiki/render.png](http://jmxtrans.googlecode.com/svn/wiki/render.png)

If you add "rootPrefix":"my.root.prefix", to the "settings" section of the configuration, then it will prepend the value to the graphite 'tree' in place of what is now "servers".