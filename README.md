# Enterprise Endpoint Management with OSQuery
<h2>Utilities Used</h2>

- <b>Docker</b>
- <b>CLI (Command Line Interface)</b>
- <b>OSQuery</b>
- <b>A little bit of PowerShell</b>

<h2>Environments Used</h2>

- <b>Windows 10 (Host Computer)</b>
- <b>VirtualBox Hypervisor </b>

<h2>Lab Walk-through:</h2>
<p>I've explored a little about the critical administrative functions of system logging to configuring host-based
intrusion detection system (HIDS) agents to compile log-based events, monitor alerts, and change rules of OSSEC
web servers in an Linux environment. In this demonstration, I will take what I’ve learned in from a single system perspective and visualize how to manage that in an enterprise environment. In this project, OSQuery will be my agent or endpoint security tool in this exploration.
<br />

I plan to run an OSQuery agent on my local Windows 10 host machine and on my Kali Linux
virtual machine (VM). I will install the required Node.js and NPM packages and deploy a Docker/Fleet agent on both
my Windows and Kali Linux machines to install OSQuery on each respective machine. I will independently explore
the processes within OSQuery and try out, compare and document five queries for each system.
</p>
<br />
<h3>Installing and Deploying OSQuery on Windows</h3>
I will use my main, local Windows 10 host machine to install and deploy Fleet OSQuery. I begin to install Node.js and
Docker on my Windows machine. I navigated to the “Fleet: Get Started” website and followed the instructions on
installing Node.js and Docker into my Windows machine. As shown below I proceed to click the link to install Node.js
the quickest way by clicking on that link (or button) and choosing the 64-bit version of the Node.js Windows installer
that has the <b>.msi</b> extension.

<br/>
<img src="https://i.imgur.com/ELif6rd.png" height="70%" width="70%" alt=".msi extension for Docker and node.js"/>
<br />
<h4>Windows 10 Enterprise</h4>
Next, we'll download a <a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise" target="_blank">Windows 10 Enterprise</a> ISO file. Next, fill out your information (uncheck the box for additional communications) and select your language. Then, click <b>Download</b>. Now, we'll prepare the VMs.
<img src="https://i.imgur.com/B1CGAMt.png" height="50%" width="50%" alt="Windows 10 Enterprise ISO Download"/>
<br />

<h3>Windows Server 2019</h3>
Let's first configure our Windows Server 2019 VM as our domain controller. On our VirtualBox VM Manager, create a new VM by clicking the <b>New VM</b> button. Configure the following:
<br/>
<img src="https://i.imgur.com/q62IZCb.png" height="80%" width="80%" alt="VM Name and OS"/>
<br />
<img src="https://i.imgur.com/30MXVBB.png" height="80%" width="80%" alt="Hardware"/>
<br />
<img src="https://i.imgur.com/hAIh2aX.png" height="80%" width="80%" alt="Virtual Hard disk"/>
<br />
Click <b>Finish</b>, but don't start the VM yet!
<br />
<img src="https://i.imgur.com/3H0FGXs.png" height="50%" width="50%" alt="Network Settings Win Server 2019"/>
<br />
<img src="https://i.imgur.com/xhlKpnr.png" height="80%" width="80%" alt="Win Server 2019  Network Adapters"/>
<br />

