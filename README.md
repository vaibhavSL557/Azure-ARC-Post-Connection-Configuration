# Azure-ARC-Post-Connection-Configuration
## Create a VM
Follow the step-by-step instructions in  [How to Build a server for Azure Arc Server](https://microsoft.sharepoint.com/:w:/r/sites/Infopedia_G01KC/_layouts/15/WopiFrame.aspx?sourcedoc=%7B59d50707-cf0e-4810-8417-b809a9ffdcc6%7D&action=default&DefaultItemOpen=1&cid=4b8f3def-17b8-498a-9eea-109ceec370e2)  to get a Windows
Machine installed on your laptop to connect with Azure ARC in your subscription. There are other ways
you can connect Windows or Linux servers to Azure ARC, but the referenced lab guide will walk you
through how to build this for demonstration purposes. 

# Manage Access to the Arc Server
In this exercise, you will grant a Role Based Access. Feel free to use any account that you have in the
portal. But if you want to try some more restricted roles, then you may want to make a test user.
Otherwise you any accounts you have access to in Azure Active Directory.

- Go to the [Azure ARC resource blade](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.HybridCompute%2Fmachines).  

   - Type **ARC** at the type of your Azure Portal in the Search field
   
      -TIP **G+/** will move you there from an open portal!
   - Select **Azure ARC** and click on the **Manage Servers** button
   
- Click on the hyperlink for one of your servers and then click on**Access Control(IAM)**

- Click on **Roles** across the top menu
 
 ![alt text](pic1)
 
 - Notice all of the possible roles that exist just at this level.  There are two new ones created just for Azure ARC.
 
 - In the field that says “Search by role name”, type **Connected**
 
 ![alt text]()
 
 - The first role above only lets you onboard Azure Connected Machines.  The Administrator is much more powerful and allows read, write, delete and re-onbaord Azure Connected machines	
 
 - Click the **Add** button at the top of the window and select **Add Role Assignment**
 
 - In the **Select a Role** drop-down list, select “Azure Connected Machines Resource Administrator
 
 - In the **Select** field type the name of an account you want to add or select one. Click **Save** at the bottom
 
 - Click on **Role** Assignments at the top
 
 
 ![alt text]()
 
 You will now see the account you added as well as the assigned role.  You could now use this account to onboard machines, but not have any other Azure rights. This is a great way to delegate a single task to an administrator that will be responsible for onboarding all machines into Azure ARC.
 

## Tag your ARC server

- Open the Azure portal page. [Click on this link](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.HybridCompute%2Fmachines) to go to the Azure ARC machine(s) you have built

- Click on your machine that you want to tag

![alt text]()

- After clicking on it, Click on **Tags** in the center blade

- Create the following tags and then Save them

| Name | Value |
| --- | --- |
| ARC server type | windows |
| cost center | IT |
| Department | Central IT |
| owner | <your name> |
  
  - After saving them they should look similar to this…
  
   ![alt text]()
   
  
  ## Create a Do Not Delete Lock
  
 - In the center “Machine – Azure Arc” menu, click on **Locks**
 
 - Click on **+ Add**
  
 - In the **Lock Name** field type “Do Not Delete This ARC Server”
 
 - Select **Delete** from the **Lock Type** drop down list and then click **OK**
 
 - The result should look like the picture below, except with your server name
 
   ![alt text]()
   
   - You will get the expected behavior and error that your Hybrid Machine “…could not be deleted, thankfully because you protected it with an Azure Resource Manager Lock
   
    ![alt text]()
    
   
   
  ## Configure Log Analytics and Diagnostic Settings
  
   There are many [diagnostic settings](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-settings) that can be configured in Azure and in Azure Arc. For Azure Arc in particular you can select from the following diagnostic settings:
      
      
   - Administrative
      
   - Security
      
   - ServiceHealth
      
   - Alert
      
   - Recommendation
      
   - Policy
      
   - Autoscale
      
   - ResourceHealth
   
   
   
   
   
   But before you select any of these, there needs to be a place to           ![alt text]()
   
   output these to.  You can select from the following:
   
   
   
   
   
   For this exercise, we will send to Log Analytics by creating a Log Analytics workspace.
   
   - Select your ARC server in the portal and then click on **Activity log**
   
   - Click **Logs** from the top of that window                  ![alt text]()
   
   - Click the **Add** button
   
   - In the right window, click on **Select a Workspace** and then click the **Create a New Workspace** button.
   
   - Type “ARC-Workspace” in the **Log Analytics Workspace** field
   
   - Select the Free pricing tier and Click **OK**.  
   
      - NOTE: you can change this after or select a larger option for more retention capabilities. [Read more](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#pricing-model) on the pricing options.
      
   - Click **OK** a second time once your workspace is completed
  
   - Once you get an alert at the top of the portal that your Deployment succeed, click the **Refresh** button. Return to the **Activity Log** section where you started above.  Use the breadcrumbs at the top to get you there quickly
  
  - Click on **Diagnostic Settings** at the top of the window                                 ![alt text]()
 
  - Now in the middle of the page click on **Add Diagnostic Settings**
 
 - In the **Diagnostic Settings Name** type “Send Arc Logs to Log Analytics Workspace”
 
 - Select All of the logs and the click on **Send to Log Analytics**
 
    - If you had only one workspace, the one you created will be shown.  Otherwise, be sure to select the workspace you just created.
    
-  Finally, click the **Save** button as shown below





                        
                        
                        
                        
                                       
                                       
                                       
                                       
                                       
                                       
                                       
      ![alt text]()
                                        
                                        
                                        
                                        
                                        
                                        
                                        
 
 - Now select your ARC server in the portal and click on **Access Control (IAM)**
 
 - Click the **Add** button to the right in the “Add a Role Assignment” box
 
    - For Role select **Owner**
    
    - Search for and select your name or someone else to test and then click **Save**
    
- Return back to the **Activity Log** window

    - Refresh until you see **Create a Role Assignment**
    
     - NOTE: this may take up to 5 minutes
     
- Create a New tag for this Azure ARC server.  Then return to **Activity Log** a little later and check for the change


 ## Verify Configuration Policy Resource Provider enabled
 
 If Azure is new and you haven’t configure much, this resource provider may not be registered.  But let’s see where it is and how to register it.
 
To register the resource provider for Guest Configuration through the Azure portal, follow these steps



   1.From the Azure portal and click on **All services** on the left. Search for and select **Subscriptions**.

   2.Find and click on the subscription that you want to enable Guest Configuration for.

   3.In the center blade of the **Subscription** page, click **Resource providers**.
   
   4.Type Guest in the “Filter by name” field then click **Register** on the **Microsoft.GuestConfiguration**, if it is not already 
        registered. A green check mark as shown below means you are all set.
        
        
        
                                                      
                                                      
   ![alt text]()
            
            
   ## Review and Assign Azure Policies
   
   - From the **Machine -Azure Arc** menu for the Hybrid Server you are working with, click on **Policies**
   
   - Click **Assign Initiative **                                                                        ![alt text]()
   
   - To the right of Basics click on the ellipses (…) to the right of **Initiative definition**
   
   - In the **Search** window for available definitions, type “Time ” and select the one called **Show Audit results from Windows VMs **
   
   **that are not set to the specified time zone**.  Click the blue Select button below
   
     
   
   - Click **Next** at the bottom of the window
   
   - From the **Time zone** drop down menu select “(UTC – 05:00) Indiana (East)”. Click **Next**
   
   - Read the description and then select the checkbox for Create a **remediation task**. This ensures that the policy will apply to 
   
     existing resources after the policy is assigned.  If that box is not selected, then the policy only applies to newly created  
   
     resources.
   
   - Select the **Create a Managed Identity** check box and the click **Next** again
   
   - Then at the bottom of the **Assign Policy**window click on **Create**
   
   - Click on the new initiative just created **Audit Windows VMs that are not set to the specified time zone**
   
   - Click Create a **Remediation Task** at the top right 
   
     ![alt text]()
     
 - Confirm that the **Scope** is showing the correct Resource Group – should default to …/ARC  
      
      ![alt text]()
          
          
      - Click on the **ellipses** …to the right to select all options to include the server as shown to the right
      
      
      


 - Select the Checkbox beneath **Re-evaulate resource compliance before remediating**
 
 - In the **Locations** drop-down list, select the location where you installed your ARC Server
 
   - If you are not sure or can’t remember, look on the **Overview** window for your server
   
 - Click **Remediate** at the bottom of the **New Remediation Task**window


In the next window at the bottom you will see a blue circle beside Evaluating. When it is successful and completed, the circle will turn green and it will say Complete. NOTE: if you had many ARC servers, you could evaluate the all at once but changing the scope to select one of more locations or all within a subscription.

Optional initiatives to try… repeat the steps above to test some other policies such as:

## Operational compliance

 - Micro-segmentation to ensure servers are connected to the right network (remote host connection status doesn't match the specified one)
 
 - Certificate about to expire
 
 - Application installed: ensure backup is installed
 
 - Machines are not restarted after 45 days, indicator that it has been forgotten, running but not used
 
 ## Security compliance
 
 - Password policy
 
 - Application installed or not installed (like diagnostic tools used for investigation, but make sure it is removed after troubleshooting)
 
 - TLS 1.2 monitoring => part of PCI DSS (data security standard)
 
 ## Azure Monitor for Azure Arc Server
 
 - Go to **Monitor** in the Azure Portal and click on **Activity log**
 
 - The activities above for creating policies should show
 
 ![alt text]()
 
 - Click **Alerts** and then select **New Alert Rule**
 
 - Click the Select button beneath **Resource**
 
 - In the **Filter by resource type** field, type “ARC” and then select **Machines – Azure Arc** from the drop down 
 
 - Select your Azure ARC machine and then click **Done**
 
 - Beneath Condition click the **Add** box
 
 - From each of the drop-down list boxes
 
   - Select **Activity Log**, from Signal Type and **Activity Log – Administrative**, from Monitor Service
   
 - Select the **All Administrative operations** option from the Signals that appear as shown below 
 
  ![alt text]()
  
  - Click **Done**
  
 - Beneath Action Groups click on the **Create** button
 
 - Complete the fields as below
 
    - **Action Group Name** ARC Alerts
  
    - **Short Name** ARC Alerts
  
    - **Action Name** ARCSMS
  
 - Select from the **Action Type** drop down list **Email/SMS/Push/Voice**
 
   - Enter an option you would like to test.  You can select multiple options
   
 - Click **Yes** on the Common Alert Schema selection.  [Learn more](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-common-schema)
 
- Click **OK**

- Click the **Add** button and select **ARC Alerts** and click **Select**
- For the **Alert Rule Name** and the **Description Fields** type

   - Send an SMS message whenever an ARC Server Reconnects
   
   - Click **Create Alert Rule**

- Try some administrative Actions to test the alert such as
   
    - Delete the lock created
   
   - Delete the Role Assignment created
   
   - Create a new tag
   
   - Create another new role assignment 


 


                                        
                                         



      

  
  
 
 



 
 


 
 
