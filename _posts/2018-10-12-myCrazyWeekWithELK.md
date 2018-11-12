---

title: ELK set up
author: priyanka
tags: []

---

## Log system set-up basics with docker-swarm

### How the logging system works in docker-swarm
1. Logs that are generated in docker swarm we can viewed using the command,
`docker service logs -f <service-name>`
Docker stores container logs as JSON files by default, but it includes a built-in driver for logging to Syslog endpoints. Both JSON and Syslog messages are easy to parse, contain critical information about each container, and are supported by most logging services. 
2. ShortcomingThe logs visible here for each and every service can also be stored into a specific location using the tool, ***syslog-ng*** . From where we can read the logs for multiple services at once.
3. Install the Syslog NG tool
    

4.  apt-get install syslog-ng
    
5.  vim /etc/syslog-ng/syslog-ng.conf
    

Append the following at the bottom of the file before the last line @include "/etc/syslog-ng/conf.d/*.conf"

3. let's begin with understanding why we need logs and how the logging system works
4. then introduction of ELK and after that the need of ELK stack
5. ELK set-up in swarm and how swarm works
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNDAyNjA1OTksMTI4MTQxNjE4OSwtMT
AwMjAzMjI4MSwzNTUyMDY4MDQsMTEzOTkwMTI1MSwxOTg2Mzc4
NTY5LDIwNjc1NjQzMzBdfQ==
-->