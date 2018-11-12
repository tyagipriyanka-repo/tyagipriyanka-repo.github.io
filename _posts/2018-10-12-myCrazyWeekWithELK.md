---

title: ELK set up
author: priyanka
tags: []

---

## Log system set-up basics with docker-swarm

### How the logging system works in docker-swarm
1. Logs that are generated in docker swarm we can viewed using the command,
`docker service logs -f <service-name>`
Docker stores container logs as JSON files by default, but it includes a built-in driver for logging to Syslog endpoints. Both JSON and Syslog messages are easy to parse, contain critical information about each container, and are supported by most logging services. Many container-based loggers such as Logspout support both JSON and Syslog, and Loggly has complete support for parsing and indexing both formats.
1. The logs visible here can also be stored into a specific location using the tool, ***syslog-ng***

3. let's begin with understanding why we need logs and how the logging system works
4. then introduction of ELK and after that the need of ELK stack
5. ELK set-up in swarm and how swarm works
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ3NTc4MDQ2NiwxMjgxNDE2MTg5LC0xMD
AyMDMyMjgxLDM1NTIwNjgwNCwxMTM5OTAxMjUxLDE5ODYzNzg1
NjksMjA2NzU2NDMzMF19
-->