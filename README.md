# Enterprise Endpoint Management with OSQuery
<h2>Utilities Used</h2>

- <b>Docker</b>
- <b>CLI (Command Line Interface)</b>
- <b>OSQuery</b>
- <b>A little bit of PowerShell</b>
- <b>And a little bit of SQL</b>

<h2>Environments Used</h2>

- <b>Windows 10 (Host Computer)</b>
- <b>VirtualBox Hypervisor </b>
- <b>Kali Linux</b>

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
Docker on my Windows machine. I navigated to the “Fleet: Get Started” <a href="https://fleetdm.com/docs/using-fleet/learn-how-to-use-fleet" target="_blank">website</a> and followed the instructions on
installing Node.js and Docker into my Windows machine. As shown below I proceed to click the link to install Node.js
the quickest way by clicking on that link (or button) and choosing the 64-bit version of the Node.js Windows installer
that has the <b>.msi</b> extension.

<br/>
<img src="https://i.imgur.com/ELif6rd.png" height="70%" width="70%" alt=".msi extension for Docker and node.js"/>
<br />
Once I run the installation wizard and finish the GUI installment of Node.js a cmd window pops up and prompts me
to press any key to continue to upgrade and install additional tools such as <b>chocolately</b>. I do as I am instructed and
then a Windows PowerShell window pops up and starts installing upgrades and additional data.
<br />
<img src="https://i.imgur.com/jcFp6di.jpeg" height="70%" width="70%" alt="Installation shown thru PS"/>
<br />

It took about ten minutes to complete the additional installation(s). Where many extension upgrades and packages were downloaded as shown below. Once the additional tools have been installed, I entered “ENTER” in the command line to exit as instructed in the Windows PowerShell below: 
<br/>
<img src="https://i.imgur.com/tZc66ku.png" height="75%" width="75%" alt="ENTER"/>
<br />


To confirm the completed installation, I opened another PowerShell window as an administrator (admin) and
entered the command <b>“node -v”</b> to check which version of Node.js I had installed. Additionally, I entered <b>“node -
npm”</b> to view the destination of the installation of NPM.
<br />
<img src="https://i.imgur.com/N0YLfFp.png" height="60%" width="60%" alt="Node commands on PowerShell"/>
<br />


Throughout out the myriad of output from the installation in the PowerShell, I notice that it was recommended to
restart the machine after finishing the installation. Hence, I restart my Windows machine and continue to install
Docker in a similar process.


Now, I proceed to install Docker in a similar fashion through the same website with instructions on installing both
Node.js and Docker to run Fleet. I click on the link (or button) to install Docker the quickest way.


Before pressing the blue URL button (as shown below) to download Docker Desktop for Windows, I read through the
systems requirements and the rest of the information throughout the webpage. I must be aware that I will probably
have to update my Windows Subsystem for Linux (WSL) once installed to WSL 2 to effectively run docker and Fleet. I
go ahead and click the download link and once the installer file has finished downloading, I double click on it through
the browser and the installation wizard soon pops up.



<br />
<img src="https://i.imgur.com/n7CoXDh.png" height="50%" width="50%" alt="Downloading Docker"/>
<br />

<br />
<img src="https://i.imgur.com/n7CoXDh.png" height="60%" width="60%" alt="System Requirements"/>
<br />

It took nearly 2 mins for the unpacking of files and everything to install.

<br />
<img src="https://i.imgur.com/pnCdkhs.png" height="60%" width="60%" alt="Installing pkgs"/>
<br />


<br />
<img src="https://i.imgur.com/YRiBhwP.png" height="70%" width="70%" alt="Installing Docker Files"/>
<br />


Once the installation finished, I was prompted the message to accept the Software Service Agreement. Once I
accepted, the installation wizard for Docker prompted for the close and restart of the Windows machine.

<br />
<img src="https://i.imgur.com/rOMC99l.png" height="70%" width="70%" alt="Docker Software Service Agreement"/>
<br />

After restarting the machine, Docker Desktop booted up and I was instructed to install the kernel update for the WSL
2 Linux kernel because its installation was incomplete, as shown below.

<br />
<img src="https://i.imgur.com/PvpsfyE.png" height="40%" width="40%" alt="WSL 2 Incomplete"/>
<br />

Thus, I navigated to the website with the provided instructions and followed through it to enable the Windows
subsystem for Linux feature, virtual machine feature, download an WSL 2 engine updates package, set WSL 2 as the
default when installing a new Linux distribution and install a Kali Linux distribution. As shown below in the following
screenshots.

