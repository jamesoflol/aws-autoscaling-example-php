AWS Auto Scaling CPU Stress Test
==============

Simple index.php page, to use for demonstration purposes from within Amazon EC2. Good for use as a demonstration of auto scaling and load balancing in AWS EC2.

On a T2.Nano instance it will take about 1/3 of a second to load the page. This page auto refreshes every 2 seconds, using about 12% CPU on average. (Until burst credits run out! But that's another story..)

Demo steps:
1. Create and customise AWS EC2 instance: install a web server with PHP, put this index.php code in, set the web server service to autorun, test. 
2. Make an AMI of that instance. (Alternatively, you can launch my public AMI of it, AMI ID ami-93c8d7f0 in region ap-southeast-2.)
3. Create an ELB (Elastic Load Balancer). Note the ELB's DNS address.
4. Create an ASG (Auto Scaling Group). 
4a. The Launch Configuration uses the AMI from step 2. 
4b. The Launch Configuration's security group needs to allow port 80 in.
4c. Ensure that you set the 'Load Balancing' advanced setting in the ASG to point to the ELB you created. 
4d. Scaling policy: 'Use scaling policies to adjust the capacity of this group' -> Scale between 1 and 6 instances. Average CPU Utilization / Target value = 50%.
5. Browse to the ELB's DNS address in a separate tab. Keep it open.
6. Check the monitoring tab of the ASG, and click on the 'EC2' link in there so that you can see the CPU utilisation rather than just stats about the ASG such as number of instances. It'll take 5 minutes for the stats to update. (unless you turned on advanced monitoring for your launch config, in which case it will take 1 minute!) If you've only got the one tab open, you'll only see about 12%. If you have at least about 5 tabs open, you should go over 50% CPU and see auto scaling kick in.