---

title: Docker Swarm Logging System
author: priyanka tyagi
tags: []

---

## Log system set-up basics with docker-swarm

### How the logging system works in docker-swarm
We know that logs for docker services that are generated in docker swarm, can be viewed using the command,

`docker service logs -f <service-name>`

Docker stores container logs as JSON files by default, but it includes a built-in driver for logging to Syslog endpoints. Both JSON and Syslog messages are easy to parse, contain critical information about each container, and are supported by most logging services. 

Going through the logs for each and service using the command mentioned above is a tedious and time consuming task. Hence, the other way to have logs visible for each and every service or a bunch of selective services, we can use the tool, ***syslog-ng*** . 
Using this tool we can keep the logs in a specific location, from where we can read the logs for multiple services at once.

### Steps to install the Syslog-NG tool
1. Run the following command to install,
	> apt-get install syslog-ng

After running the above command, you should be able to see the syslog service in `/etc` folder with service name as **syslog-ng**, as every service we install manually ***(using apt-get)*** is stored in the `/etc` folder.

2. Configure syslog-ng tool as per your requirement by editing the file,
	> vim /etc/syslog-ng/syslog-ng.conf
   
3.  Now there are four main components to syslog-ng configuration tool.
 - Port that syslog-ng tool should be listening to, configured as:
	> source var_name { network( transport(tcp) port(601)); };

	**Note:** 
	Looking inside syslog-ng.conf file, observe that syslog-ng service listens to the new syslog protocol on TCP port 601, and stores any incoming log messages in a file called `/var/log/syslog`.
	
	There are 6 ports available with syslog-ng tool, 
		 i.e. *601, 602,603,604,605,606*.
	We can use upto port 604, eventhough we can mention the same port number for all the services we require the logs for, across all docker-compose.yml files.

 -  Interaction between syslog-ng and docker service
	 There are **facilities** called `local0` to `local7`, where `facility` is the name of the (let's call it) "component" of the system, such as kernel, authentication, and so on.
	 The facilities `local0` to `local7` are ***custom*** unused facilities that syslog provides for the user. Now we want to make the docker services to log to syslog, or if you want to redirect the output of anything to syslog (for example, Apache logs), you can choose to send it to any of the `local#` facilities. Then, you can use `/etc/syslog.conf` (or `/etc/rsyslog.conf`) to save the logs being sent to that `local#` to a file, or to send it to a remote server.
 - Storing logs to a specific path
 - Combining the above three actions into one to generate the output

Append the following at the bottom of the file before the last line `@include "/etc/syslog-ng/conf.d/*.conf"`

~~. let's begin with understanding why we need logs and how the logging system works~~
. then introduction of ELK and after that the need of ELK stack
5 ELK set-up in swarm and how swarm works

```
1.    
    

# 1. Listening on port 601

source s_network {

network( transport(tcp) port(601));

};

  

# 2. Using the local6 facility, filter the debug level logs

# How did we tell docker to put logs on local6?

# In the bnext_dev_stack.yml file:

# logging:

# driver: syslog

# options:

# syslog-address: "tcp://127.0.0.1:601"

# syslog-facility: "local6"

# tag: "docker-{{.Name}}/{{.ID}}"

filter f_network { facility(local6) and not level(debug); };

  

# 3. Specify a place where we should put these filtered logs

# it will put only the message coming from docker

# which is in JSON format

destination d_network { file("/var/log/bnext.log" template("${MSG}\n")); };

  

# 4. This puts the previous 3 together and executes them

log { source(s_network); filter(f_network); destination(d_network); };

2.  touch /var/log/bnext.log
    
3.  chown root:adm /var/log/bnext.log
    
4.  Restart
    

1.  service syslog-ng restart
    

That takes care of configuring syslog-ng
```
> Written with [StackEdit](https://stackedit.io/).

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0OTgyMjI1NjQsLTQ3MjA1ODkwOSwtMT
c1NzA5MTEwMSw0NjA3NzE4NzBdfQ==
-->