<br/>
<img src="https://i.imgur.com/RYCVPct.png" height="70%" width="70%" alt="Enable WSL on PS"/>
<br />
<img src="https://i.imgur.com/HWU2GhX.png" height="70%" width="70%" alt="Operation completed"/>
<br />


<br />
<img src="https://i.imgur.com/48cSq1L.png" height="75%" width="75%" alt="Install Kali Linux from Microsoft store"/>
<br />



To ensure that I implemented these instructions correctly, I open a PowerShell window as an admin and enter the
command <b>“wsl -l -v”</b> to view what version of WSL I have, which showed sufficient results. Still on the PowerShell, I
enter <b>“npm install -g fleetctl”</b> on the command line to install the Fleet command-line tool.
<br />
<img src="https://i.imgur.com/9bgwpRL.png" height="75%" width="75%" alt="WSL version"/>
<br />


From running that, it prompts me to run the command <b>“npm install -g npm@8.15.1”</b> to update npm for the correct
packages.
<br />
<img src="https://i.imgur.com/XHpKTRb.png" height="75%" width="75%" alt="npm install -g npm@8.15.1"/>
<br />


Once the update is complete, I move on to enter <b>“fleetctl preview”</b> into the command line and this error appears. It
seems that I cannot execute the command due to the Execution Policy of my PowerShell. I must reset it in a way that
I will be able to bypass my PowerShell’s Execution Policy and run the command, <b>“fleetctl preview”</b> to run a demo of
the Fleet server.

<br />
<img src="https://i.imgur.com/g80LMdW.png" height="65%" width="65%" alt="fleetctl preview error"/>
<br />
After doing some research on the execution policies in PowerShell, I must change the execution policy by entering
<b>“Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass”</b>. The <b>“Set-ExecutionPolicy”</b> cmdlet uses the
'ExecutionPolicy' parameter to specify the policy. The 'Scope' parameter specifies the default scope value, and the <b>“-
Execution Policy Bypass”</b> section of the cmdlet specifies the execution policy Bypass. Where nothing is blocked and
there are no warnings or prompts.
<br />
<img src="https://i.imgur.com/gy8OkTD.png" height="65%" width="65%" alt="Execution policies"/>
<br />
Once I executed the Execution Policy change, I run the <b>“fleetctl preview”</b> command again and this time it went
through. I had to power up Docker in the background for the command to run correctly because running the <b>“fleetctl
preview”</b> command without a Docker daemon running in the background will result in an error, as shown below.
<br />
<img src="https://i.imgur.com/hi1T9xx.png" height="55%" width="55%" alt="fleetctl preview"/>
<br />
I allowed access to my system’s resources to the Docker Desktop Backend to run the Fleet UI, where I can run some
queries on the Fleet OSQuery.

<br />
<img src="https://i.imgur.com/IBcdCcE.png" height="55%" width="55%" alt="allow access"/>
<br />

As shown below the execution was successful, I was able to run a local demo of the Fleet server.

<br />
<img src="https://i.imgur.com/jM2OsEd.png" height="75%" width="75%" alt="Established OSQuery Fleet"/>
<br />


By running “fleetctl preview” in admin mode in a PowerShell window it automatically adds my Windows host
machine as a host to the list of devices on the Fleet server as shown below.
<br />
<img src="https://i.imgur.com/u3SlvUF.png" height="75%" width="75%" alt="System added to Fleet server"/>
<br />
The Fleet UI (fleet for osquery dashboard) loaded up for me. Now I can attempt to run my own queries.
<br />
<img src="https://i.imgur.com/QPw4ozC.png" height="70%" width="70%" alt="Fleet for osquery dashboard"/>
<br />
I navigate to the “Queries” tab on the top of the Fleet UI interface to run my own queries there. On the page there
are queries for me to try listed as well. From here, I will run five queries and witness their results after running each
one.


<h3>Query 1</h3>
I’m going to start with listing my host machine’s processes by entering:
<br />
<img src="https://i.imgur.com/IlRpaJW.png" height="60%" width="60%" alt="SELECT * FROM processes"/>
<br />
I am selecting all the columns of data available within the ‘processes’ table using SQL while writing this query. From
running this query, so many results have appeared as shown below. This query lists for five columns the command line, elapsed time, name of the process, pid, and root of each listed process.
<br />
<img src="https://i.imgur.com/YXFVqUs.png" height="80%" width="80%" alt="Query 1 Results"/>
<br />

