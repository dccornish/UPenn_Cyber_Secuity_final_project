# UPenn_Cyber_Secuity_final_project
Attack, Defense, and Analysis of a Vulnerable Network | Cybersecurity Bootcamp, University of Pennsylvania

In this project, you will act as a security engineer supporting an organization's SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the security engineering team to investigate and confirm that newly created alerts are working.
If the alerts are working, you will then monitor live traffic on the wire to detect any abnormalities that aren't reflected in the alerting system. Then, you will report back your findings to the manager with appropriate analysis.

Overview
You are working as a Security Engineer for X-CORP, supporting the SOC infrastructure. The SOC analysts have noticed some discrepancies with alerting in the Kibana system and the manager has asked the Security Engineering team to investigate.
To start, your team needs to confirm that newly created alerts are working. Once the alerts are verified to be working, you will monitor live traffic on the wire to detect any abnormalities that aren't reflected in the alerting system.
You will then report back all your findings to both the SOC manager and the Engineering Manager with appropriate analysis.
You have two class days to complete this activity.

PART 1

Instructions
Start by configuring new alerts in Kibana. Once configured, you will test them by attacking a system.
Open the Defensive Report Template and complete it as you progress through the activity.

Configuring Alerts
Complete the following to configure alerts in Kibana:


Access Kibana at 192.168.1.100:5601


Click on Management > License Management and enable the Kibana Premium Free Trial.


Click Management > Watcher > Create Alert > Create Threshold Alert


Implement three of the alerts you designed at the end of Project 2.


You are free to configure any alerts you'd like, but you are recommended to start with the following:


Excessive HTTP Errors
WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes


HTTP Request Size Monitor
WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute


CPU Usage Monitor
WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes


Note: There are a few way to to view these log messages and their associated data. options.


First, you can see when alerts are firing directly from the Watcher screen.


As you attack Target 1, keep the watcher page open to view your alerts fire in real time.



To view network traffic associated with these messages, we need to create a new 'Index Pattern':


Click on Management > Index Patterns and click on the button for 'Create Index Pattern'.




Make sure to turn on the toggle button labeled 'Include System Indices' on the top right corner.



Create the pattern .watcher-history-*



After you have this new index pattern, you can search through it using the 'Discovery' page.



Enter result.condition.met in as search filter and you can see all the traffic from your alerts.





Attacking Target 1
Open the Offensive Report Template and complete it while you progress this activity.
You will need to run a few commands on Target 1 in order to ensure it forwards logs to Kibana. Follow the steps below:

Open the Hyper-V Manager.
Connect to Target 1.
Log in with username vagrant and password tnargav.
Escalate to root with sudo -s.
Run /opt/setup.

This enables Filebeat, Metricbeat, and Packetbeat on the Target VM if they are not running already.
Now that you've configured alerts, you'll attack a vulnerable VM on the network: Target 1.
Ignore the Target 2 machine at this time. If you complete the entire project with time to spare, ask your instructor for directions on attacking Target 2 and integrating it into your project.
Complete the following high-level steps:


Scan the network to identify the IP addresses of Target 1.


Document all exposed ports and services.


Enumerate the WordPress site. One flag is discoverable after this step.


Hint: Look for the Users section in the output.



Use SSH to gain a user shell. Two flags can be discovered at this step.


Hint: Guess michael's password. What's the most obvious possible guess?



Find the MySQL database password.


Hint: Look for a wp-config.php file in /var/www/html.



Use the credentials to log into MySQL and dump WordPress user password hashes.


Crack password hashes with john.


Hint: Start by creating a wp_hashes.txt with Steven and Michael's hashes, formatted as follows

user1:$P$hashvalu3
user2:$P$hashvalu3


Secure a user shell as the user whose password you cracked.


Escalate to root. One flag can be discovered after this step.

PART 2

Instructions
Connect to your Kali VM, launch Wireshark, and begin capturing on the eth0 interface using the steps above. Let the capture run for at least fifteen minutes and then stop it. As your capture runs, read the following overview:


The Security team requested this analysis because they have evidence that people are misusing the network. Specifically, they've received tips about:

"Time thieves" spotted watching YouTube during work hours.
At least one Windows host infected with a virus.
Illegal downloads.



A number of machines from foreign subnets are sending traffic to this network. Therefore, you will see many IP ranges in your capture.


Your task is to collect evidence confirming the Security team's intelligence.


Be sure to use display filters. In addition, be sure to record your work by adding comments to packets as you go.

For example, if you find a packet containing a username of interest, comment on the packet: Illegal Downloads: Contains Windows username.



Record your answers in the following Google Doc. This file will be submitted as a deliverable at the end of the project. You must make a copy of this file in order to edit it.

Network Analysis Report Template


Time Thieves
At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

They have set up an Active Directory network.
They are constantly watching videos on YouTube.
Their IP addresses are somewhere in the range 10.6.12.0/24.

You must inspect your traffic capture to answer the following questions in your Network Report:


What is the domain name of the users' custom site?


What is the IP address of the Domain Controller (DC) of the AD network?


What is the name of the malware downloaded to the 10.6.12.203 machine?

Once you have found the file, export it to your Kali machine's desktop.



Upload the file to VirusTotal.com.


What kind of malware is this classified as?



Vulnerable Windows Machines
The Security team received reports of an infected Windows host on the network. They know the following:

Machines in the network live in the range 172.16.4.0/24.
The domain mind-hammer.net is associated with the infected computer.
The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.
The network has standard gateway and broadcast addresses.

Inspect your traffic to answer the following questions in your network report:


Find the following information about the infected Windows machine:

Host name
IP address
MAC address



What is the username of the Windows user whose computer is infected?


What are the IP addresses used in the actual infection traffic?


As a bonus, retrieve the desktop background of the Windows host.



Illegal Downloads
IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, such as downloading operating systems. However, they have a strict policy against copyright infringement.
IT shared the following about the torrent activity:

The machines using torrents live in the range 10.0.0.0/24 and are clients of an AD domain.
The DC of this domain lives at 10.0.0.2 and is named DogOfTheYear-DC.
The DC is associated with the domain dogoftheyear.net.

Your task is to isolate torrent traffic and answer the following questions in your Network Report:


Find the following information about the machine with IP address 10.0.0.201:

MAC address
Windows username
OS version



Which torrent file did the user download?