<h3>Windows 10 Enterprise Template</h3>
Now, we'll create a new VM and give it a name such as <b><i>Win10EnterpriseTemplate</i></b>.
<br/>
<img src="https://i.imgur.com/5NUCs6B.png" height="80%" width="80%" alt="Create Win10EnterpriseTemplate"/>
<br />
<img src="https://i.imgur.com/qcZ29aP.png" height="80%" width="80%" alt="Hardware"/>
<br />
<img src="https://i.imgur.com/XLLFNp9.png" height="80%" width="80%" alt="Virtual Hard disk"/>
<br />
Again, click <b>Finish</b>, but do not start the VM yet! We'll go to the network settings of the VM.
<br/>
<img src="https://i.imgur.com/z236WTf.png" height="60%" width="60%" alt="Win10Enterprise Network Settings"/>
<br />
<img src="https://i.imgur.com/4EKyJtJ.png" height="80%" width="80%" alt="Win10EnterpriseTemplate Network Adapter"/>
<br />
Make sure to save the settings of the VM and now we'll start the installation of the operating systems.
<br />
<h3>Windows Server 2019</h3>
We'll hover over our newly created Windows Server 2019 VM and double-click to start the VM. Once booting finishes, Choose languge > Click <b>Install Now</b>.
<img src="https://i.imgur.com/Gnh0EGN.png" height="75%" width="75%" alt="Choose Language"/>
<br />
Choose <b>Windows Server 2019 Standard Evaluation (Desktop Experience)</b>
<img src="https://i.imgur.com/e9H4q1x.png" height="75%" width="75%" alt="Choose Standard Evaluation (desktop experience)"/>
<br />
We'll click <b>Next</b> and accept the terms and conditions. Then, we'll choose a <b>Custom</b> installation of Windows.
<br />
<img src="https://i.imgur.com/Zgy0KNX.png" height="75%" width="75%" alt="Custom: Install Windows..."/>
<br />
Click <b>Next</b> and wait for the installation to finish.
<br />
<img src="https://i.imgur.com/iDtp03c.png" height="55%" width="55%" alt="Installing Windows"/>
<br />
Enter a local administrator password and keep it in your records.
<br />
<img src="https://i.imgur.com/YfhVRHt.png" height="75%" width="75%" alt="Customize Settings"/>
<br />
Now, with VirtualBox, we'll click <b>Input</b> > <b>Keyboard</b> > <b>CTRL + ALT + DEL</b> to enter <b>CTRL + ALT + DEL</b> into the VM, and log in with your local Administrator password.
<br />
<img src="https://i.imgur.com/8JxQdlk.png" height="75%" width="75%" alt="Administrator Login"/>
<br />
Now, we'll configure the Server's Network Interface. In my environment, the pfSense DHCP service has been disabled for the AD (Active Directory) lab LAN, because we want the domain controller to act as the DHCP server for the client Windows computers. Therefore, we'll need to manually configure the domain controller.

<br />
To get started, on the Windows Server 2019 VM, we'll right-click the network interface icon and choose <b>Open Network & 
 Internet Settings</b>
 
<br />
<img src="https://i.imgur.com/2DV62M9.png" height="15%" width="15%" alt="Network Interface Icon"/>
<br />
<img src="https://i.imgur.com/pT0crPJ.png" height="35%" width="35%" alt="Open Network & Internet Settings"/>
<br />
Scroll down and choose <b>Change adapter options</b>
<br />
<img src="https://i.imgur.com/Kxz9gZD.png" height="50%" width="50%" alt="Change adapter options"/>
<br />
Right-click the adapter and choose <b>Properties</b>
<br />
<img src="https://i.imgur.com/JfJF1qu.png" height="60%" width="60%" alt="Properties"/>
<br />
Double-click <b>Internet Protocol Version 4 (TCP/IPv4)</b>
<br />
<img src="https://i.imgur.com/qhEDaJG.png" height="80%" width="80%" alt="IPv4"/>
<br />
We'll configure the adapter as follows:
<br />
<img src="https://i.imgur.com/bvM9p9A.png" height="80%" width="80%" alt="Use the following IP address"/>
<br />
So, for the DNS Server...two things:

