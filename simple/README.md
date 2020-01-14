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
* It will take some time for the WebLogic configuration to properly deploy (could be up to an hour). Once the deployment completes, in the portal go to 'All resources'. Enter `<your suffix>` into the filter box and press enter.
* Find and click on adminServerVM. Copy the DNS name for the virtual machine. You should be able to log onto http://`<admin server DNS name>`:7001/console successfully using the credentials above.  If you are not able to log in, you must troubleshoot and resolve the reason why before continuing.

Once you are done exploring this aspect of the demo, you should delete the weblogic-cafe-group-`<your suffix>` resource group. You can do this by going to the portal, going to resource groups, finding and clicking on weblogic-cafe-group-`<your suffix>` and hitting delete. This is especially important if you are not using a free subscription! If you do keep these resources around (for example to begin your own prototype), you should in the least use your own passwords and make the corresponding changes in the demo code.

## Configure Managed PostgreSQL

We will be using the same fully managed PostgreSQL we configured in the [local demo](../javaee/README.md).  Please make sure to obtain the configuration strings from when you executed that part of the demo.

* Log in to the WebLogic console as shown in the preceding step
* Click on 'Lock and Edit'. 
* Follow the same steps as in the local setup for [Connect WebLogic to the PostgreSQL Server](../javaee/README.md#connect-weblogic-to-the-postgresql-server).  Return to these steps after you have successfully tested the datasource.

## Running the Application
The next step is to get the application up and running. These steps assume you have already connected Eclipse to the locally running WebLogic Server.

* Start Eclipse.
* Go to the 'Servers' panel, secondary click. Select New -> Server -> Oracle -> Oracle WebLogic Server. 
* For "Server's host name" use the hostname `<admin server DNS name>`.
* Click next.
* Choose remote, for the remote host enter `<admin server DNS name>`
* Enter the WebLogic admin username/password from above.
* Click "Test connection".  If "Test connection succeeded!" appears, click Ok and you may continue.  Otherwise, troubleshoot and resolve the reason for the connection failure.
* 'Finish'.
* You have already built opened the application and built it locally in the IDE when you executed the [local demo](../javaee/README.md).

### Deploying the Application
Ensure that the deployment action from Eclipse will target the WebLogic Server running on Azure.
* Secondary click on the weblogic-cafe in the Project Explorer and choose Properties.
* Click on Server in the left navigation pane.
* You should see your local and remote WebLogic servers.  Select the remote one and choose Apply and Close.
* Secondary click on weblogic-cafe in the Prjoect Explorer and choose Run As -> Run on Server.
* Once the application runs, Eclise will open it up in a browser. The application is available at http://`<admin server DNS name>`:7001/weblogic-cafe.

## Content

The application is composed of:

- **A RESTFul service*:** protocol://hostname:port/weblogic-cafe/rest/coffees

	- **_GET by Id_**: protocol://hostname:port/weblogic-cafe/rest/coffees/{id} 
	- **_GET all_**: protocol://hostname:port/weblogic-cafe/rest/coffees
	- **_POST_** to add a new element at: protocol://hostname:port/weblogic-cafe/rest/coffees
	- **_DELETE_** to delete an element at: protocol://hostname:port/weblogic-cafe/rest/coffees/{id}
    
    
Some sample interactions:

```
curl --verbose http://wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001/weblogic-cafe/rest/coffees
*   Trying 40.121.58.67...
* TCP_NODELAY set
* Connected to wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com (40.121.58.67) port 7001 (#0)
> GET /weblogic-cafe/rest/coffees HTTP/1.1
> Host: wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Sat, 11 Jan 2020 01:10:00 GMT
< Content-Length: 335
< Content-Type: application/xml
< X-ORACLE-DMS-ECID: 4f2101e9-b8c9-4e76-a6eb-22bd86e0edfc-00000022
< X-ORACLE-DMS-RID: 0
<
* Connection #0 to host wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com left intact
<?xml version="1.0" encoding="UTF-8" standalone="yes"?><coffees><coffee><id>1</id><name>Strong</name><price>5.0</price></coffee><coffee><id>2</id><name>Weak</name><price>0.25</price></coffee><coffee><id>200</id><name>Medium</name><price>5.0</price></coffee><coffee><id>202</id><name>Medium 2</name><price>5.0</price></coffee></coffees>* Closing connection 0

curl --verbose http://wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001/weblogic-cafe/rest/coffees/1
*   Trying 40.121.58.67...
* TCP_NODELAY set
* Connected to wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com (40.121.58.67) port 7001 (#0)
> GET /weblogic-cafe/rest/coffees/1 HTTP/1.1
> Host: wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Sat, 11 Jan 2020 01:10:44 GMT
< Content-Length: 102
< Content-Type: application/xml
< X-ORACLE-DMS-ECID: 4f2101e9-b8c9-4e76-a6eb-22bd86e0edfc-00000023
< X-ORACLE-DMS-RID: 0
<
* Connection #0 to host wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com left intact
<?xml version="1.0" encoding="UTF-8"?><coffee><id>1</id><name>Strong</name><price>5.0</price></coffee>* Closing connection 0

cat medium.xml
<?xml version="1.0" encoding="UTF-8"?>
<coffee>
  <id>207</id>
  <name>Medium 7</name>
  <price>7.0</price>
</coffee>

curl --verbose -X POST -d @medium.xml http://wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001/weblogic-cafe/rest/coffees --header "Content-Type: application/xml"
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* Connected to wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com (::1) port 7001 (#0)
> POST /weblogic-cafe/rest/coffees HTTP/1.1
> Host: wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001
> User-Agent: curl/7.64.1
> Accept: */*
> Content-Type: application/xml
> Content-Length: 112
>
* upload completely sent off: 112 out of 112 bytes
< HTTP/1.1 201 Created
< Date: Mon, 13 Jan 2020 19:59:14 GMT
< Location: http://wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com:7001/weblogic-cafe/rest/coffees/207
< Content-Length: 0
< X-ORACLE-DMS-ECID: 6b7bc556-c060-46b0-bdd6-f3b5c95614bd-00000041
< X-ORACLE-DMS-RID: 0
<
* Connection #0 to host wls-3eb5405ea2-admindomain.eastus.cloudapp.azure.com left intact
* Closing connection 0
```
    

- **A JSF Client:** protocol://hostname:port/weblogic-cafe/index.xhtml

Feel free to take a minute to explore the application.
