# AzureSIEMProject
A Tutorial for an Azure SIEM
IN this lab we will make a SIEM in microsoft Azure

There are three main components to this project

<ul>
<li>Honeypot/Target</li>
<li>Log Anylitcs Workspace</li>
<li>Microsoft Sentinal Workbook</li>
</ul>

 <h2>Honeypot/Target</h2>
 In this lab we will be taking the logs of failed rdp events for a host machine. Using an API to geolocate where the failed rdp request came from and showing where those requests came from on a map. 

 In order to set up the honeypotvm you go to Azure and under virtual machines we are going to make a new virtual machine.

 There are only two things we need to configure. The first being the name, locatoin, resource group, etc.

 <img src ="https://i.imgur.com/tyzLYFI.png" height="80%" width="80%"  >

 The other one is the under the network tab where we will use an advanced security group. We will then create a new security group and then edit it.

 <img>

 We will then delete rule that already exists 

 <img>

and create a new rule. This rule will allow for all traffic to come through.

<img>

<h2>Log Anylitcs Workspace</h2>

 Next we're going to create the log anylitics workspace.

In this screenshot you can see that in the middle of screen is elipses. If you click on that you will get the option to make a new anylitics workspace

<img>

Configuring a log anylitcs work space is pretty simple

<img>

<h2>Microsoft Sentinal</h2>

Go to Microsoft Sentinal and create one

<img>

This is simple just click the log analytics Workspace

<img>

Then click connect

<img>

<h2>Setting up logging</h2>

Go to the Microsoft Defender for Cloud then go to the Enviorment Settings.

<img>

Then allow the Server protection but there's no need for the SQL server protection

<img>

Go to data collection and change to all events.

<img>

<h2>Connecting log anylitcs workspace</h2>

Go to log anylitics workspace and choose the one that was made.

<img>

Then choose virtual machines on the left

<img>

Click the virtual machine we made 

<img>

Then connect it.

<h2>Remoting into the VM</h2>

Go to the vm and copy the ip

<img>

Copy that ip into remote desktop in and remote in using the details that was supplied when we created the VM.

<img>

Here i would make sure that you allow the clipboard between the two machines but not allow anything else especially drives. 

<img>

Go to the Powershell ISE and make a new script. The name doesn't matter the code you will copy from [here](github.com)

This script takes every failed rdp attempt that happens while the script is running it then sends the ip to the API to get it's geolocation finally it takes that information and stores it in <code>C:\ProgramData\failed_rdp.log</code>
Make sure you change the API key to one that you get from [here]{ipgeolocater.com}

<img>

Run the script then go to <code>C:\ProgramData\failed_rdp.log</code> and copy that information to your desktop

<img>

I would suggest that if you turn it off after a few requests come in before doing the next part.

<h2>Create Custom Logs</h2>

So we have the custom logs in a file on the VM. Now we need those to be read by the Log Anylitics Workspace.

Go to the Log Anylitcs Workspace and choose custom logs.

<img>

Next we will supply logs to train the Logs Anylitics Workspace

Use the file that is located on your own computer, that was got from the virtual machine.

<img>

In the collection path put where log is on the VM which is<code>C:\ProgramData\failed_rdp.log</code> 

<img>

In details i named it FAILED_RDP_WITH_GEO it will append _CL at the end of it.

<img>

<h2>Extracting the fields from the logs</h2>

Now we are going to extract the logs.

In the Log Anylitic Workspace go to Logs

<img>

There run the FAILED_RDP_WITH_GEO_CL in the KUBL interpreter. 

<img>

Then choose one of the logs and right click choose the extract fields option 

<img>

Now highlight the field and name it

<img>

After that it will show the extracted field and how it has been applied to the other logs. If everything worked it will look something like this.

<img>

If it doesn't work it will look like this

<img>

To fix it go to the wrong entry on the right click on the circle with the line through it in the top right corner of the entry and choose modify.

<img>

Then do it just like you did before

<img>

After a couple they should work 

<img>

You need to extract *These* fields 

<h2>Mapping the data</h2>

Finally we are going to see all of our hard work on a map. 

<img>

Go to Microsoft Sentinal and start a new work book

<img>

Make a new Query 

<img>

Then run this query 

<img>

Change the visualization to map 

<img>

And lastly configure the map with the proper latitude, longitude, and label

<img>

Then you should see something like this

<img>
