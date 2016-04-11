# RRDTool Writer #

This is a slightly more complicated writer that relies heavily on convention based configuration. You **must** have rrdtool installed on the machine you are planning on running this on. Let's start off with an example configuration for for the OutputWriter:

```
{
  "servers" : [ {
    "port" : "1099",
    "host" : "w2",
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

There are three configuration options that are all required in order for RRDToolWriter to work.

  * **templateFile** is the path to the [heapmemory-rrd-template.xml](http://code.google.com/p/jmxtrans/source/browse/trunk/heapmemory-rrd-template.xml) file which configures the database schema. This file is primarily documented on the [JRobin website](http://oldwww.jrobin.org/api/templatesapi.html) and it mirrors the configuration options for ['rrdtool create'](http://oss.oetiker.ch/rrdtool/doc/rrdcreate.en.html), so I won't go into too much detail here.
  * **outputFile** is the path to the rrd database file that will get automatically created if it doesn't already exist.
  * **binaryPath** is the path to the directory that contains the rrdtool binary for your platform. That binary is used to create the database and write data to it. This isn't the most efficient way of doing things, but unfortunately is the easiest option using Java.
  * **debug** is an optional setting that defaults to false. When it is on, it will print out the generated datasource name that you would use in the template.xml file to record the numbers that you want to keep track of. These names are shortened versions of the attribute name initials.
  * **generate** will output, to the log, example datasource xml that you can copy/paste into the template.xml file. This is what is used to create your rrd database. If you have a lot of datasources, this will save you some typing. Note, this only works if you have debug enabled as well.

RRDToolWriter will first read in the template.xml file and create a database based on that file. The datasources (aka: 'columns') must match up to the values in the the Results. For example, if you have a Query that returns a Result that has the values 'commmitted' and 'max', then you will need to have two datasources named 'committed' and 'max'. This is the part where convention based configuration is employed. Enable debug mode to see what the names of the generated columns are. They are shortened because datasources have a maximum length of 20 characters. You may need to throw away your first couple of rrd files in order to get the schema (datasource name mapping) correct.

Only the datasources which are defined in the template.xml file are used to lookup the values as part of the Results. Thus, you can have a Query which returns Results that has many values. You only need to define datasources for the values that you want to store. Once you have created the rrd file, there is no way to modify the schema unless you use the rrdtool from the command line.