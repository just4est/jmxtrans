# v? #

  * [Issue #15](https://code.google.com/p/jmxtrans/issues/detail?id=#15). KeyOutWriter was using a static log instance so it would combine logs together. Duh.
  * [Issue #14](https://code.google.com/p/jmxtrans/issues/detail?id=#14). Fixed file descriptor leakage in RRDToolWriter.
  * [r261](https://code.google.com/p/jmxtrans/source/detail?r=261). Added better support for weblogic.

# v250 #

  * log level is now configurable on /etc/default/jmxtrans or /etc/sysconfig/jmxtrans in LOG\_LEVEL variable. Default value stay debug. If you're using JMXTrans zip, you should now provide it with -Djmxtrans.log.level=debug on Java command line.

# v239 #

  * jmxtrans multi-instances support by providing PIDFILE vars to jmxtrans.sh and JMXTRANS\_OPTS passed to Java command line
  * Ant like variables into json files, so you could avoid hardcoding some values, like graphite servers.
  * RPM package (built on Fedora)

# v233 #

  * Support for Ganglia added.

# v168 #

  * Support for caching the JMX side of the connection.

# v155 #

  * Debian installer
  * Documentation on the website for starting jmxtrans-all.jar has also been updated
  * .sh script for starting things up