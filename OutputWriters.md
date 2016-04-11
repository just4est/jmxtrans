

# Writing Results #

OutputWriter's are very powerful. They allow you to easily transform your Queries into whatever format you want. This allows sysadmins who may not be Java experts to quickly and easily add graphing of any Java system without writing code.

**The recommended way to use jmxtrans is to use the GraphiteWriter or GangliaWriter over the RRDToolWriter**.

The reason is that Graphite lends itself more easily to a schemaless database. The issue is that JMX produces a fairly complicated set of data and defining how that data should be stored can be difficult. Graphite / jmxtrans gets around this by taking advantage of the fact that Graphite expects its data columns in a dot notation and doesn't require defining any sort of schema in advance.

On the other hand RRD requires a schema to be defined and that can be a pain to setup (even though JMX trans will generate most of the configuration information for you by turning on debugging). That said, once you have the RRD schema, you still need to configure Cacti to read that schema and that is also a pain. Since this tool aims to keep things simple, your best shot at getting pretty graphs setup in a short amount of time is to use the GraphiteWriter.