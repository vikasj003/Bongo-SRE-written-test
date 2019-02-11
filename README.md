# Bongo-SRE-written-test
Below is the written test solution for Bongo’s Site Reliability Engineer position

Q. Certain web pages are loading slow in user’s browser for our live web application. What
steps will you take to resolve the issue?

Solution:

1. By implementing browser/HTTP caching and server-side caching.
2. Use Sprites to reduce HTTP requests or increasing TPS to take more request.
3. Investigating the system utilization for the particular applications and improving the resource allocation.
4. Minify your stylesheet files.
5. By implementing CDN. Depending upon the geographic location of your visitor, the requested content gets served by the node located at the nearest available data center.
5. Reducing the size of your Flash files or eliminating it.
6. By cleaning coding as excessive white spaces, inline stylings, empty new lines and unnecessary comments can make the website stylesheet grow larger in size.
7. By reducing the size of data being transferred between your server and the visitors.


Q. Imagine a scenario where a web application is serving from a single web server to the
internet. What are the problems in this scenario? Design and architect a solution that will
mitigate these problems? Or How would you design a scalable architecture with resiliency in
mind for the following situations:
a. if a service is resource intensive
b. a service needs to be low latency
c. if parts of a service need to be restricted to certain geographical boundaries

Solution:

With a Single webserver there may be a number of problems we can face, if for some reason e.g. when more user will access it will be slow down,the is no redandency, power issues or network connectivity issues the server becomes unavailable, the whole service will go down. 
Such situation can be mitigated by deploying the Web Server in a AWS Cloud Architecture. In the given scenario, the web service has to be deployed in 2 or more different regions considering the geographical constraints. Configure the Route53 service with Geolocation routing to direct the traffic accordingly based on the nearest geographical location. Going deep, in each region the web service has to be deployed in the appropriate EC2 instance that can handle the resource intesive workload with auto scalling enabled at minimum 2 availability zones with an ELB on top of them to distibute the traffic evenly within the multiple AZs. The low latency will be ensured via individual VPC configured in each region. 


Q. Currently there’s no monitoring in place for the above single web server. How and what
application will you use to monitor the resources/process in your new design?

Solution:

From AWS point of view, we can configure Cloudwatch metrics to monitor the EC2 instance that will host the Web Service. By default, we can monitor traffic every 5 mins. 

If it is an on-premises setup than you can integrate this server with any monitoring tool like Zabbix, Nagios, Zenoss etc.


Q. In our server we want to create a user who can only view logs using `less` from this path
/var/log. Please explain how to achieve this.

Solution:
Follow the bellow procedure to achieve this.

##create the user with the below changes

cp /bin/bash /bin/rbash
useradd -s /bin/rbash loguser

##Here programs are the directory for all his allowed programs/command

mkdir /home/loguser/programs
vim /home/loguser/.bash_profile
PATH=$HOME/programs

ln -s /bin/cd /home/loguser/programs/  
ln -s /bin/less /home/loguser/programs/  
ll /home/loguser/programs/

##restrict the user for making any modifications in their .bash_profile , as users can change it
chattr +i /home/localuser/.bash_profile


Q. Explain how you can ssh into a private server from the internet.

Solusion:
From AWS best practice viewpoint, we can ssh into a private server using a Bastion host. A bastion host is an EC2 instance in the public subnet. For which, we can allow the access in the private server using Security Group and Network access control list (NACL) for 1 IP or a range of IPs. In this way, access to the private server can be restricted as per required. 

For other co-located or on premise setup you can established a IPSEC-VPN to access your private servers or you can keep a Jumphost in private network which will be accessible from internet over VPN and from Jumphost all the internal system will be accessible.


Q. Write a bash function that will find all occurrences of an IPv4 from a given file.

Solution:

#!/bin/bash
cat filename.txt|grep '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}'
