

# Introduction #

The first thing you need to do is install jmxtrans. The easiest way is to use the Debian installer. Alternatively, you can install the files included in the .zip into a directory and run it from there.

Once you have it installed, you also need to make sure to enable remote access to JMX on the JVM's that you want to monitor.

# Installing the Debian #

There is a .deb file which you can download and install on an Ubuntu/Debian machine. This makes setting up the application trivial and is highly recommended. It also makes upgrades painless as well.

To install it:
  1. Download the .deb package.
  1. As root: `dpkg -i jmxtrans_239-1_amd64.deb` (replace the version number)
  1. Enter in the JVM heap size you want: 512 (megs) is the default. The more JVM's you need to monitor, the more memory you will probably need. If you are getting OutOfMemoryError's, then increase this value by editing `/etc/default/jmxtrans`.

  * The application is installed in: `/usr/share/jmxtrans`
  * Configuration options are stored in: `/etc/default/jmxtrans`
  * There is an init script in: `/etc/init.d/jmxtrans` (this wraps the `jmxtrans.sh` discussed below)
  * Put your .json files into: `/var/lib/jmxtrans`

# Installing the RPM #

There is a .rpm file which you can download and install on an Fedora/CentOS/RHEL machine. This makes setting up the application trivial and is highly recommended. It also makes upgrades painless as well.

To install it:
  1. Download the .rpm package.
  1. As root: `rpm -i jmxtrans_239-0.noarch.rpm` (replace the version number)
  1. Enter in the JVM heap size you want: 512 (megs) is the default. The more JVM's you need to monitor, the more memory you will probably need. If you are getting OutOfMemoryError's, then increase this value by editing `/etc/sysconfig/jmxtrans`.

  * The application is installed in: `/usr/share/jmxtrans`
  * Configuration options are stored in: `/etc/sysconfig/jmxtrans`
  * There is an init script in: `/etc/init.d/jmxtrans` (this wraps the `jmxtrans.sh` discussed below)
  * Put your .json files into: `/var/lib/jmxtrans`

# Running Jmx Transformer #

There is a jmxtrans.sh script included with the distribution. This should be used to start the application running. If you read through the script, you will see that all of the options are customizable by exporting environment variables without having to edit the .sh script. You can even create a jmxtrans.conf file to put the options into so that you don't need to setup environment variables yourself.

To run the jmxtrans:
  * `./jmxtrans.sh start [optional path to one json file]`

To stop jmxtrans:
  * `./jmxtrans.sh stop`

Options you may want to configure:
  * JSON\_DIR - Location of your .json files. Defaults to '.'
  * LOG\_DIR - Location of where the log files get written. Defaults to '.'
  * SECONDS\_BETWEEN\_RUNS - How often jobs run. Defaults to 60.

# Enabling JMX for a JVM #

In order to use jmxtrans, you must first enable Java Management Extensions (JMX) on your Java Virtual Machine (JVM). We recommend that you connect to Java 6 JVM's because there are improvements to the JMX protocol that we can take advantage of, such as wildcard (`*`) queries.

For applications behind a firewall that do not need security, add these arguments to the startup of the JVM in order to enable remote JMX connections:

```
-Dcom.sun.management.jmxremote.port=1105 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false
```

You should set the port number to any free port number on your machine that is above 1024.

For more details on enabling the agent, please read:

  * [JMX Agent Configuration](http://download.oracle.com/javase/6/docs/technotes/guides/management/agent.html)
  * [Monitoring and Management](http://download.oracle.com/javase/6/docs/technotes/guides/management/)

# JConsole #

If you are going to use jmxtrans, it is helpful to gain an understanding of JConsole. This is a good visual tool for viewing attributes in a JVM. Using this tool will help you write your jmxtrans queries.

  * [JConsole Documentation](http://download.oracle.com/javase/6/docs/technotes/guides/management/jconsole.html)

# Using Ant Vars #

Ant like variables could be used in json files since **v239**, so you could avoid hardcoding some values, like graphite servers.

### Before ###

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

### Now ###

```
{
  "servers" : [ {
    "port" : "${myserverport}",
    "host" : "${myserverhost}",
    "queries" : [ {
      "obj" : "java.lang:type=Memory",
      "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ],
      "outputWriters" : [ {
        "@class" : "com.googlecode.jmxtrans.model.output.GraphiteWriter",
        "settings" : {
          "port" : "${mygraphiteport}",
          "host" : "${mygraphitehost}"
        }
      } ]
    } ]
  } ]
}
```

### Notice ###

Double-quotes (") should be using even when providing int values, like port, it's mandatory for StringResolver, String to Int conversion will be done internally.

Variables should be provided via -D for example via JMXTRANS\_OPTS in jmxtrans.conf :

```
JMXTRANS_OPTS="-Dmyserverport=1099 -Dmyserverhost=w2 -Dmygraphiteport=2003 -Dmygraphitehost=192.168.192.133"
```