<h3>Query 2</h3>
For my second query, I want to see who is logged into the system by entering:
<br />
<img src="https://i.imgur.com/SN2yIDR.png" height="60%" width="60%" alt="SELECT * FROM logged_in_users"/>
<br />
From running this query, I am provided the columns associated with the ‘logged_in_users’ table such as the
hostname, pid, time, type, and tty of the process, as shown below.

<br />
<img src="https://i.imgur.com/xYPVkH2.png" height="80%" width="80%" alt="Query 2 results"/>
<br />

<h3>Query 3</h3>
For my third query, I want to know I what are the top 5 most memory intensive processes. I do this by entering the
following SQL query.
<br />
<img src="https://i.imgur.com/pQJrdGE.png" height="70%" width="70%" alt="Query 3"/>
<br />

In this query, I use the <b>SELECT</b> SQL clause to retrieve the pid, name, a numeric or percentage columns from the
processes table by using the <b>FROM</b> clause listing the ‘processes’ table and join information. The <b>ORDER</b> clause will sort the numerical results by ‘total_size’ in a descending order which is calculated within the <b>ROUND</b> clause, and since I want to only see the top 5 processes that use the most memory, I use the <b>LIMIT</b> clause to limit the query results by 5. This is shown in the screenshot below.

<br />
<img src="https://i.imgur.com/BPjtKFi.png" height="80%" width="80%" alt="Query 3 results"/>
<br />

<h3>Query 4</h3>
On my fourth query, I can list my system information by entering the following query to retrieve information on the
type of CPU, the brand, the hardware vendor and model of my Windows host machine from the ‘system_info’ table,
as shown below.
<br />
<img src="https://i.imgur.com/xRxrFfC.png" height="60%" width="60%" alt="select cpu_type,..."/>
<br />
Since I am running my queries on just one machine (my host), one result is listed from the query execution. Most of
the information from these columns on the resulting table I should know, except for the hardware model.

<br />
<img src="https://i.imgur.com/TNVVFTY.png" height="85%" width="85%" alt="Query 4 results"/>
<br />


<h3>Query 5</h3>
For my fifth query, I have an interest in compiling certain information listed out from the ‘drivers’ table on my
Windows machine. I will use the <b>SELECT</b> clause to retrieve information on the (hostname by default) device_name
and version extracted from the ‘drivers’ table using the <b>FROM</b> clause, as shown below.
<br />
<img src="https://i.imgur.com/NswesQv.png" height="60%" width="60%" alt="SELECT device_name, version FROM drivers"/>
<br />
Once I run the query, I am met with over 200 results listing the device names, hostname, and the version of the
driver on the table shown below.
<br />
<img src="https://i.imgur.com/QRd2mDt.png" height="75%" width="75%" alt="Query 5 results"/>
<br />
You can also check for to see if drivers are signed as well, as shown below.
<br />
<img src="https://i.imgur.com/FBeeACe.png" height="55%" width="55%" alt="Query 5.2"/>
<br />
IT and security administrators review drivers in this similar fashion to verify proper installation of required drivers
and ensuring that the correct versions of popular drivers are installed.


<br />
<img src="https://i.imgur.com/rSDvxsd.png" height="75%" width="75%" alt="Query 5.2 results"/>
<br />

<h3>Installing and Deploying OSQuery on Kali Linux</h3>
Now that I have essentially installed and deployed OSQuery on my Windows machine, I will now proceed to do the
same with my Kali Linux VM.


**Disclaimer** You might run into some issues while running VirtualBox after installing WSL 2.0 on your local Windows machine. To mitigate that we can disable <b>WSL 2.0</b> and enable the “Windows Hypervisor Platform” again from the Window Features menu. Once I do that, I restart my machine and things should run smoothly to install and run fleet on a Linux VM. We might have to switch back and forth between enabling and disabling while working on queries on the Windows machine. Essentially, for working on a Linux VM, I would re-enable the Windows Hypervisor platform feature every time to use a VM.



<br />
<img src="https://i.imgur.com/y6isC6C.png" height="55%" width="55%" alt="Re-enable Windows Hypervisor Platform"/>
<br />


