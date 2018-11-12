---

title: ELK set up
author: priyanka
tags: []

---

## Log system set-up basics with docker-swarm

### How the logging system works in docker-swarm
Logs that are generated in docker swarm we can viewed using the command,
`docker service logs -f <service-name>`
Docker stores container logs as JSON files by default, but it includes a built-in driver for logging to Syslog endpoints. Both JSON and Syslog messages are easy to parse, contain critical information about each container, and are supported by most logging services. 

Going through the logs for each and service using the command mentioned above is a tediuos and time consuming task. Hence, the other way t loo have logs visible for each and every service or a bunch of selective services can use the tool, ***syslog-ng*** . 
Using this tool we can keep the logs in a specific location, from where we can read the logs for multiple services at once.

### Steps to install the Syslog-NG tool

1. Run the following command to ,
`apt-get install syslog-ng`
After running the above command, you should be able to see the syslog service in `/etc` folder, as every service we install manually *(using apt-get)* is stored in the `/etc` folder
`ls /etc/syslog-ng` 
  
 2. 

     vim /etc/syslog-ng/syslog-ng.conf
    

Append the following at the bottom of the file before the last line @include "/etc/syslog-ng/conf.d/*.conf"

3. let's begin with understanding why we need logs and how the logging system works
4. then introduction of ELK and after that the need of ELK stack
5. ELK set-up in swarm and how swarm works
> Written with [StackEdit](https://stackedit.io/).

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzY0MjEyNzQsLTExNDAyNjA1OTksMT
I4MTQxNjE4OSwtMTAwMjAzMjI4MSwzNTUyMDY4MDQsMTEzOTkw
MTI1MSwxOTg2Mzc4NTY5LDIwNjc1NjQzMzBdfQ==
-->