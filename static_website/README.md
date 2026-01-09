# Static website

This scenario will create a simple website.

### Cost estimates

If you do not have an active free tier account, then this scenario **may** incur a small cost:
* EC2 instance
* Public IP address

### Objectives

* Deploy a single EC2 instance with a public IP address
* Deploy a second EC2 instance with another public IP address
* Deploy an DNS Alias record in Route53
* Deploy a load balancer
* Configure ELB health checks
* Create an auto-scaling group
* Change the load balancer to multi-AZ

## Deploy a single EC2 instance with a public IP address

If you want a simple website, you can simply deploy an EC2 instance with a public IP address.

Include the following in the "User data" section to install and enable Apache2, and to create a very basic webpage on your instance:

    #!/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html

<details>
  <summary>I need help ...</summary>
  
  * In the EC2 Console, click **Launch instances**
  * Provide a name (e.g. "WebServer1")
  * This scenario is fine with a **micro Amazon Linux** instance
  * Ensure that your Security Group allows **HTTP** access from **0.0.0.0/0**
  * Expand the **Advanced Details** section
  * Add the commands above to the **User Data** section
  * Click **Launch instance**
  
</details>
