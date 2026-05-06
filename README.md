# Project 03 – Nginx Load Balancer on AWS

## Jira

### Epic

- DEVOPS-8: Configure Nginx Load Balancer

### Tasks

- DEVOPS-9: Create backend web servers
- DEVOPS-10: Configure Nginx upstream
- DEVOPS-11: Configure DNS
- DEVOPS-12: Test load balancing algorithms

---

## Project Goal

The goal of this project is to learn how Layer 7 load balancing works using Nginx.

In this project, I will deploy two backend web servers and one Nginx load balancer server on AWS EC2.

The load balancer will receive HTTP traffic and forward requests to the backend servers using Nginx upstream configuration.

---

## Initial Architecture

```text
                Internet
                    |
             load.devopsroad.xyz
                    |
            [ NGINX LOAD BALANCER ]
                    |
        -------------------------
        |                       |
   [ WEB SERVER 1 ]       [ WEB SERVER 2 ]
```

---

## Tech Stack

- AWS EC2
- Ubuntu Server
- Nginx
- Linux
- SSH
- DNS
- Git
- GitHub
- Jira

---
