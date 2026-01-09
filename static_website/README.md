# Static website

This scenario will create a simple website.

### Cost estimates

If you do not have an active free tier account, then this scenario **may** incur a small cost:
* EC2 instance
* Public IP address

### Objectives

* Deploy a single EC2 instance with a public IP address
* Deploy a second EC2 instance with another public IP address
* Deploy a load balancer with a target group
* Create an auto-scaling group
* Change the load balancer to multi-AZ


## Deploy a single EC2 instance with a public IP address

> If you want a simple website, you can simply deploy an EC2 instance with a public IP address.
> 
> Include the following in the "User data" section to install and enable Apache2, and to create a very basic webpage on your instance:

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


## Deploy a second EC2 instance with another public IP address

> Create a second instance which has the exact same output.
> Call it "WebServer2".
> You can use the steps on the previous step to create the second instance.


## Deploy a load balancer with a target group

* Now there are two exact copies of your instance which are accessible from two different public IP addresses.
* It would be much better for the users if both instances were accessible from a single IP address.
* Also, you can only have five public IPs per region per account, so you cannot scale this was indefinitely.

> Create a new Load Balancer using a new Target Group which has all of your EC2 instances.

<details>
  <summary>I need help ...</summary>
  
  * In the EC2 Console, click **Load Balancers** in the left menu
  * In the **Application Load Balancer** box, click **Create**
  * Ensure that your Security Group allows **HTTP** access from **0.0.0.0/0**
  * Click **Create target group** (this will open another window)
  * Provide a Target Group name (e.g. "WebTargetGroup")
  * Click **Next**
  * Select all of your WebServer instances and click **Include as pending below**
  * Click **Next**
  * Click **Create target group**
  * In your Load Balancer create window, click the target group refresh button and then select your new target group
  * Click **Create load balancer**

Your new load balancer will have a custom DNS name assigned.  Use this to access either instance - refreshing that page should flip between the two instances.

</details>

## Create an auto-scaling group

* This is useful, but what if we need to add a third instance?  Or a tenth?
* We want our application to automatically spin up new EC2 instances or remove unneeded instances when required.

> Create an auto-scaling group so that new instances can be created or removed as needed based on demand.

