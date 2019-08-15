
# Ping App ARM Template 
This repo contains the ARM Template for all the resources required for the PingConsole or .NET Core App. It creates an Event Hub, Log Analytics Workspace, Logic App which pulls all the ping data from event hub into a log analytics workspace. This demo shows that you can use the Azure Deploy Button without a custom Azure Resource Manager template (azuredeploy.json).


<a href="https://azuredeploy.net/" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

# Deploy Instructions
<b>Step 1:</b> Click on the <b>'Deploy to Azure'</b> button </br>

<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/0.png">
<b>Step 2:</b> Fill template form as shown below, click <b>'Next' </b>button </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/1.png">
<b>Step 3:</b> Click <b>'Next' </b> after looking at validation passed </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/2.png">
<b>Step 4:</b> Wait for Deployment to go through when you see the form below; your resources are ready in Azure Portal </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/4.png">
<b>Step 5:</b> Login to Azure portal to view the newly created resources </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/5.png">
<b>Step 6:</b> For now the Log Analytics connector is in (preview) mode therefore you will need to fill out the connection info manually. </br> Click on advanced settings for the log analytics workspace and copy the Workspace ID and Primary key 
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/5.5.png">
Copy the Workspace ID and Primary key in a seperate file or notepad as you will need it for the next step
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/6.png">
<b>Step 7:</b> Open up the API connection for Log Analytics workspace as shown below and paste the information copied in previous step </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/7.png">
<b>Step 8:</b> Click on Edit API connection 
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/6.5.png">
<b>Step 9:</b> Fill out the Workspace ID and Primary key as shown below and click on <b>Save</b> button </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/7.5.png">
<b>Your system is ready to be used! </b>
