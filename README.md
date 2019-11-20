# Ping App ARM Template 
This repo contains the ARM Template for all the resources required for the PingConsole or .NET Core App. It creates an Event Hub, KeyVault, Storage Account, Log Analytics Workspace, and Logic App which pulls all the ping data from event hub into a log analytics workspace. This demo shows that you can use the Azure Deploy Button without a custom Azure Resource Manager template (azuredeploy.json).

# Deployment Steps
<b>Step 1:</b> Click on the <b>'Deploy to Azure'</b> button below </br>

<a href="https://azuredeploy.net/" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

<b>Step 2:</b> Fill template form as shown below, fill the KeyVault Name with only letters between 3-24 with no numbers or special characters, ObjectId is a GUID string obtained from Azure CLI in portal, in PowerShell mode of Azure CLI type 'Get-AzADUser -UserPrincipalName foo@domain.com' repalcing foo@domain.com with your own Azure Portal email and pasting the Object Id string in the form below as shown, click <b>'Next' </b>button </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/1%20new.png">
<b>Azure CLI</b> How to obtain <b>'ObjectId'</b> field value from Azure CLI</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/0.5.png">
<b>Step 3:</b> Click <b>'Next' </b> after looking at validation passed </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/2%20new.png">
<b>Step 4:</b> Wait for Deployment to go through when you see the form below; your resources are ready in Azure Portal </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/4%20new.png">
<b>Step 5:</b> Login to Azure portal to view the newly created resources </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/5new.png">
<b>Step 6:</b> For now the Log Analytics connector is in (preview) mode therefore you will need to fill out the connection info manually. </br> Click on advanced settings for the log analytics workspace and copy the Workspace ID and Primary key </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/5.5.png"  height="760" width="850">
Copy the Workspace ID and Primary key in a seperate file or notepad as you will need it for the next step</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/6%20new.png">
<b>Step 7:</b> Open up the API connection for Log Analytics workspace as shown below and paste the information copied in previous step </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/7new.png">
<b>Step 8:</b> Click on Edit API connection </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/6.5%20new.png"  height="660" width="700" ></br>
<b>Step 9:</b> Paste the Workspace ID and Primary key copied in the previous steps as shown below and click on <b>Save</b> button </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/7.5.png">
<b>Step 10:</b> Next You need to replace the secrets in KeyVault with EventHubConnection string and also copy the name of the eventhub instance. In addition make note of the event hub name instance as pointed below</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/8.png">
<b>Step 11:</b>  Click on the EventHub and copy it's Connection String to a seperate file, you will need this to be added as Key-Vault Secret value</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/8.5.png">
Navigate to and Copy the event hub name as shown below</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/9.png">
<b>Step 12:</b> Blob Storage Primary Key is the second keyvault secret that we will retrieve as shown below. Open the Storage Account annd copy it's access key value and container name as show below   </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/10.png">
<b>Step 13:</b> Navigate to the Access Keys for the Storage account and copy the Key Value shown below onto a seperate file </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/11.png">
<b>Step 14:</b> Then Navigate to the Containers tab in the same storage account and copy the name of the container onto a seperate file as shown below. This will be need in appsettings json file of the ping utility code </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/12.png">
<b>Step 15:</b> Click on the Key vault and Navigate it to it's Overview section to copy the DNS name for the keyvault. Make a note of the key vault's DNS name as shown below on to a seperate file. This will be needed in appsettings json file of the ping utility code and then to navigate to it's Secrets section to replace the values with the information copied in the above steps. Key vault will have all the information collected above to be stored in key vault in the form of secret's value as shown below.</br>

<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/13.png">
Save the key-vault's DNS name in a seperate file, it will be needed in appsettings json file of the ping utility code </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/14.png">
<b>Step 16:</b> Open the Key Vault tab called "Secrets" and click on the first secret value "blobStoragePrimaryKey" and click on "New Version" to store the new value copied in the previous steps.
When the form opens, paste the Blob Storage Primary Key in the value field as shown below </br>

