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

 In order to set up the virtual machine you go to Azure and under virtual machines; we are going to make a new virtual machine.

 There are only two things we need to configure. The first being the name, locatoin, resource group, etc.

 <img src ="https://i.imgur.com/tyzLYFI.png" height="50%" width="50%" >

 The other one is the under the network tab where we will use an advanced security group. We will then create a new security group and then edit it.

 <img src ="https://i.imgur.com/MR96VTE.png" height="50%" width="50%" >

 We will then delete the rule that already exists by clicking on the three dots to the right.

 <img src ="https://i.imgur.com/98guLqL.png" height="50%" width="50%" >

Then create a new rule. This rule will allow for all traffic to come through.

<img src ="https://i.imgur.com/sd7Ey9y.png" height="50%" width="50%" >


<h2>Log Anylitcs Workspace</h2>

 Next we're going to create the log anylitics workspace.

In this screenshot you can see that in the middle of screen is elipses. If you click on that you will get the option to make a new anylitics workspace.

<img src ="https://i.imgur.com/daGw75L.png" height="50%" width="50%" >

Configuring a log anylitcs work space is pretty simple.

<img src ="https://i.imgur.com/zEc0pS2.png" height="50%" width="50%" >

<h2>Microsoft Sentinal</h2>

Search Microsoft Sentinal in the search box and create it.


This is simple just click the log analytics workspace.

<img src ="https://i.imgur.com/6BCWkGB.png" height="50%" width="50%" >

Then click connect

<h2>Setting up logging</h2>

Go to the Microsoft Defender for Cloud then go to the Enviorment Settings.

<img src ="https://i.imgur.com/jcUtk23.png" height="50%" width="50%" >

Then allow the Server protection but there's no need for the SQL Server Protection.

<img src ="https://i.imgur.com/kuPKiVi.png" height="50%" width="50%" >

Go to data collection and change to all events.

<img src ="https://i.imgur.com/CmxRdCJ.png" height="50%" width="50%" >

<h2>Connecting log anylitcs workspace</h2>

Go to log anylitics workspace and choose the one that was made.

<img src ="https://i.imgur.com/CmxRdCJ.png" height="50%" width="50%">

Then choose virtual machines on the left

Click the virtual machine we made 

<img src = "https://i.imgur.com/wqQWQk0.png" height = "50%" width = "%50">

Then connect it.
<img src = "https://imgur.com/dwTSuSY" height = "%50" width = "%50">

<h2>Remoting into the VM</h2>

Go back to the Virtual Machine we created earlier and copy the it's IP address.

<img src = "https://i.imgur.com/UeH1MZD.png" height = "%50" width = "%50">

Copy that ip into remote desktop in and remote in using the details that was supplied when we created the VM.

<img src = "https://i.imgur.com/b6mru8q.png" height = "%50" width = "%50">

Here I would make sure that you allow the clipboard between the two machines but not allow anything else especially drives. 

Go to the Powershell ISE and make a new script. The name doesn't matter the code you will copy from [here](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1).

<img src = "https://i.imgur.com/ahs5Pzd.png" height = "%50" width = "%50">

This script takes every failed rdp attempt that happens while the script is running it then sends the ip to the API to get it's geolocation finally it takes that information and stores it in <code>C:\ProgramData\failed_rdp.log</code>
Make sure you change the API key in the script to one that you get from [here](https://app.ipgeolocation.io).

Run the script then go to <code>C:\ProgramData\failed_rdp.log</code> and copy that information to your desktop

I would suggest that if you turn it off after a few requests come in before doing the next part.

<h2>Create Custom Logs</h2>

So we have the custom logs in a file on the VM. Now we need those to be read by the Log Anylitics Workspace.

Go to the Log Anylitcs Workspace and choose custom logs.

<img src="https://i.imgur.com/ekMf1eM.png" height = "%50" width = "%50">

Next we will supply logs to train the Logs Anylitics Workspace

Use the file that is located on your own computer, that was got from the virtual machine.

<img src="https://i.imgur.com/xL9tGXf.png" height = "%50" width = "%50">

In the collection path put where log is on the VM which is<code>C:\ProgramData\failed_rdp.log</code> 

<img src = "https://i.imgur.com/4ih5fCy.png" height = "%50" width = "%50">

In details i named it FAILED_RDP_WITH_GEO it will append _CL at the end of it.

<img src = "https://i.imgur.com/SqWzegk.png" height = "%50" width = "%50">

<h2>Extracting the fields from the logs</h2>

Now we are going to extract the logs.

In the Log Anylitic Workspace go to Logs.

<img src = "https://i.imgur.com/A1Gwo9g.png" height = "%50" width = "%50">

There run the FAILED_RDP_WITH_GEO_CL in the KUSTO interpreter. 

<img src = "https://i.imgur.com/1jcOR0o.png" height = "%50" width = "%50">

Then choose one of the logs and right click choose the extract fields option.

<img src = "https://i.imgur.com/3bIyMUL.png" height = "%50" width = "%50">

Now highlight the field and name it.

<img src = "https://i.imgur.com/S4wgq2A.png" height = "%50" width = "%50">

After that it will show the extracted field and how it has been applied to the other logs. If everything worked it will look something like this.

<img src = "https://i.imgur.com/8D76WNs.png" height = "%50" width = "%50">

If it doesn't work it will look like this

<img src = "https://i.imgur.com/uUgT1n3.png" height = "%50" width = "%50">

To fix it go to the wrong entry on the right click on the circle with the line through it in the top right corner of the entry and choose modify.

<img src = "https://i.imgur.com/uUgT1n3.png" height = "%50" width = "%50">

Then do it just like we did before.

<img src = "https://i.imgur.com/vagb04U.png" height = "%50" width = "%50">

After a couple they should work 

<img src = "https://i.imgur.com/fSQD1fd.png" height = "%50" width = "%50">

You need to extract the label, country, latitude, longitude, source host, and destination host fields. 

<h2>Mapping the data</h2>

Finally we are going to see all of our hard work on a map. 

Go to Microsoft Sentinal and start a new work book.

<img src = "https://i.imgur.com/UP2CAhH.png" height = "%50" width = "%50">

Make a new Query.

<img src = "https://i.imgur.com/1c6MjjL.png" height = "%50" width = "%50">

Then run this query.
<code>
 FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF
| where destinationhost_CF != "samplehost"
| where sourcehost_CF != ""
 </code>

<img src = "https://i.imgur.com/CrSvl3d.png" height = "%50" width = "%50">

Change the visualization to map. 

<img src = "https://i.imgur.com/NHDeV7p.png" height = "%50" width = "%50">

And lastly configure the map with the proper latitude, longitude, and label

<img src = "https://i.imgur.com/zV0HCaX.png" height = "%50" width = "%50">

Then you should see something like this.

<img src = "https://i.imgur.com/xoMBo79.png" height = "%50" width = "%50">
