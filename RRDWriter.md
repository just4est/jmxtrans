# RRD Writer #

This is simlar to the RRDToolWriter, except it uses a [100% java solution](http://www.jrobin.org/index.php/Main_Page) to write the RRD files. This means that the files are **not** compatible with the binary C version of RRD. I'm not sure why the authors made a version of rrd that isn't compatible with anything else, seems kind of broken to me. Regardless, I still support it for those of you who really need it.

You specify the path to the template file and the output rrd file.

```
{
  "servers" : [ {
    "port" : "1099",
    "host" : "w2",
    "queries" : [ {
      "obj" : "java.lang:type=Memory",
      "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ],
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.RRDWriter",
        "settings" : {
            "templateFile" : "heapmemory-rrd-template.xml",
            "outputFile" : "target/heap.rrd",
        }
      } ]
    } ]
  } ]
}
```