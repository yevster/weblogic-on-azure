# Basic Java EE CRUD Application
This is the basic Java EE 7 application used throughout the WebLogic on Azure demos. It is a simple CRUD application. It uses Maven and Java EE 7 (JAX-RS, EJB, CDI, JPA, JSF, Bean Validation).

We use Eclipse but you can use any Maven capable IDE such as NetBeans. We use Postgres but you can use any relational database such as MySQL or SQL Server.

## Setup

* Install JDK 8 (we used [AdoptOpenJDK OpenJDK 8 LTS/HotSpot](https://adoptopenjdk.net)).
* Install the Eclipse IDE for Java EE Developers from [here](https://www.eclipse.org/downloads/packages/). 
* Download this repository somewhere in your file system (easiest way might be to download as a zip and extract).
* You will need an Azure subscription. If you don't have one, you can get one for free for one year [here](https://azure.microsoft.com/en-us/free).

## Start Managed PostgreSQL on Azure
We will be using the fully managed PostgreSQL offering in Azure for this demo. Below is how we set it up. 

* Go to the [Azure portal](http://portal.azure.com).
* Select Create a resource -> Databases -> Azure Database for PostgreSQL. Select a single server.
* Specify the Server name to be weblogic-cafe-db-`<your suffix>` (the suffix could be your first name such as "reza"). Create a new resource group named weblogic-cafe-group-`<your suffix>` (the suffix could be your first name such as "reza"). Specify the login name to be postgres. Specify the password to be Secret123!. Hit 'Create'. It will take a moment for the database to deploy and be ready for use.
* In the portal, go to 'All resources'. Find and click on weblogic-cafe-db-`<your suffix>`. Open the connection security panel. Enable access to Azure services and disable SSL connection enforcement. Hit Add client IP. Hit Save.

Once you are done exploring the demo, you should delete the weblogic-cafe-group-`<your suffix>` resource group. You can do this by going to the portal, going to resource groups, finding and clicking on weblogic-cafe-group-`<your suffix>` and hitting delete. This is especially important if you are not using a free subscription! If you do keep these resources around (for example to begin your own prototype), you should in the least use your own passwords and make the corresponding changes in the demo code.

## Running the Application
The next step is to get the application up and running. Follow the steps below to do so.
* Start Eclipse.
* Go to the 'Servers' panel, right click. Select New -> Server -> Red Hat JBoss Middleware -> JBoss AS, WildFly and EAP Server Tools. Click next. Accept the license agreement, click 'Finish'.
* After the Eclipse WildFly adapters are done installing, go to the 'Servers' panel again, right click. Select New -> Server -> JBoss Community and select the latest WildFly version. Choose the defaults on the next screen and hit 'Next'. Select 'Download and install runtime..." and select the latest stable version of WildFly. Hit next, accept the license agreement. Hit next again. Select where you want WildFly downloaded and installed. Click 'Finish'. Keep the defaults and hit 'Finish'. WildFly is now setup in Eclipse.
* Go to the directory where you have WildFly installed.
* Create a new directory structure org/postgresql/main within /modules.
* Copy the Postgres driver and the corresponding module.xml to the newly created directory structure under main. These files are located in the javaee/server directory where you downloaded the application code.
* From the same javaee/server directory, copy the standalone.xml file to the /standalone/configuration directory of the WildFly installation.
* Get the javaee-cafe application into the IDE. In order to do that, go to File -> Import -> Maven -> Existing Maven Projects. Then browse to where you have this repository code in your file system and select javaee/javaee-cafe. Accept the rest of the defaults and finish.
* Once the application loads, you should do a full Maven build by going to Right click the application -> Run As -> Maven install.
* It is now time to run the application. Go to Right click the application -> Run As -> Run on Server. Make sure to choose WildFly as the server going forward. Just accept the defaults and wait for the application to finish running.
* Once the application runs, Eclise will open it up in a browser. The application is available at [http://localhost:8080/javaee-cafe](http://localhost:8080/javaee-cafe).

## Content

The application is composed of:

- **A RESTFul service*:** protocol://hostname:port/javaee-cafe/rest/coffees

	- **_GET by Id_**: protocol://hostname:port/javaee-cafe/rest/coffees/{id} 
	- **_GET all_**: protocol://hostname:port/javaee-cafe/rest/coffees
	- **_POST_** to add a new element at: protocol://hostname:port/javaee-cafe/rest/coffees
	- **_DELETE_** to delete an element at: protocol://hostname:port/javaee-cafe/rest/coffees/{id}

- **A JSF Client:** protocol://hostname:port/javaee-cafe/index.xhtml

Feel free to take a minute to explore the application.
