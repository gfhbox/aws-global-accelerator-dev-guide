# Port overrides<a name="about-endpoint-groups-port-override"></a>

By default, an accelerator routes user traffic to endpoints in AWS Regions using the protocol and port ranges that you specify when you create a listener\. For example, if you define a listener that accepts TCP traffic on ports 80 and 443, the accelerator routes traffic to those ports on an endpoint\.

However, when you add or update an endpoint group, you can override the listener port used for routing traffic to endpoints\. For example, you can create a port override in which the listener receives user traffic on ports 80 and 443, but your accelerator routes that traffic to ports 1080 and 1443, respectively, on the endpoints\.

For each port override, you specify a listener port that that accepts traffic from users and the endpoint port that Global Accelerator will route that traffic to\. For more information, see [ Adding, editing, or removing a standard endpoint group](about-endpoint-groups.create-endpoint-group.md)\.

When you create a port override, keep the following in mind:
+ **Endpoint ports can’t overlap listener port ranges\.** The endpoint ports that you specify in a port override cannot be included in any of the listener port ranges that you've configured for the accelerator\. For example, say that you have two listeners for an accelerator, and you've defined the port ranges for those listeners as 100\-199 and 200\-299, respectively\. When you create port overrides, you can't define one from listener port 100 to endpoint port 210, for example, because the endpoint port \(210\) is included in a listener port range that you defined \(200\-299\)\.
+ **No duplicate endpoint ports\.** If one port override in an accelerator specifies an endpoint port, you can’t specify the same endpoint port with port override from a different listener port\. For example, you can’t specify a port override from listener port 80 to endpoint port 90 together with an override from listener port 81 to endpoint port 90\.
+ **Health check continues to use the original port\.** If you specify a port override for a port that is configured as a health check port, the health check still uses the original port, not the override port\. For example, say that you specify health checks on listener port 80, and you also specify a port override from listener port 80 to endpoint port 480\. Health checks continue to use endpoint port 80\. However, user traffic that comes in through port 80 goes to port 480 on the endpoint\.

  This behavior maintains consistency between Network Load Balancer, Application Load Balancer, EC2 instance, and Elastic IP address endpoints\. Because Network Load Balancers and Application Load Balancers don’t map health check ports to a different endpoint ports when you specify a port override in Global Accelerator, it would be inconsistent for Global Accelerator to map health check ports to different endpoint ports for EC2 instance and Elastic IP address endpoints\.
+ **Security group settings must allow port access\.** Make sure that your security groups allow traffic to arrive at endpoint ports that you've designated in port overrides\. For example, if you override listener port 443 to endpoint port 1433, make sure that any port restrictions set in your security group for that Application Load Balancer or Amazon EC2 endpoint allow inbound traffic on port 1433\.