Now I boot up my Kali Linux VM and will download Node.js, NPM, Docker, and Docker-Compose. First, I will install Node.js and NPM installer through the Kali Linux terminal by adding the Node.js repository to my system.

<br />
<img src="https://i.imgur.com/htxdQAL.png" height="55%" width="55%" alt="add node.js repo to Kali with curl command"/>
<br />
<br />

Once I added the node repository to my VM, I enter “<b>sudo apt update</b>” into the command line to essentially let my
Kali Linux machine know that I have added a new repository to the system.
<br />
<img src="https://i.imgur.com/A290nEx.png" height="55%" width="55%" alt="sudo apt update"/>
<br />


I have everything I need on my system to install Node.js. I proceed to install the latest Node.js version on my Kali
Linux machine by entering “<b>sudo apt install nodejs</b>” into the command line. To confirm the installation, I enter
“<b>node -v</b>” to show my current Node.js version. I assumed that from installing the Node.js packages it would include
the NPM package manager, but when I enter the command: “<b>npm -v</b>” to check the version, the command was not
found. However, the command line prompted me if I wanted to go ahead and install NPM, so I enter “y” to go ahead
and execute the “<b>sudo apt install npm</b>” command to install NPM. To confirm its installation, I enter “<b>npm -v</b>” for the most recent version of NPM to pop up, as shown below.

<br />
<img src="https://i.imgur.com/1wqTOr0.png" height="75%" width="75%" alt="confirm node and npm installation"/>
<br />


Now that I finished installing Node.js and NPM on my Kali Linux VM, I will proceed to install Docker and DockerCompose for my Linux machine for smooth installation of Fleet OSQuery on my Kali Linux VM.



<figure>
  <img src="https://i.imgur.com/YBtj11N.png" alt="sudo apt update && sudo..." style="width:70%">
  <figcaption>It took a solid 10 minutes for the installation to finish</figcaption>
</figure>
<br />
<br />


<br />
<img src="https://i.imgur.com/dwWvuvo.png" height="65%" width="65%" alt="more package installation"/>
<br />


Before installing Docker and Docker-Compose, I need to make sure that all the packages used by Docker as
dependencies are installed, as shown below.
<br />
<img src="https://i.imgur.com/WbOU63H.png" height="75%" width="75%" alt="confirm Docker and Docker-compose pkg installation"/>
<br />


Then I enter “<b>[ -f /var/run/reboot-required ] && sudo reboot -f</b>” to check if a reboot is required after the lengthy
upgrade. Next, I will import the Docker GPG key used for signing Docker packages by entering “<b>curl -fsSL
https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted/gpg.d/docker-archivekeyring.gpg</b>” into the command line. I will add the Docker repository which contains the latest stable releases of Docker Community Edition (CE) by entering the command “<b>echo "deb [arch=amd64] https://download.docker.com/linux/debian bullseye stable" | sudo tee /etc/apt/sources.list.d/docker.list</b>”. This command will add that repository URL to <b>/etc/apt/source.list.d/docker.list</b>
<br />
<img src="https://i.imgur.com/BA3CA0C.png" height="75%" width="75%" alt="echo deb arch=amd64..."/>
<br />


Before I’m ready to install Docker on Kali Linux, I make sure that I update the apt package index by entering “<b>sudo
apt update</b>” in the command line. Now, to install Docker CE on Kali Linux I run the command “<b>sudo apt install
docker-ce docker-ce-cli containerd.io</b>”, and enter the ‘y’ key to start the installation.
<br />
<img src="https://i.imgur.com/PMuaDfB.png" height="75%" width="75%" alt="sudo apt install..."/>
<br />


This installation will add docker group to the system without any users. I add my user account to the group to run
docker commands as a non-privileged user.
<br />
<img src="https://i.imgur.com/R1qNf7i.png" height="45%" width="45%" alt="sudo usermod -aG..."/>
<br />


Docker is installed, I confirm by entering “<b>docker version</b>” in the command line to view the Docker version and other
information as shown below.
<br />
<img src="https://i.imgur.com/j4DKaL1.png" height="75%" width="75%" alt="docker  version"/>
<br />


