# Pure watchdog
A small bash watchdog that monitors the tomcat process on the Elsevier Pure CRIS server.

## About
The watchdog runs on each of our four Pure servers on a shared NFS drive (located in /data/tomcat_watchdog). If the tomcat process should die unexpectedly the process is restarted by the watchdog.

## Installation
Place the files in /data/tomcat_watchdog or edit the path in the bash script. Add your server hostnames to the script and the name of the matching Pure instance running on the server in question. E.g. in our installation the server "srv-vbn-pure1" runs the "admin" administration interface.

## Running the watchdog

Start the ./watchdog script as root and it will refork as a detached daemon process. Every minut the watchdog will check to see if tomcat is running.

If tomcat has died the logfiles are archived in crash/ and tomcat is then restarted.

As a safety precaution the watchdog will *newer* restart tomcat if someone is logged onto the server. This is usually the case during upgrades.

You need to enable the daemon for an instance by touching the instance name in the enabled/ folder:

    touch enabled/admin
    touch enabled/portal
    ...

And you can disable the deamon on demand by removing the file again:

    rm enabled/admin
    rm enabled/portal
    ...

Actions are logged to syslog.
