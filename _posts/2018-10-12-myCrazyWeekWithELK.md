---

title: ELK set up
author: priyanka
tags: []

---

## ELK set-up basics with docker-swarm

### How the logging system works in docker-swarm
1. Logs that are generated in docker swarm we can viewed using the command,
`docker service logs -f <service-name>`
1. The logs visible here can also be stored into a specific location using the tool, ***ng-syslog***

3. let's begin with understanding why we need logs and how the logging system works
4. then introduction of ELK and after that the need of ELK stack
5. ELK set-up in swarm and how swarm works
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NjIyNzQwMTgsLTEwMDIwMzIyODEsMz
U1MjA2ODA0LDExMzk5MDEyNTEsMTk4NjM3ODU2OSwyMDY3NTY0
MzMwXX0=
-->