Now I want to install Docker Compose on my Kali Linux VM. First, I must ensure that I have the command line tools <b>curl</b> and <b>wget</b> for this operation. I do this by entering “<b>sudo apt update</b>” and “<b>sudo apt install -y curl wget</b>”. As shown below, it seems that I already have it installed. Now, I am ready to download the latest Docker Compose on my Linux machine. I do this by entering “<b>curl -s https://api.github.com/repos/docker/compose/releases/latest | grep browser_download_url | grep docker-compose-linux-x86_64 | cut -d '"' -f 4 | wget -qi -</b>” in the command line.
<br />
<img src="https://i.imgur.com/9oAOdpM.png" height="75%" width="75%" alt="sudo apt install -y curl wget"/>
<br />


I make the binary file executable by entering “<b>chmod +x docker-compose-linux-x86_64</b>”. I proceed to move the file
to my PATH by entering “<b>sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose</b>” into the
command line. Then I confirm the version. I have successfully installed the required prerequisites for running Fleet
OSQuery on my Kali Linux VM
<br />
<img src="https://i.imgur.com/tT9tizw.png" height="45%" width="45%" alt="docker-compose version"/>
<br />


Now I am ready to install Fleet OSQuery to run five queries on my Kali Linux machine. I want to go the same route I
went for my Windows machine, so on my terminal I will enter the command “<b>sudo npm install -g fleetctl</b>” to install
the packages needed for the Fleet server with an escalated root privilege. Once that is executed, I proceed further by
entering “<b>sudo fleetctl preview</b>” to start a server and open the Fleet UI webpage to run my own queries within it.
<br />
<img src="https://i.imgur.com/eW3yBnV.png" height="75%" width="75%" alt="look at Fleet preview"/>
<br />
The Fleet UI (fleet for osquery dashboard), as shown below loaded up for me. Now I can attempt to run my own
queries.
<br />
<img src="https://i.imgur.com/3Xt4aCq.png" height="75%" width="75%" alt="Fleet UI osquery dashboard for Kali"/>
<br />


I navigate the “Queries” tab on the top of the Fleet UI interface to run my own queries there. On the page there are
queries for me to try listed. Compatible with macOS, Windows, Linux, or all three. I select the “Create new query”
button to get started.
<br />
<img src="https://i.imgur.com/bjRLM88.png" height="65%" width="65%" alt="create a new query"/>
<br />
Just for kicks, I decide to run a query that was already in the Query compiler: 
<br />
<img src="https://i.imgur.com/NkL4sHw.png" height="65%" width="65%" alt="New query"/>
<br />
I run the query and from the results it shows me information about the targeted host (my Linux machine in this case)
such as the build_distro, hostname, version, pid, and more from the ‘osquery_info’ table, as shown below.
<br />
<img src="https://i.imgur.com/gRXMpJV.png" height="75%" width="75%" alt="New query results"/>
<br />

<h3>Query 1</h3>
To start myself off, I want a count of all the users on my Linux machine. I can retrieve this information from the
‘users’ table. I enter the query shown below.
<br />
<img src="https://i.imgur.com/nvTw2lA.png" height="60%" width="60%" alt="SELECT COUNT(*) FROM users"/>
<br />
As a result, there are 55 users within my Linux machine.
<br />
<img src="https://i.imgur.com/S4U2h2D.png" height="80%" width="80%" alt="Query 1 Results"/>
<br />

<h3>Query 2</h3>
I am going to run a query that retrieves the information of my available machine, associated with the ‘uptime’ table
as shown below..
<br />
<img src="https://i.imgur.com/bqegN6J.png" height="45%" width="45%" alt="SELECT * from uptime"/>
<br />
This query shows me the that it’s been almost five hours (at the time of writing this report) since my Kali Linux
machine has been rebooted now. A very interesting query I have executed here.
<br />
<img src="https://i.imgur.com/4CbWLNk.png" height="85%" width="85%" alt="Query 2 results"/>
<br />

<h3>Query 3</h3>
I want to look at some more information about my users inside my machine. I want to see the user IDs (uid),
usernames, description, and location from the ‘users’ table. I compile the following query below:
<br />
<img src="https://i.imgur.com/UfEDwyx.png" height="60%" width="60%" alt="Query 3"/>
<br />
I am using the <b>SELECT</b> clause to select these columns of data, such as username, uid, directory, etc. With the <b>from</b> clause, I grab this information from the ‘users’ table in my machine. Notice how I used the from clause as fully uppercased from what I did in Query 1; it makes me wonder if the SQL syntax is case sensitive. From here it lists all the credentials I asked for, listing the columns I requested from ‘users’ table.
<br />
<img src="https://i.imgur.com/WKEwa0r.png" height="90%" width="90%" alt="Query 3 results"/>
<br />
55 results are a bit too much to look at. I can do something to limit the number of results I get by appending a <b>LIMIT</b> clause into the query.

