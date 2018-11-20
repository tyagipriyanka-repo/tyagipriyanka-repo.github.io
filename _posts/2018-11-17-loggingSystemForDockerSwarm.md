---

title: Docker Swarm Logging System
author: priyanka tyagi
tags: []

---

## Log set-up basics in docker-swarm with Syslog-NG

### How the logging system works in docker-swarm
We know that logs for docker services that are generated in docker swarm, can be viewed using the command,

`docker service logs -f <service-name>`

Docker stores container logs as JSON files by default, but it includes a built-in driver for logging to Syslog endpoints. Both JSON and Syslog messages are easy to parse, contain critical information about each container, and are supported by most logging services. 

Going through the logs for each and service using the command mentioned above is a tedious and time consuming task. Hence, the other way to have logs visible for each and every service or a bunch of selective services, we can use the tool, ***syslog-ng*** . 
Using this tool we can keep the logs in a specific location, from where we can read the logs for multiple services at once.

### Steps to install the Syslog-NG tool
1. Run the following command to install,
	> apt-get install syslog-ng

	After running the above command, we should be able to see the syslog service in `/etc` folder with service name as **syslog-ng**, as every service we install manually ***(using apt-get)*** is stored in the `/etc` folder.

2. Configure syslog-ng tool as per our requirement by editing the file,
	> vim /etc/syslog-ng/syslog-ng.conf
   
3.  Now all that remains is to configure the syslog-ng tool as per our requirement.

### Configure Syslog-NG tool
There are four main components to syslog-ng configuration tool.
What needs to be appended is mentioned along with steps, we can append the following at the bottom of the file before the last line `@include "/etc/syslog-ng/conf.d/*.conf"`

**Port that syslog-ng tool should be listening to, configured as:**
> source var_name { network( transport(tcp) port(601)); };

- 	Looking inside syslog-ng.conf file, observe that syslog-ng service listens to the new syslog protocol on TCP port 601, and stores any incoming log messages in a file called `/var/log/syslog`.

	**Note:** 	
	There are 6 ports available with syslog-ng tool, 
		 i.e. *601, 602,603,604,605,606*.
	We can use upto port 604, keeping in mind we can mention the same port number for all the services that we require the logs for, across all docker-compose.yml files.

**Interaction between syslog-ng and docker service by filtering out the logs**
- There are **facilities** called `local0` to `local7`. 
	 (where `facility` is the name of the ***component*** of the system, such as kernel, authentication, and so on.)
	 
-	 The facilities `local0` to `local7` are ***custom*** unused facilities that syslog provides for the user. Now we want to make the docker services to log to syslog,  hence we can choose to send it to any of the `local#` facilities. 
	 - Most commonly used facility is **local6**
		 > filter <var_name> { facility(local6) and not level(debug); };
	 
-	 Then, we can use `/etc/syslog-ng/syslog-ng.conf` to save the logs being sent to that `local#` to a file, or to send it to a remote server.
 
**Storing logs to a specific location**
 - Specify a place where we should put these filtered logs, it will put only the message coming from docker, which is in JSON format.
	>  destination <var_name> { file("/var/log/<file_name>.log" template("${MSG}\n")); };

**Combining the above three actions into one to generate the output**
- Now the final step is to start logging using the above three steps.
	> log { source(<var_name>); filter(<var_name>); destination(<var_name>); };

	**Note:**
	For each port hat we use we need to keep separate variable names for source, filter and destination. As separate ports should be aligned to different facilities.
	 
### Configuration for docker services
 - Now we need to tell our docker services to interact wth syslog-ng on the configured port and the filtered out facility.
 - Docker privide us with the option of `logging`. Following is the configuration need to be placed for the docker services we want to view the logs for, in the `docker_stack.yml` file
```
logging:
  driver: syslog
  options:
    syslog-facility: "local6"
    syslog-address: "tcp://127.0.0.1:601"
    tag: "{{.ImageName}}-{{.Name}}/{{.ID}}"
```
**Note:**
So we know syslog facility is the facility we chose from `local0` to `local6`, syslog address is the port where syslog-ng service will be listening, i.e. 601.
Tag option specifies how to format a tag that identifies the container’s log messages. By default, the system uses the first 12 characters of the container ID.
- `.ID`: The first 12 characters of the container ID
- `.ImageName`: The name of the image used by the container
- `.Name`: The container name

### Start the syslog-ng service

1.  We need to create the file we've mentioned for the logs to store
	> touch /var/log/<file_name>.log  
2.  Change the file owner and group information
	> chown root:adm /var/log/<file_name>.log
3.  Restart the syslog-ng service
	>  service syslog-ng restart

All these steps takes care of configuring syslog-ng. Now we can see the logs being generated for the docker services using the command,
> tail -f /var/log/<file_name>.log
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3OTgwMjE2ODMsLTYzMDUwMjU0MywxMz
I4NzkyMzU2LC0xNDQ4NjkyOTI0LDE0MjQyNDg2MTQsLTc2NDcz
NzU2NCwtMTkxMDk3NzExOSwtMTAyMjIwNTAwNSwxMDEyMjU5NT
EwLC0xMTIxMzkxNzI4LDYxODM2NjA4MCwtODgwNTYxOTg3LC00
NzIwNTg5MDksLTE3NTcwOTExMDEsNDYwNzcxODcwXX0=
-->