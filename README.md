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

---

## DEVOPS-9: Backend Web Servers

Two EC2 instances were created as backend web servers:

- `web1`
- `web2`

Both servers use:

- Ubuntu
- Nginx
- Git

The project repository was cloned on each backend server:

```bash
git clone https://github.com/USERNAME/project-03-nginx-load-balancer.git
```

The backend HTML files were deployed from the repository to Nginx:

```bash
sudo cp backend1/index.html /var/www/html/index.html
sudo cp backend2/index.html /var/www/html/index.html
sudo systemctl restart nginx
```

Backend pages:

- `web1` displays: `Backend Server 1 🚀`
- `web2` displays: `Backend Server 2 🚀`

The backend servers were tested successfully through their public IP addresses.

```bash
curl http://WEB1_PUBLIC_IP
curl http://WEB2_PUBLIC_IP
```

---

## DEVOPS-10: Nginx Load Balancer Upstream

The `lb1` EC2 instance was configured as an Nginx Layer 7 load balancer.

The load balancer uses an Nginx `upstream` block to forward HTTP requests to the two backend servers using their private IP addresses.

```nginx
upstream backend_servers {
    server WEB1_PRIVATE_IP;
    server WEB2_PRIVATE_IP;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```

The default Nginx configuration was disabled and the load balancer configuration was enabled:

```bash
sudo cp nginx/load-balancer.conf /etc/nginx/sites-available/load-balancer
sudo ln -s /etc/nginx/sites-available/load-balancer /etc/nginx/sites-enabled/load-balancer
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

The load balancer was tested successfully through its public IP address:

```bash
curl http://LB_PUBLIC_IP
```

The responses alternated between:

- `Backend Server 1 🚀`
- `Backend Server 2 🚀`

This confirms that Nginx round robin load balancing is working correctly.

---

## DEVOPS-11: DNS Configuration

A DNS record was configured in AWS Route 53 for the Nginx load balancer.

Record:

```text
load.devopsroad.xyz -> LB_PUBLIC_IP
```

The domain `devopsroad.xyz` was delegated from Namecheap to AWS Route 53 using the Route 53 nameservers.

DNS was verified using public DNS resolvers:

```bash
dig @8.8.8.8 load.devopsroad.xyz A +short
dig @1.1.1.1 load.devopsroad.xyz A +short
dig @9.9.9.9 load.devopsroad.xyz A +short
```

The load balancer was tested successfully through the domain:

```bash
curl http://load.devopsroad.xyz
```

The responses alternated between:

- `Backend Server 1 🚀`
- `Backend Server 2 🚀`

This confirms that the DNS record points correctly to the Nginx load balancer.