<h3>Query 4</h3>
I want to view fewer results of the selected criteria. I do this by appending a <b>LIMIT</b> clause to the query, as shown
below. I want to limit the number of results to display 4 users.
<br />
<img src="https://i.imgur.com/7lpdJmQ.png" height="65%" width="65%" alt="Query 4"/>
<br />
Thus, I am shown 4 users in my host machine (E99s) from the results.
<br />
<img src="https://i.imgur.com/sN6c52r.png" height="95%" width="95%" alt="Query 4 results"/>
<br />

<h3>Query 5</h3>
From looking at other references, I’m amazed to see all the possibilities that queries have to offer. I’ve executed
many commands while installing and setting the floor for this OSQuery server, so I would like to see my shell history.
I will search for executed commands on my system by entering:
<br />
<img src="https://i.imgur.com/8y4Izkj.png" height="45%" width="45%" alt="Query 5"/>
<br />
After running this query, there are 97 results listed. The table is filled with all the previous commands I executed
regarding installation of Node.js and Docker. Some commands were executed as a root user, and a majority were
executed as a regular user
<br />
<img src="https://i.imgur.com/2LRaViG.png" height="75%" width="75%" alt="Query 5 result"/>
<br />
<br />


<h3>Closing Thougts</h3>
OSQuery has the ability to execute schedule queries, real-time queries, and has an interactive mode. I've read that in
terms of keeping your network environment updated, you can schedule certain queries to retrieve information on
whether for example, if Windows updates are installed on an enterprises’ grand scale of machines. OSQuery's
cross-platform capabilities enables users to better understand and configure the best queries to run for different
respective systems such as Windows, macOS, Linux, and Docker APIs. This makes it reliable because you can
essentially apply somewhat similar queries across different systems with the same functionality. 


Additionally, OSQuery has safeguard functions. For example, if an organization were to run a query that would be too processor heavy OSQuery will terminate the agent and list that certain query on a banned list, and it restarts it to make sure that certain query won’t run unless you remove it from the blacklist of banned queries not allowed to run. The ability
to prevent someone from making a mistake twice is extremely flexible for an enterprise environment to mitigate
potential internal and external threats. Utilizing the data that OSQuery has to offer in an enterprise enforces a good
level of security. Organizations can customize various queries to check that certain settings are configured as
expected, or not as expected. In terms of learning, the near-infinite amount data one can receive from OSQuery
maximizes system logging efficiency by possibly creating alerts in real time. For security teams, it makes it drastically
flexible to get data back in real time and write alerts in SQL using OSQuery to better understand their enterprise
environment. With the tool’s wide breadth of functionality and ease of customization it enables security
professionals to create and add new functionality to their environment, utilizing SQL to write queries against tables
to retrieve information about not only standard cross platform systems, but virtualized infrastructure as well.


For example, a query could be written to flag servers with an escalated privilege or root login within certain time
frames. Security teams can use the access to that kind of user session data to indicate where and when specific
logins are occurring within an organization’s infrastructure. This is especially important when performing audits or
investigations on systems.


OSQuery is continuing to improve its technology by working on its compatibility with Windows. OSQuery on
Windows systems provide powerful analysis into its registry, Windows Management Instrumentation, and several
other components that were previously difficult to monitor with a single tool. Instead of attempting to replace
existing node security tooling, OSQuery strives to serve as an effective lightweight companion to provide visibility on
the areas of the OS that were ignored or missed by other solutions. On Windows, the Windows Registry OSQuery can also help with monitoring for hardware changes. Particularly important for high security environments, networks that have to protect classified information or for those IT departments who just want to know when someone plugs in a malware ridden USB device and navigating your way through virtual machines provides a path into the threshold of advance technical skills.
OSQuery’s registry table capabilities allow us to see the entire fleet’s registry values at any given point in time. With
flexibility, the registry table permits users to check for the existence or non-existence of a key before attempting to
check its value.

Overall, it was very impactful and insightful to research about OSQuery and what functionality the prominent
endpoint tool provides so far. I have learned a lot and still have much to learn about the near limitless capabilities of
OSQuery and navigating through the endpoint agent. This fresh knowledge will be a crucial factor into my career development towards anywhere in the cybersecurity field.