<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/14.5.png" >
<b>Step 17:</b> When the Blob Storage Primary Key is pasted in the value field as shown below, click on the Create button</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/15.png" >
<b>Step 18:</b> Then repeat the same steps for replacing the Event Hub Connection string titled "eventhubconnString", click on the New Version button and paste the Event Hub Connection string saved from the previous step</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/16.png" >
<b>Step 19:</b> The following steps are related to the installation of the PingAsync Utility on your System.
You need to create a Linux VM or setup an existing Linux/Windows VM, on which your PingAsync utility Code will run. It requires you to open an Azure CLI PowerShell and run the following commands. The following is taken from a reference article here (https://docs.microsoft.com/en-us/azure/key-vault/tutorial-net-linux-virtual-machine) </br>
<b>
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
</b></br>
Once the Linux VM is setup the next steps will show you how to link the VM to the key-vault. In case you are using an existing VM, skip to the next step
</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/16.5.png" >
When the above runs, a new VM will be created in the resource group specified, copy the details of the newly created VM in a seperate file</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/18.png" >
<b>Step 20:</b> In this next step assign an identity to the VM as shown below by running the following command, after the above has successfully run in the Azure CLI, run the following in the same Azure CLI, when the following has successfully run, save the details in a seperate file to be used in the next step</br>
<b>
az vm identity assign --name NameOfYourVirtualMachine --resource-group YourResourceGroupName</br>

</b>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/19.png" >
<b>Step 21:</b> In this last step related to key-vault and VM pairing, give your Key Vault permission to the Virtual Machine identity you created in the previous step by running the following command as shown below in the same Azure CLI window, replace the KeyVault name and the object Id with "systemAssignedIdentity" value obtained from the previous steps</br>
<b>
az keyvault set-policy --name 'YourKeyVaultName' --object-id VMSystemAssignedIdentity --secret-permissions get list
</b>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/19.5.png" >
<b>Step 22:</b> Now that you have the VM setup in Azure along with the necessary resources to run PingUtility on an Azure based VM, let's download the PingAsync Utility application through the following steps. Open a <b> Windows Powershell in Administrator mode </b> as shown below (or you can carry the same steps in the <b>Azure CLI</b> session used above - it will make no difference!), then run the following command in Windows Powershell: </br><b> ssh azureuser@PublicIpAddress </b> </br>
The following shows how to access your Azure subscriptions in a Windown Powershell command prompt, run the <b>az login </b> command and select your subscription to access the VM - (in case you have multiple subscriptions):</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/20.5.png" >
To access the VM, if it is a new VM created through the process in the previous steps, you might need to reset the password for the '<b>azureuser</b>' login name from the Azure poral. </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/22.png" >
Once you have reset the password, run the <b>ssh azureuser@PublicIPAddress</b> command as shown below to login in to your VM </br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/21.png" >

<b>Step 23:</b> The following walks you through the steps to prep the Azure based VM that was accessed above so the the PingUtilty can be run on the VM. Install the following components on the VM by running the following commands in the same Windows Powershell session that you used to ssh or login to VM as shown below:
<b></br>
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg</br>
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg</br>

$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb</br>
$ sudo dpkg -i packages-microsoft-prod.deb</br>
$ sudo apt-get install apt-transport-https</br>
$ sudo apt-get update</br>
$ sudo apt-get install dotnet-sdk-3.0</br>
$ dotnet --version</br>
</b>
Your results of running the above should look like the following
</br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/23.png" >
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/23.5.png" >
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/24.png" >

<b>Step 24:</b> Finally we can install the Ping Async Tool by running the following commands</br>
<b>
$ wget -qO - https://raw.githubusercontent.com/komalsyed-Azure377/pingasynctool/master/PUBLIC.KEY | sudo apt-key add -</br>
$ echo "deb https://raw.githubusercontent.com/komalsyed-Azure377/pingasynctool/master bionic main" | sudo tee /etc/apt/sources.list.d/pingtool.list</br>
$ sudo apt-get update</br>
$ sudo apt-get install pingasync</br>
$ export PING_HOME=/opt/ksyed/pingtool/PingAsync.dll</br>
</b>
<b> At this stage the Ping Utilty is installed in your system ready to be used! </b></br>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/26.png" >

<b>Step 25:</b> Changing the configuration for the PingUtilty is shown below - we will require all the information copied seperately in the previous steps. In order to change the configuration for the Ping Utility being run navigate to the <b>appsettings.json</b> file by writing the command '<b>cd /opt/ksyed/pingtool</b>' in the same session above and then type the command  <b>sudo vim appsettings.json</b> will open the configuration file for you to change various parameters as shown below</br>

The configuration has 5 elements which need to be changed according to resources in your enviornment and VM setup.</br>
1. The first element required is the <b>Key-Vault DNS name</b> saved in a seperate file in Step 15 above</br>
2. The second element required is the <b>EventHub Name</b> saved in a seperate file in Step 11 above</br>
3. The third element is the name of your file, which you have uploaded to blob storage or even if it is on your local VM, the <b>filename</b> should be complete with extension i.e filename.csv or filename.xlsx.</br>
4. The fourth element required is the <b>Storage Account Name</b> saved in a seperate file in Step 12 above.</br>
5. The fifth element required is the <b> Storage Container Name</b> saved in a seperate file in Step 14 above.</br>
6. Line 6 just explains that depending on the file extension this SELECTED_FILE_TYPE should have an integer value set to 1 or 2, if it is an excel file or csv file respectively.</br>
7. line 7 explains the purpose of the SELECTED_IP_COLUMN which is a string in the format _IP_COLUMNX where X is the column for the ip address value in the uploaded file - you can skip this explanation if your IP address is the first column for a csv file, if it is excel file then this should be set to _IP_COLUMN1 for e.g.</br>
8. line 8 again just explains that if the file has been uploaded to blob to set the FILE_LOCATION BLOB value to true, otherwise set it to true for a file placed in LOCAL_FILE_PATH on the VM.</br>

Navigate out of this file after having made the changes above and saving your file by pressing Esc and typing ":x" in the console, this will save your data and close the file, taking you back to the command line
</b>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/27.png" >
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/28.png" >
</br>

<b>Step 26:</b> Uploading a sample csv file to blob storage account in the Container pointed to in Step 14, the following walks you through to upload a csv from your local system to blob
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/29.png" >
</br>

<b>Step 27:</b> Your system is ready to be used type the following in the same Windows Powershell or AzureCLI that you have been using above and type the following </br>
<b>
$ sudo dotnet $PING_HOME</br>
</b>
<img src="https://komalsandboxdiag.blob.core.windows.net/pingarmtemplatereadmefiles/30.png" >
<b>Your system has the Ping Utilty running successfully!</b></br>





