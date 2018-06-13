---
layout: default
title: {{ site.name }}
---
# Overview
The Dragonn software and associated tutorial can be accessed as a public image through Amazon Web Services. We recommend this option for users who 
do not have access to GPUs on their local systems, as the AWS instance will allow them to run Dragonn on a GPU. Alternativelly, users have the option to install 
Dragonn locally on their system through Anaconda. 

## Launching an Amazon AWS instance with Dragonn 

1. Create an account with [Amazon AWS](<http://www.aws.amazon.com>)

 ![AWS login window]({{ site.baseurl }}/images/aws_login.png "AWS Login Window")

2. Login to your AWS account, and set your region to **US West (Oregon)** or to **US West (Northern California)**

 ![AWS Region]({{ site.baseurl }}/images/aws_region.png "AWS Select Region")

3. Go to the **services** tab and select **EC2**

 ![AWS Services]({{ site.baseurl }}/images/aws_services.png "AWS Services")

4. On the left pane of the Web site, select **AMIs**, and type "dragonn" into the search bar. You should see AMI Name: **Kundaje Lab Dragonn** (if you are in the North California region) 
or **Kundaje Lab Dragonn Oregon** if you are in the Oregon region. You will not see the AMI if your region is set to anything other than these two options (see step 2). 

 ![AWS AMI]({{ site.baseurl }}/images/aws_ami.png "AWS AMI")

5. Select the AMI and click **Launch**. 

6. Go through the setup tutorial  to configure your Dragonn instance, making sure to select the following options: 

  a. In **Step 2: Choose an Instance Type** select **g2.2xlarge**.  

   ![AWS GPU Instance]({{ site.baseurl }}/images/aws_gpuinstance.png "AWS GPU Instance")

  b. In **Step 3: Configure Instance Details** use the default options. 

   ![Step 3: Configure Instance Details]({{ site.baseurl }}/images/aws_step3.png "Step 3: Configure Instance Details")

  c. In **Step 4: Add Storage** set the Size(GiB) value to 20. 

   ![Step 4: Add Storage]({{ site.baseurl }}/images/aws_step4.png "Step 4: Add Storage")

  d. In **Step 5: Tag Instance** leave the defaults (expert users may want to add tags to identify the instance uniquely). 

   ![Step 5: Tag Instance]({{ site.baseurl }}/images/aws_step5.png "Step 5: Tag Instance")

  e. In **Step 6: Configure Security Group** follow these steps: 

     i. Add Rule --> Custom TCP Rule ; Port Range --> 8443; Source --> Anywhere 

     ii. Add Rule --> HTTP; Port Range --> 80; Source --> Anywhere 

     iii. Add Rule --> HTTPS; Port Range --> 443; Source --> Anywhere
 
   ![Step 6: Configure Security Group]({{ site.baseurl }}/images/aws_step6.png "Step 6: Configure Security Group")

  f. In **Step 7: Review and Launch** leave all defaults and click **Launch**. 

   ![Step 7: Review and Launch]({{ site.baseurl }}/images/aws_step7.png "Step 7: Review and Launch")

  g. In **Select an existing key pair or create a new key pair** select **Create a new key pair**. Select a name for your key pair (such as "dragonn_keys") and click **Download Key Pair**. 
     **Keep the file in a safe place, you will need it to connect to your Amazon AWS instance.** 

   ![Create key pair]({{ site.baseurl }}/images/aws_keypair.png "Create key pair")

7. When you launch the instance, you will arrive back at the EC2 dashboard, and you will see your new instance in the **running** state. Please note that the cost of running your g2.2xlarge instance 
is $0.65 per hour. 

 ![Instance running]({{ site.baseurl }}/images/aws_running.png "Instance running")

8. Select your instance, and click the **Connect** button. You will see instructions on how to connect to your instance through ssh (If you are running Windows, you will need an SSH client such as [putty](<http://www.chiark.greenend.org.uk/~sgtatham/putty/>). Follow the instructions in the popup window to chmod your key file (see step 6g) and ssh into your instance. It is likely that you will need to ssh in as the "ubuntu" user rather than the "root" user. Modify the example command to indicate:
 ```
 ssh -i 'dragonn_keys.pm' ubuntu@ec2-52-43-29-19.us-west-2.compute.amazonaws.com
 ```
 ![Connecting to Amazon Instance]({{ site.baseurl }}/images/aws_connect.png "Connecting to Amazon Instance")

9. Once you have connected to the instance, type

 ```
 sudo run.sh
 ```

10. You will see commands similar to the following as the code pulls the latest container with the Dragonn software from Dockerhub:
 ![Docker Pull]({{ site.baseurl }}/images/docker_pull.png "Docker Pull")

11. The last message to show up on the screen should contain a series of characters preceded by "token=".
For example:

![Jupyter Authentication Token]({{ site.baseurl }}/images/token.png "Jupyter Authentication Token")

In this case, the token you will need to access the software is: `9a497a929ab4ea87c0b00bedb976457d4bc7e407b20abf6c` (your actual token will be different from this one).


12. After you have launched the juypter server, in your browser, navigate to the public ip address of your GPU instance on port 80. You can find the public ip address from the EC2 dashboard: 

 ![Obtaining public ip]({{ site.baseurl }}/images/aws_publicip.png "Obtaining public ip")

 ![Logging in]({{ site.baseurl }}/images/aws_login_browser.png "Logging in")

13. Paste in the token you obtained in step 11. 

14. Once you have logged in, you will see files associated with the dragonn software. 

 ![Dragonn files]({{ site.baseurl }}/images/aws_notebook.png "Dragonn files")

15. There are three tutorials available:
    a. click on  `paper_supplement` -> `primer_tutorial.ipynb` to complete the tutorial referenced in the Dragonn manuscript:
     ![Primer tutorial]({{ site.baseurl }}/images/primer_tutorial.png "Primer tutorial")

    b. click on `examples` -> `tutorial1.ipynb` or `examples` -> `tutorial2.ipynb` to complete additional tutorials.

When you are finished with the Amazon instance, follow these steps to shutdown the instance (and avoid incurring extra usage fees): 

1. Navigate to the EC2 dashboard and select your Amazon instance. 

2. In the toolbar, select **Actions** --> **Instance State** --> **Stop**

  ![Stopping the Amazon instance]({{ site.baseurl }}/images/aws_shutdown.png "Stopping the Amazon instance")

3. Wait for your Amazon instance to shutdown. If you want to permanently delete the Amazon instance, select **Terminate** (instead of **Stop**).