- First, check with the DNS server running on the domain controller (we'll install it in a bit)
- If the DNS server doesn't know the answer, it will forward the DNS query to the default gateway and pfSense will resolve it


<h3>Rename the Server</h3>
Now we'll rename our Windows Server 2019 by clicking the <b>Start Menu</b> and click <b>Settings</b>
<br />
<img src="https://i.imgur.com/WbiYkia.png" height="40%" width="40%" alt="Settings > System"/>
<br />
<img src="https://i.imgur.com/1lwhOUj.png" height="40%" width="40%" alt="About"/>
<br />
<img src="https://i.imgur.com/YBoIL9J.png" height="40%" width="40%" alt="Rename this PC"/>
<br />
Enter DC1 as the name for the domain controller
<br />
<img src="https://i.imgur.com/R0jMYtX.png" height="75%" width="75%" alt="Rename to DC1"/>
<br />
Choose <b>Restart Now</b>. If a reason is required, choose <b>Other (planned)</b>.

<h3>Take Snapshot of the VM</h3>
In the VirtualBox VM manager, next to the Windows Server 2019 machine, click the menu icon and choose <b>Snapshots</b>.
<br />
<img src="https://i.imgur.com/wkqbEyO.png" height="60%" width="60%" alt="Choose Snapshots"/>
<br />
Click the <b>Take</b> button
<br />
<img src="https://i.imgur.com/pVTeu44.png" height="25%" width="25%" alt="Take button"/>
<br />
We can fill out the Snapshot entry with something like this:
<br />
<img src="https://i.imgur.com/8SjDYYN.png" height="45%" width="45%" alt="Description"/>
<br />
Click <b>OK</b>. Now, we can restore this snapshot any time if we want to roll back to a pre-domain install.

<h3>Configure Domain Services</h3>
Now, we'll reboot our DC1 (Windows Server 2019) VM. Once it is on, click <b>Manage</b> > <b>Add Roles and Features</b>
<br />
<img src="https://i.imgur.com/VQYqYo1.png" height="55%" width="55%" alt="Manage"/>
<br />
Click <b>Next</b> > <b>Next</b> > <b>Next</b> until you reach <b>Server Roles</b>. Check the following boxes:

- <b>Active Directory Domain Services</b>
- <b>DNS Server</b> (so we can resolve the domain controller by DNS name)

<figure>
  <img src="https://i.imgur.com/e4TXHcF.png" alt="Click Add Features" style="width:70%">
  <figcaption>Click Add Features</figcaption>
</figure>
<br />
<br />

<figure>
  <img src="https://i.imgur.com/FKwNcLV.png" alt="Click Add Features" style="width:70%">
  <figcaption>Click Add Features</figcaption>
</figure>

<br />

<br />
Click <b>Next</b> > <b>Install</b>. Wait for the installation to finish and click <b>Close</b>
<br />
<img src="https://i.imgur.com/1KiGLj6.png" height="75%" width="75%" alt="Feature Installation"/>
<br />
<h3>Configure Active Directory Domain Services</h3>
Log back into the domain controller (if you've taken a break) as the local administrator and wait for the Server Manager app to load. On the top ribbon of the Dashboard, click the flag icon with the alert:
<br />
<img src="https://i.imgur.com/dPQMT2a.png" height="20%" width="20%" alt="Flag"/>
<br />
<figure>
  <img src="https://i.imgur.com/ErVyPyZ.png" alt="Click Promote this server to a DC" style="width:50%">
  <figcaption>Click <b>Promote this server to a domain controller</b></figcaption>
</figure>
<br />
<br />
Choose <b>Add a new forest</b> and specify a <b>root domain name</b>. I chose <b>ad.lab</b> as my domain name, but you can choose any other local TLD (e.g., .com, .org, .net). Also, it is best to avoid using <b>.local</b> because that can interfere with multicast traffic.
<br />
<img src="https://i.imgur.com/IZ5JcmE.png" height="60%" width="60%" alt="add a new forest"/>
<br />
Click <b>Next</b>. The default options are fine. Specify a <b>restore password</b>. You can use the same password as the local admin or something different. It doesn't matter. Click <b>Next</b>.
<img src="https://i.imgur.com/21eNC4n.png" height="70%" width="70%" alt="DSRM"/>
<br />
<figure>
  <img src="https://i.imgur.com/7h9ourc.png" alt="DNS delegation" style="width:30%">
  <figcaption>Ignore the message</figcaption>
</figure>
<br />
<br />
Click <b>Next</b> and continue with the defaults
<br />
<img src="https://i.imgur.com/y1EHwNu.png" height="65%" width="65%" alt="Specify the location..."/>
<br />
Looks good. Click <b>Install</b> and wait for it to complete.
<br />
<img src="https://i.imgur.com/tVxnyei.png" height="75%" width="75%" alt="Install"/>
<br />
The server will automatically reboot afterwards. Be patient, it might take a while.
<br />
<img src="https://i.imgur.com/iEU6kYR.png" height="60%" width="60%" alt="rebooting"/>
<br />
<h3>Configure Active Directory Certificate Services</h3>
Active Directory Certificate Services will be installed to enable LDAPs. Log back into the domain controller as the local administrator and wait for the Server Manager application to load. Once it's open, go to <b>Manage > Add Roles and Features</b>.
<br />
<img src="https://i.imgur.com/gr5XxN3.png" height="45%" width="45%" alt="Add Roles and Features"/>
<br />
Click <b>Next</b> > <b>Next</b> > <b>Next</b> > Choose <b>Active Directory Certificate Services</b>
<br />
<figure>
  <img src="https://i.imgur.com/IJ2NdWs.png" alt="DNS delegation" style="width:60%">
  <figcaption>Click <b>Add Features</b></figcaption>
</figure>
<br />
<br />
<br />
Click <b>Next</b> > <b>Next</b> > <b>Next</b> > <b>Next</b>. For AD CS, choose the <b>Certificate Authority</b> role service.
<br />
<img src="https://i.imgur.com/M8rXjmM.png" height="80%" width="80%" alt="Certification Authority"/>
<br />
<img src="https://i.imgur.com/fi3FPXs.png" height="80%" width="80%" alt="View Installation Progress"/>
<br />
Click on the alert icon and click on the text to <b>Configure Active Directory Certificate Services</b>
<br />
<img src="https://i.imgur.com/ImVmtem.png" height="60%" width="60%" alt="Alert"/>
<br />
Click <b>Next</b>, then select the service role to configure. Click <b>Next</b>.
<br />
<img src="https://i.imgur.com/ZvmYyH0.png" height="70%" width="70%" alt="Select Role services to configure"/>
<br />
Choose <b>Enterprise CA</b> and click <b>Next</b>.
<br />
<img src="https://i.imgur.com/kI4kSFc.png" height="70%" width="70%" alt="Enterprise CA"/>
<br />
We're just going to use the default settings. Click <b>Next</b> > <b>Next</b> > <b>Next</b> > <b>Next</b> > <b>Next</b> > <b>Next</b> > <b>Configure</b>

<h3>Configure DNS Forwarders</h3>
The DNS server running on the domain controller will act as a resolver for the ad.lab domain (or whichever local domain you chose). We need a forwarder for any DNS query for which the DNS server does not know the answer. We can use the pfSense default gateway as a downstream DNS server that the domain controller can pass queries to for any unknown hostnames.
<br />
<br />
Still on the DC1 VM, open up the Start Menu and search for <b>DNS</b>
<br />
<img src="https://i.imgur.com/650QBqg.png" height="25%" width="25%" alt="DNS"/>
<br />
Expand <b>DNS</b> > <b>DC1</b> and double-click <b>Forwarders</b>
<br />
<img src="https://i.imgur.com/u4Sn7DG.png" height="75%" width="75%" alt="Forwarders"/>
<br />
Click <b>Edit</b> and add the IP address of the default gateway (mine is the following). Click <b>OK</b>.
<br />
<img src="https://i.imgur.com/u4Sn7DG.png" height="55%" width="55%" alt="Forwarders"/>
<br />
<img src="https://i.imgur.com/wErn29S.png" height="75%" width="75%" alt="Edit Forwarders"/>
<br />

<h3>Add and Configure a DHCP Server</h3>
Next, we'll open the Server Manager and go to <b>Manage</b> > <b>Add Roles and Features</b>
<br />
<img src="https://i.imgur.com/gr5XxN3.png" height="45%" width="45%" alt="Add Roles and Features"/>
<br />
Click <b>Next</b> > <b>Next</b> > <b>Next</b>
<br />
<br />
Click <b>DHCP Server</b>
<br />
<br />
<figure>
  <img src="https://i.imgur.com/cvbRxBm.png" alt="DHCP Server" style="width:60%">
  <figcaption>Click <b>Add Features</b> and click <b>Next</b> > <b>Next</b> <b>Next</b> > <b>Install</b></figcaption>
</figure>
<br />
<br />
<br />
Once the installation is complete, click on <b>Complete DHCP Configuration</b>
<br />
<img src="https://i.imgur.com/xSB0jCs.png" height="75%" width="75%" alt="DHCP Feature Installation"/>
<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/cvbRxBm.png" alt="DHCP Server" style="width:60%">
  <figcaption>Click <b>Next</b> > <b>Commit</b> > <b>Close</b> > <b>Close</b></figcaption>
</figure>
<br />
<br />
<br />

Go to the <b>Start Menu</b> and search <b>DHCP</b>
<br />
<img src="https://i.imgur.com/tjRnMsk.png" height="25%" width="25%" alt="DHCP"/>
<br />
Expand the DHCP server tree and right-click <b>IPv4</b> and choose <b>New Scope</b>
<br />
<img src="https://i.imgur.com/eJP0AQj.png" height="45%" width="45%" alt="New Scope"/>
<br />
Click <b>Next</b> and give your DHCP configuration a name and description. Then, click <b>Next</b>.
<br />
<img src="https://i.imgur.com/efbEwHf.png" height="55%" width="55%" alt="Name & Description"/>
<br />
Configure the <b>DHCP address space</b> and <b>subnet mask</b>. Then, click <b>Next</b>.
<br />
<img src="https://i.imgur.com/oquEzTY.png" height="55%" width="55%" alt="Address space and subnet mask"/>
<br />
We're not configuring any DHCP exclusions (reservations), so click <b>Next</b>. We'll make it so clients' leases are good for one year. Click <b>Next</b>.
<br />
<img src="https://i.imgur.com/ju3GKVr.png" height="75%" width="75%" alt="Lease Duration"/>
<br />
Click <b>Next</b> to configure it now
<br />
<img src="https://i.imgur.com/Psgs4Cv.png" height="75%" width="75%" alt="Configure it now"/>
<br />
Enter the address of the default gateway and click <b>Add</b>.
<br />
<img src="https://i.imgur.com/1iaYiYx.png" height="75%" width="75%" alt="Click Add"/>
<br />
The default DNS configuration for DHCP clients is good here. Click <b>Next</b>.
<br />
<img src="https://i.imgur.com/bIOpayu.png" height="75%" width="75%" alt="DNS Servers"/>
<br />
We don't have a WINS server in our lab environment. Click <b>Next</b>.
<br />
<img src="https://i.imgur.com/RjMI06j.png" height="75%" width="75%" alt="No WINS Servers"/>
<br />
Click <b>Next</b> to activate the DHCP scope and click <b>Finish</b>.
<br />
<img src="https://i.imgur.com/UOg1LBK.png" height="45%" width="45%" alt="Activiate Scope now"/>
<br />

<h3>Add a Domain Administrator Account</h3>
Still on our domain controller (DC1), go to the <b>Start Menu</b> and search for <b>Active Directory Users and Computers</b> and open the app.
<br />
<img src="https://i.imgur.com/OUEd1t4.png" height="45%" width="45%" alt="Active Directory Users and Computers"/>
<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/Kyb5lF6.png" alt="Right click for New User" style="width:50%">
  <figcaption><b>ad.lab</b> > <b>Right-click Users</b> > <b>New</b> > <b>User</b></figcaption>
</figure>
<br />
<br />
<br />

<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/U36Ap5u.png" alt="New User details" style="width:60%">
  <figcaption>Fill out the fields with the User details</figcaption>
</figure>
<br />
<br />
<br />

<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/XD01EsV.png" alt="Password options" style="width:60%">
  <figcaption>Set the password and password options</figcaption>
</figure>
<br />
<br />
<br />
We've just created a new user which will be another administrator account for the AD forest. We will now assign him to the 'Domain Admins' group.
Click <b>Users</b> > <b>Domain Admins</b>
<br />
<img src="https://i.imgur.com/2DWI3hO.png" height="25%" width="25%" alt="Domain Admins"/>
<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/SzIOYxr.png" alt="Password options" style="width:50%">
  <figcaption>Enter the domain administrator username and click <b>Check Names</b>. Click <b>OK</b> > <b>OK</b></figcaption>
</figure>
<br />
<br />
<br />
<br />
<img src="https://i.imgur.com/sJKd32q.png" height="65%" width="65%" alt="Domain Admins Properties"/>
<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/2mfML45.png" alt="Sign out" style="width:40%">
  <figcaption>Sign out of the local administrator account</figcaption>
</figure>
<br />
<br />
<br />
<br />

<h3>Add Some Users to the Lab</h3>
Log in as the new domain administrator
<br />
<img src="https://i.imgur.com/3eZotPY.png" height="75%" width="75%" alt="Log in as new admin"/>
<br />
<br />

Once logged in, go to the <b>Start Menu</b>. Search for <b>Active Directory Users and Computers</b> and open the app.
<br />
<img src="https://i.imgur.com/OUEd1t4.png" height="45%" width="45%" alt="Active Directory Users and Computers"/>
<br />

<br />
<br />
<figure>
  <img src="https://i.imgur.com/Kyb5lF6.png" alt="Right click for New User" style="width:50%">
  <figcaption><b>ad.lab</b> > <b>Right-click Users</b> > <b>New</b> > <b>User</b></figcaption>
</figure>
<br />
<br />
<br />
<h4>John Doe</h4>
<br />
<img src="https://i.imgur.com/wMuNVIm.png" height="65%" width="65%" alt="John Doe User Details"/>
<br />
<br />
<img src="https://i.imgur.com/ZlxWYLa.png" height="65%" width="65%" alt="John Doe User Password & Passwd opts."/>
<br />

<h4>Jane Doe</h4>

<br />
<img src="https://i.imgur.com/83IwDko.png" height="65%" width="65%" alt="Jane Doe User Details"/>
<br />
<br />
<img src="https://i.imgur.com/1Aau9W4.png" height="65%" width="65%" alt="Jane Doe User Password & Passwd opts."/>
<br />

<h3>Windows 10 Enterprise Template</h3>
We'll navigate back to VirtualBox VM manager and power on the Windows 10 Enterprise Template VM.
<br />
<br />
Choose your language and click <b>Next</b>
<br />
<img src="https://i.imgur.com/BCToWpd.png" height="65%" width="65%" alt="Choose Language"/>
<br />

Choose <b>Install Now</b> and accept the terms and conditions. Choose <b>Custom: Install Windows Only</b>.
<br />
<img src="https://i.imgur.com/XDHodFw.png" height="55%" width="55%" alt="Custom"/>
<br />

<br />
Click <b>Next</b>. Wait for the installation to finish.
<br />
<img src="https://i.imgur.com/SbWlTdA.png" height="65%" width="65%" alt="Installing Windows"/>
<br />

<br />
Select your regional and language settings. Choose <b>Domain join instead</b>
<br />
<img src="https://i.imgur.com/4mR3BKn.png" height="25%" width="25%" alt="Domain join instead"/>
<br />

<br />
Enter the username Template, as this is going to be our template VM.
<br />
<img src="https://i.imgur.com/aJ3jTv9.png" height="75%" width="75%" alt="Template"/>
<br />

<br />
Enter a password and set security questions. Save the information in your records.
<br />
<br />


Turn off all the services here:
<br />
<img src="https://i.imgur.com/MRaWJqr.png" height="75%" width="75%" alt="Disabled services"/>
<br />

Choose <b>Not now</b> for Cortana.
<br />

<h3>Sysrep the Template</h3>
***We want to run <b>sysrep</b> to create a template VM, so that when we clone the VM, the Windows systems will always have a unique SID when joining the domain.***

<br />
To get started, let's log into the system using the template credentials and open a PowerShell terminal as administrator.

<br />
Run the command
<br />
<img src="https://i.imgur.com/XeQEDdM.png" height="75%" width="75%" alt="PS command"/>
<br />

<br />
Click <b>OK</b>. Let the sysprep process run to completion. The VM should shutdown.
<br />

<h3>Windows 10 Enterprise VM 1</h3>
Right-click the <b>Windows 10 Enterprise Template</b> and choose <b>Clone</b>. Give the cloned VM a name such as <b><i>Win10Ent1</i></b>. Click <b>Clone</b> and move on to the next one.
<br />
<img src="https://i.imgur.com/9AvLfDB.png" height="75%" width="75%" alt="Win10Ent1"/>
<br />

<h3>Windows 10 Enterprise VM 2</h3>
Same thing goes for another clone. Right-click the <b>Windows 10 Enterprise Template</b> and choose <b>Clone</b>. Give the cloned VM a name such as <b><i>Win10Ent2</i></b>. Click <b>Clone</b> and wait for the process to complete

<br />
<h3>Joining the Computers to the Domain</h3>
I will only demonstrate this process on one of the VMs. Follow along and repeat this process on any other clients you would want to join to the domain.
<h4>Windows 10 Enterprise VM 1</h4>
**Because we selected the Out-of-Box-Experience (OOBE) when running sysprep on the template — which is the correct choice — we are required to run the through the Windows setup again as a newly issued computer.
<br />

This is essentially the same thing as receiving a newly imaged Windows computer from your employer and joining it to the local domain.**
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/5D3ABko.png" alt="Domain join instead" style="width:70%">
  <figcaption>Choose <b>Domain join instead</b></figcaption>
</figure>
<br />
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/JP8wuQc.png" alt="Domain join instead" style="width:70%">
  <figcaption>Set up a password and security questions. Save them for later use.</figcaption>
</figure>
<br />
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/vhmMInF.png" alt="Disable services" style="width:70%">
  <figcaption>Once again, disable these settings</figcaption>
</figure>
<br />
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/Uci7qzy.png" alt="Disable services" style="width:50%">
  <figcaption>Choose <b>Not now</b> for Cortana</figcaption>
</figure>
<br />
<br />


<br />
If you're prompted to allow your PC to be discoverable in the network, choose <b>Yes</b> for the sake of this lab. Once you're logged in, go to the <b>Start Menu</b> > <b>Search for This PC</b> > <b>Right-click</b> > <b>choose Properties</b>
<br />
<img src="https://i.imgur.com/5wslnT9.png" height="25%" width="25%" alt="This PC"/>
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/9bawckY.png" alt="Advance sys settings" style="width:40%">
  <figcaption>Go to <b>Advanced system settings</b></figcaption>
</figure>
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/UaPfFnv.png" alt="Change" style="width:70%">
  <figcaption>Click <b>Change</b></figcaption>
</figure>
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/Fh2Ytdi.png" alt="More" style="width:70%">
  <figcaption>Click <b>More</b></figcaption>
</figure>
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/ajbAV6z.png" alt="DNS suffix" style="width:70%">
  <figcaption>Enter your local <b>DNS suffix</b> for your AD domain</figcaption>
</figure>
<br />
<br />

<br/>
Name your computer whatever you like and Enter your AD local domain
<br />
<img src="https://i.imgur.com/fN3lm2L.png" height="45%" width="45%" alt="Win10Ent1 and Domain ad.lab"/>
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/Wnnm30v.png" alt="Enter credentials" style="width:45%">
  <figcaption>Enter the domain controller credentials</figcaption>
</figure>
<br />
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/n080Aqo.png" alt="Success" style="width:45%">
  <figcaption>Success!</figcaption>
</figure>
<br />
<br />

<br />
<img src="https://i.imgur.com/WZAVK9n.png" height="30%" width="30%" alt="Networks"/>
<br />


<br />
<br />
<br />
<figure>
  <img src="https://i.imgur.com/Xcj6axt.png" alt="Success" style="width:45%">
  <figcaption>Choose <b>Other User</b> > Log in as a domain user</figcaption>
</figure>
<br />
<br />


<br />
<img src="https://i.imgur.com/escNIPY.png" height="35%" width="35%" alt="Jane Doe logging in"/>
<br />

<br />
Pull up PowerShell and look at the output from this <b><i>whoami</i></b> command
<br />
<img src="https://i.imgur.com/mRGIlli.png" height="65%" width="65%" alt="PS whoami"/>
<br />


Nice! We now have a domain controller and two Windows 10 Enterprise clients joined to the domain controller.
<br />
