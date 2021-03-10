# Lamp configuration and IaC

This is a demonstration of a bunch of Devops tools that I am familiar with. This repo of Infrastructure as code and configuration management using the following tools:

1. Vagrant
2. Ansible
3. Terraform

The objective is to write a working repository that will deploy the following virtual machines:

- Varnish load balancer
- Apache web server \* 2
- Memcached for caching
- MySQL database server \* 2
- Monitoring Grafana & Prometheus

This architecture is designed to provide high availability and redundancy on the web application and database level through a load balancer and caching nodes. The architecture also adds a monitoring node to monitor performance and logs from all nodes. It should however be noted that this is not a production ready architecture since there are some single points of failure for simplicity sake.
All persistent data stored in the database is stored in a slave server, and one of the slowest and most constrained parts of the stack (the web servers, in this case running a PHP application through Apache) is easy to scale horizontally, behind Varnish, which is acting as a caching (reverse proxy) layer and load balancer.

For the purpose of demonstration, Varnish's caching is completely disabled, so you can refresh and see both Apache servers (with caching enabled, Varnish would cache the first response then keep serving it without hitting the rest of the stack). You can see the caching and load balancing configuration in playbooks/varnish/templates/default.vcl.

Steps:

1. Create vagrant file
2. Create inventory file
3. Create ansible galaxy install requirement file
4. Create playbooks for each of the node sets
   - create web playbook
   - create varnish playbook
