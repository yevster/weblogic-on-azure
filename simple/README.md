# Deploying a Java EE Application on Azure Using a Single WebLogic Virtual Machine Instance
This demo shows how you can deploy a Java EE application to Azure using a simple instance of WebLogic Server deployed from the Azure Marketplace.

## Setup

* This demo assumes you have successfully executed the steps in the [local portion of the demo](../javaee/README.md).

## Create the WebLogic Instance on Azure
The next step is to get WebLogic up and running on a virtual machine. Follow the steps below to do so.
* Go to the [Azure portal](http://portal.azure.com).
* Click 'Create a resource'. In the search box, enter 'weblogic' and press enter. 
* Select 'Oracle WebLogic Server 12.2.1.3 With Admin Server'. Hit 'Create'.
* In the the basics blade, just accept the defaults. 
* The steps in this section use `<your suffix>`.  The suffix must be different from what you used in [the local portion of the demo](../javaee/README.md)
* Create and specify a new resource group named weblogic-cafe-group-`<your suffix>` . Hit OK. 
* Choose the default for the virtual machine size and hit OK. 
* In the "Credentials for Server Creation" use these values
   * For the "admin account of VMs", enter 'Secret123456'. 
   * Enter your OTN/Oracle.com username and password (you can create an account for free). 
   * For the "Password for WebLogic Administrator", enter 'Secret123456'. 
   * Click OK. 
   * On the Summary blade you must see "Validation passed".  If you don't see this, you must troubleshoot and resolve the reason.  After you have done so, you can continue.
   * On the Summary blade, click OK. On the final screen, click Create.
* It will take some time for the WebLogic configuration to properly deploy (could be up to an hour). Once the deployment completes, in the portal go to 'All resources'. Find and click on adminServerVM. Copy the DNS name for the virtual machine. You should be able to log onto http://`<admin server DNS name>`:7001/console successfully.

Once you are done exploring the demo, you should delete the weblogic-cafe-group-`<your suffix>` resource group. You can do this by going to the portal, going to resource groups, finding and clicking on weblogic-cafe-group-`<your suffix>` and hitting delete. This is especially important if you are not using a free subscription! If you do keep these resources around (for example to begin your own prototype), you should in the least use your own passwords and make the corresponding changes in the demo code.

## Configure Managed PostgreSQL

We will be using the same fully managed PostgreSQL we configured in the [local demo](../javaee/README.md).  Please make sure to obtain the configuration strings from when you executed that part of the demo.

* In the portal, go to 'All resources'. Find and click on adminServerVM. Copy the DNS name for the virtual machine. Log onto http://`<admin server DNS name>`:7001/console using "weblogic" and "Secret123456".
* Click on 'Lock and Edit'. Click on Services -> Data Sources. Select New -> Generic Data Source. Enter the name as 'WebLogicCafeDB', JNDI name as 'jdbc/WebLogicCafeDB' and select the database type to be PostgreSQL. Click next. Accept the defaults and click next. On the next screen select 'Logging Last Resource' and click next. Enter the database name to be 'postgres'. Enter the host name to be 'weblogic-cafe-db-`<your suffix>`.postgres.database.azure.com' (the suffix could be your first name such as "reza"). Enter the user name to be 'postgres@weblogic-cafe-db-`<your suffix>`' (the suffix could be your first name such as "reza"). Enter the password to be 'Secret123!'. Click next. On the next screen, accept the defaults and click next. Select 'admin' and click Finish.
* Once the data source is configured, click 'Activate Changes'.

## Running the Application
The next step is to get the application up and running. Follow the steps below to do so.
* Start Eclipse.
* Go to the 'Servers' panel, right click. Select New -> Server -> Oracle -> Oracle WebLogic Tools. Click next. Accept the license agreement, click 'Finish'.
* After the Eclipse WebLogic adapters are done installing, go to the 'Servers' panel again, right click. Select New -> Server -> Oracle -> Oracle WebLogic Server. Choose the defaults and hit 'Next'. Enter where you have WebLogic and Oracle JDK installed, click next. Choose remote, for the remote host enter the admin server DNS name on Azure, enter the WebLogic admin username/password and hit 'Finish'.
* Get the weblogic-cafe application into the IDE. In order to do that, go to File -> Import -> Maven -> Existing Maven Projects. Then browse to where you have this repository code in your file system and select javaee/weblogic-cafe. Accept the rest of the defaults and finish.
* Once the application loads, you should do a full Maven build by going to Right click the application -> Run As -> Maven install.
* It is now time to run the application. Right click the application -> Run As -> Run on Server. Make sure to choose remote WebLogic as the server going forward. Just accept the defaults and wait for the application to finish running.
* Once the application runs, Eclise will open it up in a browser. The application is available at http://`<admin server DNS name>`:7001/weblogic-cafe.

## Content

The application is composed of:

- **A RESTFul service*:** protocol://hostname:port/weblogic-cafe/rest/coffees

	- **_GET by Id_**: protocol://hostname:port/weblogic-cafe/rest/coffees/{id} 
	- **_GET all_**: protocol://hostname:port/weblogic-cafe/rest/coffees
	- **_POST_** to add a new element at: protocol://hostname:port/weblogic-cafe/rest/coffees
	- **_DELETE_** to delete an element at: protocol://hostname:port/weblogic-cafe/rest/coffees/{id}

- **A JSF Client:** protocol://hostname:port/weblogic-cafe/index.xhtml

Feel free to take a minute to explore the application.
