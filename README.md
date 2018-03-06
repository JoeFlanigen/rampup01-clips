<h1>Ramping up on Powershell & CLI 2.0<h1>

<h3>“For the things we have to learn before we can do them, we learn by doing them.”  Aristotle</h2>

<p>While I doubt that Aristotle was talking about Azure his wisdom sure holds true for me. In my experience with Azure the best way to learn is just to start building something.  
 
<p>The following labs I developed as an exercise to teach Powershell and CLI for one of my customers. This customer wanted to cover things like Virtual Networks, Network Security Groups, Resource Groups, Load Balancers vs Application Gateways and how to setup failover recovery and other scenarios. But they didn't want to just learn about them they wanted to do lots of labs. Plus this class was going to have half of the students on Windows machines and half on Mac's wanting to do both Powershell and CLI. The following is the first set of labs in a journey of labs we did together.

<h2>The Scenario </h2>

<p>In these labs you will create hosted websites on virtual machines in both the East US and West US regions with a solution to manage traffic routing and failover between the two regions. 
<ul>
  <li>Traffic Manager --> East & West Regions</li>
  <li>East & West US Regions</li>
  <ul>
   <li>Resource Group</li>
   <li>App Gateway in East </li>
   <li>Azure Load Balancer in West</li>
   <li>VNETs/Subnets</li>
   <li>2 VM's in an Availability Set</li>
  </ul>
</ul>

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/visio.png">

<h1>Prerequisites </h1>
<p>If you haven't done it already, there is some stuff you need to make sure you have installed before we begin on the lab. 
<p><b>Windows, use the Webplatform Installer:</b>
https://www.microsoft.com/web/downloads/platform.aspx
<br>Download and run the application. This will enable you to install Microsoft Azure Powershell latest version

<p><b>Azure CLI 2.0 (Mac or Windows)</b>
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
<h3>Pick your Editor</h3>
If you are just getting started with Azure and don't know what tools to use to run the scripts I would recommend the following for your choice of Editors.
<ul>
<li>For Windows use Powershell ISE and setup the split window with your script in one window, and the terminal in the other. Highlight one line at a time and run the selection by selecting F8.  In windows, you can use Powershell ISE with either the Powershell lab, or the CLI lab, both will work in ISE.</li>

<li>For Mac users, I'd recommend using VS Code and setup the same split window.  You want to add the Azure CLI extension and save your file with the .azcli.  Then you can highlight your code and do the same process.  Rumor is you can also use VS Code to run Powershell on your mac but I haven't spent the time to try that.  VS Code also works on Windows. So you can do that as well.</li> 
</ul>
<p>One more thing, of course you will need an Azure Subscription. If you don't have one, you can sign up for a free trial at www.azure.com. 

<h2>Download the Labs and Get Going</h2>
<p>Download the following labs from my github unzip. 
 <ul>
  <li>Repository: https://github.com/dkj0/rampup01-clips </li>
  <li>Link to CLI: https://github.com/dkj0/rampup01-clips/blob/master/cli-lab-agw-alb-tm.azcli
   <li>Link to PS: https://github.com/dkj0/rampup01-clips/blob/master/ps-lab-agw-alb-tm.ps
  </ul>
<p>You can open the files as text files and copy and paste them into your editor, not terminal.  When you are ready to go, it should look like this.

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/vscode-mac-lab.png">
<br><small>VS Code on Mac</small>

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/powershell-ise-lab.png">
<br><small>Powershell ISE on Windows</small>

<h2>Tips & Tricks</h2>
<h3>You can't run it all at once</h3>
<p>Note that with these labs you will not be able to just run the code all at once.  You will have to read the scripts, edit them and make changes and run them.  In this process hopefully you will learn a lot about Powershell and CLI and how the various solutions work. Part of the process is also learning how to read the error messages.  It's all part of getting comfortable with Azure and how this stuff works.

<h3>Edit, Select and Run</h3>
Select each line of the code, right click and run. Read the variables and values. 

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/right-click-ISE.png">
<br>Powershell ISE - Right Click Run Selection

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/run-line-in-editor.png">
<br>VS Code - Right Click Run Line in Terminal 

<h3>Don't forget the ` when using Powershell</h3>
<p>In the powershell example I used ` for to continue the script on the next line. So when you highlight and run a script, make sure you catch the wrapped lines. 
<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/powershell-ise-wrap.png">


<h3>The Virtual Machines need IIS installed for the lab to work</h3>
<p>For these labs we are deploying Windows VM's and in order to have them act like web servers you will need to install IIS.  Do do this you will RDP to the VM's and run a powershell script on each VM to install IIS and the necessary tools. Don't worry, it's all in the instructions/comments of the script. The powershell to run on the VM's is. The biggest mistake is that students forget to remove the # when trying to run it. 

<b>Install-WindowsFeature -name Web-Server -IncludeManagementTools </b>

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/run-install-web-server-tools.png">

<p>After running this, navigate to the following file on the VM and edit in Notepad.
C:\inetpub\wwwroot\iistart.html

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/edit-iis-html.png">

<p>Note that iistart.html is the default website homepage. We want to edit the HTML in this file so you can easily identify which VM is being hit in the tests. AGW, ALB and Traffic Manager will all mask the URL so editing the HTML makes it easy to know which machine is which. The result will look like this.

<img src="https://raw.githubusercontent.com/dkj0/rampup01-clips/master/blogimages/iisstartpage-mac.png">


<h3>Watch What Happens</h3>
<p>Use portal.azure.com to see what you are building and how things show up as you are working through your labs.  Also you can use resources.azure.com to see the same. 

<h3>Don't be afraid to break stuff</h3>
If you get stuck, do some research and figure it out. Don't be afraid to break anything. You can always delete your resource group and start over.  The best way to ramp up on this stuff, is to just start doing it and learn as you go. 

<h3>Delete and Do It Again</h3>
When you are all done, I recommend deleting your resource groups and doing it all over again. Change the setup and try again.

<h3>Always Create A New Resource Group</h3>
When trying new stuff, ALWAYS create a new Resource Group so you can easily delete, and or move it later if you want to keep it.

<h3>Where to from here?</h3>
<p>After getting comfortable with powershell and CLI, I like to work on the following with my class

<ul>
 <li>Try ARM template deployments with CLI and Powershell
<li>Try Creating a Web App using PaaS and CosmosDB  
<li>Use Powershell and Update the App Gateway above to point to your Web App 
<li>Best practices for ramping up on ARM
<li>Use the portal to select and configure your solution
<li>Then export the ARM template and play with it
<li>Or try finding something on github and play with it and deploy
<li>Try it first on the portal to create, poke around and maybe try a "fast start"
</ul>

<h3>Have fun!</h3>




