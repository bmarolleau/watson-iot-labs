 {#section .ListParagraph}

{ Lab }

**IoT application in IBM Cloud**

**with Node-RED and IBM Watson**

![/var/folders/71/fyvbq3v924l147w4g99kqt200000gn/T/com.microsoft.Word/WebArchiveCopyPasteTempFiles/leaders\_IoT.jpg]

June 2019 -- v1.1

Authors: B. Marolleau, C. Bacle, C. Lalevée, Y. Holvoet

June 2019 -- v1.1

© Copyright IBM Corp. 2018

Materials may not be reproduced in whole or in part without the prior written permission of IBM

**Agenda**

[Before Starting 4]

[Hands-on presentation 5]

[Section 1. Overview 5]

[Section 2. Prerequisites 5]

[Cloud: Create Node-RED application and Login 6]

[1. Cloud: Create a virtual sensor and a new flow 10]

[Section 1. Sensors & IoT 10]

[Section 2. Node-RED flow: creation & import 11]

[2. Node-RED on PI: Your first MQTT Simulator 14]

[3. Cloud & Raspberry PI: Visual Recognition 16]

[4. Node-RED on PI: Grovepi Hat & Sensors (Optional \#1) 17]

[Section 1. GrovePi Hat Setup 17]

[Section 2. First flow with Grovepi 18]

[5. Insert IoT Data in Cloudant DB (Optional \#2) 21]

[6. Process IoT Data with Watson Language Translator (Optional \#3) 25]

[7. Create a dashboard application in Node-RED (optional \#4) 29]

[Section 1. Import Node-RED Dashboarding capability 29]

[Section 2. Create a simple Node-RED Dashboard 32]

[Section 3. Add voice alert on dashboard 35]

[8. Appendices: Architecture & Schema on IBM Cloud 39]

![][1]Before Starting
---------------------

> This hands-on required to have an IBM Cloud account. If you don't, you can create one here: <http://bluemix.net/>.\
> A complete guideline can also be found on the "IBM Cloud & Watson Day" web page here: <http://ibmcloud-watson-day.mybluemix.net/>

-   Open a browser and access to IBM Cloud: <https://console.bluemix.net>.

-   If you have an IBM Cloud account, click **Log in**, and enter your IBM ID credentials.\
    If you don't have an IBM Cloud account, click **Create a free account**. Enter your email address, and additional information required. You will receive an email with activation link. Once activated, you could use your new free IBM cloud account: log in.

![][2]

-   Select organization, location and space to use during this lab.

![][3]

-   If needed, free resources (GB / \#Services) in your IBM Cloud Organization & Spaces to run the lab exercises.\
    If you encounter a resource contention (error message saying you are out of resources), clean up your spaces by deleting existing Apps or Services.

Hands-on presentation  {#hands-on-presentation .ListParagraph .HeadingPartorSection}
----------------------

Overview  {#overview .HeadingSubsection}
---------

In this hands-on session, you will create a Node-RED application in IBM Cloud to collect, store and display virtual sensor data.

Node-RED (<https://nodered.org/>) is a flow-based programming tool, originally developed by the IBM Emerging Technology Services team (in early 2013) and now a part of JS Foundation.

Traditional development can be very technical, but Node-RED enables you to concentrate on the logic of your workflow and allows fast prototyping.

Node-RED consists of a Node.js-based runtime with a flow editor accessed through a web browser. Within the browser, you create your application by dragging nodes from a customizable palette into a workspace and start to wire them together. With a single click, the application is deployed back to its runtime.

Session objectives are:

-   Create & modify an application using Node-RED in IBM Cloud and on a device. In that example, we will use a Raspberry PI 2 B+

-   Discover new services & Node-RED to consume or create services (IoT / AI /Database)

-   Discover IBM Watson AI and IoT services

-   Discover how to integrate Cloud Services with a Raspberry PI or any device using Node-RED and create prototype quickly.

Prerequisites  {#prerequisites .HeadingSubsection}
--------------

There are 2 kinds of exercises in the present Lab: in the Cloud (IBM Cloud) and local (Node-RED on a Raspberry PI).

1)  The necessary hardware (provided);

-   Raspberry PI 2 with an internet access and Node-RED installed.

-   A PC connected to the Raspberry PI network (provided)

2)  Setup & Connectivity check:

-   Power on the Raspberry - USB power supply

-   From your PC - go to <http://raspberry314-pot.eu-de.mybluemix.net/home> . 

    -   click on your team picture and get the IP of your Raspberry on the local network.

> You will use this address to access your Raspberry from a Browser.

-   From your PC, connect to Node-RED using http://YOUR-RASPI-IP:1880 from a Browser (Firefox, ...)

-   IBM Cloud Sign in / Log on as mentioned in "Before Starting" section above.

Cloud: Create Node-RED application and Login {#cloud-create-node-red-application-and-login .ListParagraph .HeadingPartorSection}
--------------------------------------------

Expected results are:

-   Your Node-RED application is operational (using Node.js runtime), accessing Cloudant & IoT platform (QuickStart)

![][4]

-   Your Node-RED app is online (reachable from the Internet), & will be connected to a temperature simulator (sensor)

![][5]

1.  In IBM Cloud Catalog, choose **Boilerplate** category

![][6]

2.  Click on **Internet of Things Platform Starter** to create an instance:\
    Fill in the App Name & Host name fields.\
    Note: Node-RED is a Node.js based application: using this boilerplate will instantiate a Node.js runtime + a Cloudant (NoSQL DB) service.

![][7]

3.  Click **Create**.\
    Wait for the environment to be created & the app to start (\~4 minutes).

![][8]

4.  By clicking **Visit App URL**, access the Node-RED application

![][9]

5.  Run the wizard to configure authentication: secure your editor with your own credentials so only authorized users can access it (Node-RED has its own authentication system).

> Don't check **Allow anyone to view the editor, but not make any changes** and **Allow anyone to view the editor**.

![][10]

6.  On wizard last step screen, click on **Finish** to start Node-RED.

![][11]

7.  Node-RED is a browser-based editor that makes it easy to wire together flows that can be deployed to the runtime. In the case of IoT, Node-RED is really powerful to quickly test all the possibilities that IBM Cloud offers with different kind of services.\
    Your Node-RED app has a public URL like any web app (you defined it in step 2).\
    Click on **Go to your Node-RED flow editor** and use the credentials provided before.

![][12]

8.  You now have access to the Node-RED UI.\
    Keep the existing default flow and create a new flow: click **+**. You will use it in next exercise.

![][13]

Cloud: Create a virtual sensor and a new flow {#cloud-create-a-virtual-sensor-and-a-new-flow .HeadingPartorSection}
---------------------------------------------

Sensors & IoT  {#sensors-iot .HeadingSubsection}
--------------

1.  Open a new window or tab in your browser.

2.  To create a sensor simulator, connect to [[http://ibm.biz/iotsensor]{.underline}]\
    There are 3 simulated sensors:

    -   Object temperature

    -   Temperature

    -   Humidity

> The simulator (from IBM Cloud IoT Quickstart) connects automatically and starts publishing data.\
> It must remain connected to visualize the data.\
> Use the simulator buttons to change the simulated sensor readings. Data is published periodically.

![][14]

> Note: Instead of using your desktop browser, you can use your smartphone.

3.  Identify your virtual device ID (top right corner) : copy it. You will use it in next section.\
    Warning: if you reload this page, the device ID changes.

Node-RED flow: creation & import {#node-red-flow-creation-import .HeadingSubsection}
--------------------------------

1.  Go back to Node-RED window

2.  From left panel, drag and drop nodes to the workspace

    -   Chose the Input node **ibmiot**

    -   Add an output **debug** node

    -   Link them

![][15]

3.  Configure IBM IoT by double clicking on it :

    -   Authentication: Quickstart (means it is a simple authentication -- for demo purposes)

    -   Device ID : \<The value from Section 1, step 3 - Generated by the Simulator\>

![][16]

4.  Click **Done** & deploy your flow by clicking the **Deploy** button (top right).

5.  Check the **Debug** Panel on the right side while you are playing with the sensor simulator. You should receive Device (sensor = web app. you opened in other window) data as the IBM IoT Node subscribed to this particular Device topic.

> ![][17]![][18]

6.  Delete the whole flow by selecting all the nodes & pressing the 'Delete' key.

![][19]

7.  Now import a new flow. A flow can be exported and imported using JSON file.\
    Open link [[http://ibmcloud-watson-day.mybluemix.net/files/Lab3\_IoT\_Flow.v2.1.json]{.underline}] to display code in JSON format. You can also open the file you downloaded previously.

![][20]

8.  Select all and copy it

9.  Click on the top right button near **Deploy**.\
    Select **Import**, **Clipboard** & copy/paste the content of the JSON file in **Current flow**.

> ![][21]![][22]

10. Click on workspace to paste imported nodes

11. Fill in the **Device ID** field in the **IBM IoT App In** node.

![][23]

12. Click **Deploy** to deploy the new Flow.\
    Modify the Device Temperature & check the **Debug** logs.

![][24]

Node-RED on PI: Your first MQTT Simulator  {#node-red-on-pi-your-first-mqtt-simulator .HeadingPartorSection}
------------------------------------------

MQTT is a pub/sub protocol: you are going to write your own device that will publish messages to the IoT Platform service (MQTT Broker).

Using the Credentials, given in the Labs menu of the web application at http://Raspberry314-POT.eu-de.mybluemix.net, create on the Raspberry PI directly a NodeRed flow simulating a device sending data at regular intervals.

![][25]![][26]

![][27]

Cloud & Raspberry PI: Visual Recognition  {#cloud-raspberry-pi-visual-recognition .HeadingPartorSection}
-----------------------------------------

Using the URL given in the Labs menu of the web application at http://Raspberry314-POT.eu-de.mybluemix.net, which returns the last image of one of the Raspberry Pis, analyze the picture with Watson Visual Recognition.

You first need to create a Visual Recognition service in your IBM Cloud account and bind this service to your existing NodeRed application.

Then create this flow below with the HttpRequest node using the URL provided to get the last image taken by one of the Raspberry Pis.

This flow requires the installation of the package base64 (Menu Manage Palette of NodeRed).

![][28]

Export and import the flow on your Node-RED running on your Raspberry. Make sure all the nodes are properly installed on the target Node-RED. Adapt your flow (API Key, etc.) and test your flow.

Node-RED on PI: Grovepi Hat & Sensors (Optional \#1) {#node-red-on-pi-grovepi-hat-sensors-optional-1 .HeadingPartorSection}
----------------------------------------------------

GrovePi Hat Setup  {#grovepi-hat-setup .HeadingSubsection}
------------------

1.  Shutdown your Raspberry PI and connect the Hat as shown below

> ![Résultat de recherche d\'images pour \"grovepi connection\"]
>
> Source: <https://www.dexterindustries.com/GrovePi/get-started-with-the-grovepi/>

2.  Connect a few sensors on the Hat. Note that some ports are numbered as AXX (as Analogic) and others are numbered DXX as Digital.\
    LED, button, Ultrasonic sensors etc. are all Digital sensors.

    ![][29]

    ![][30]

3.  Reconnect your Raspberry PI : your Node-RED server should be back & running after a few seconds. Connect to Node-RED using

4.  If not already installed, Install the Grovepi Nodes:\
    - In the top right Menu, select "Manage Palette" and browse the already installed nodes. If not in the list, go to the **Install** panel type "grovepi" in the search bar, and search for "node-red-contrib-grovepi".\
    - Click on **install**

5.  If not done, install the Camera PI Nodes.

6.  Adapt your previous flow to create a Visual Recognition flow , using the Camera PI node instead of the previously used http get node.

First flow with Grovepi  {#first-flow-with-grovepi .HeadingSubsection}
------------------------

1.  Adapt your previous flow, and replace the **Inject** node which triggers the execution of the flow by a set of Grovepi nodes. For example, trigger the photo capture using an ultrasonic sensor (presence sensing) and/or a simple pushbutton switch.

    NB: An example of flow is available [here] on the **Lab** **Github** and can be imported using the top right menu, **Import** (from clipboard). Integrate this example with your existing flow.\
    \
    \
    ![][31]

![][32]

2.  Use an output sensor like a LED for customizing an action based on conditions.

![][33]

3.  Prototype testing & result sharing with other teams\
    *Example: Presence detection, Visual Recognition with Watson, actions based on detections.*

![][34]

Insert IoT Data in Cloudant DB (Optional \#2) {#insert-iot-data-in-cloudant-db-optional-2 .HeadingPartorSection}
---------------------------------------------

Let's insert the event data coming from the Device sensors in a Cloudant database!

Remember that you already have a Cloudant service deployed for Node-RED. You will use it to store your data.

1.  Add a Cloudant Node (**Cloudant OUT** node in the **Storage** Category) & link it to the **temp** function node

![][35]

2.  Configure Cloudant client in Node-RED.\
    2 Options:\
    A) Node-RED On IBM Cloud\
    B) Node-RED on any "remote" device (Raspberry PI, server...)

> **A) If running on IBM Cloud (Cloudant service auto-detection)**

-   Service : Cloudant service name bound to your Node.js runtime.\
    > *As Node.js is already bound to a Cloudant Service, the service name should appear in the Drop-down list.*

-   Database: name of your choice ([lower case]{.underline})

-   Name (node): name of your choice

> Click **Done**.

![][36]

B)  **If running on a remote device (Raspberry PI ...)\
    **

> ![][37]

-   In **Service**, select "External cloudant or couchdb service"

-   In **Server,** click on the button on the right

-   On IBM Cloud, Go to the Resource list page , then on the Cloudant database service running in your IBM Cloud workspace.This Cloudant Database service was created in your IBM Cloud account, in the first exercise.

-   On the service page, Go to **Service** **Credentials .\
    > If there is no credentials available, click the New Credentials button, click Add\
    > \
    > **![][38]

-   Click on **View Credentials** and copy/paste the information to your Node-RED config node as shown below

> ![][39]
>
> **Host** = url field in the Credentials.\
> **Username** and **Password** = username, password in the Credentials.\
> Click **Add**

-   Database: name of your choice ([lower case]{.underline})

-   Name (node): name of your choice.

-   Click **Done**

3.  Deploy your new flow (**Deploy** button)

4.  From your IBM Cloud Dashboard (IBM Cloud window of your browser), start the Cloudant dashboard by clicking on the line of Cloudant service

![][40]

5.  Click **Launch** button to start Cloudant console

![][41]

6.  Select **Database** icon in left panel, then your database name (defined in step 2).

![][42]

7.  Have a look to the inserted data in the database. Click on record to see content.

![][43]

Process IoT Data with Watson Language Translator (Optional \#3) {#process-iot-data-with-watson-language-translator-optional-3 .HeadingPartorSection}
---------------------------------------------------------------

1.  In IBM Cloud windows, add a « Watson Language Translator » service to your existing Node-RED application. From **Catalog**, click on **Language Translator** in Watson category.\
    Select **Lite** plan, then click **Create**. Service is deployed.

> **If you use Node-RED outside IBM Cloud, please skip steps 2, 3, 4, 5, 6 as they are related to service binding (Node-Red App Watson Service)\
> **

2.  From Language Translator dashboard, **Connections** menu, click on **Create connection**.

![][44]

3.  Connect service to your Node-RED application

![][45]

4.  Accept the restage step to actually bind the service to the app.

5.  While it is restaging (\~3 minutes), take a look at **Service Credentials**. This information is useful if you want to invoke your Watson Service from any program (running in IBM Cloud or outside IBM Cloud, on a Raspberry PI for example).

6.  Go back to Node-RED window.\
    If you get a connection error message, your application restaging is not finished. Wait. Try to refresh page.

![][46]

7.  Go back to the Node-RED environment.\
    Add **Language translator** node and link it between the template (*safe & danger) & debug* nodes (*cpustatus*)*.*

![][47]

8.  Configure the Watson language translator node (2 options: A or B)

<!-- -->

A)  If running Node-RED on IBM Cloud,

> Your Watson Service is connected/bound to your Node-RED App, you don't have to set any credentials on the Watson Node.\
> Note: The user/password fields are not necessary & do not appear in the node settings if a Watson Language Translator service is properly bound to Node-RED application.

-   Name (of your choice)

-   Mode: **Translate**

-   Domains: **Conversational**

-   Source: **English**

-   Target: **French** (or Spanish, Portuguese & Arabic)

B)  If Node-RED running on a remote location (server, Raspberry...)

> Go to your IBM Cloud dashboard, click on your Watson language translator service, **Credentials.** Copy the API Key**\
> **
>
> ![][48]
>
> ![][49]

-   Name (of your choice)

-   Mode: **Translate**

-   Domains: **Conversational**

-   Source: **English**

-   Target: **French** (or Spanish, Portuguese & Arabic)

9.  **Deploy** your flow & check the logs (**debug** tab).

![][50]

Create a dashboard application in Node-RED (optional \#4) {#create-a-dashboard-application-in-node-red-optional-4 .HeadingPartorSection}
---------------------------------------------------------

Import Node-RED Dashboarding capability  {#import-node-red-dashboarding-capability .HeadingSubsection}
----------------------------------------

The required file used during this lab can be downloaded the JSON files from

-   <http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Flow.v2.1.json>

-   <http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Dashboard.v2.1.json>

1.  At the top right-hand side of the page, click the 'burger' menu:

![][51]

2.  Click **Manage palette**:

![][52]

3.  In the sidebar that appears on the left-hand side of the page, click the Install tab:

![][53]

4.  In the search field, type **node-red-dash**:

![][54]

5.  Next to the nod-red-dashboard result, click **install**

![][55]

6.  Click **Install**

![][56]

7.  Wait for the new nodes to be deployed.\
    A popup shows which nodes have been installed.

![][57]

8.  Click **Close**.

9.  Note the additional dashboard nodes on the palette : **Dashboard** category.

![][58]

10. Note also that there is a new dashboard tab in the right-hand sidebar:

![][59]

*TIP: This dashboard tab may be used to add new tabs, menus etc. to the visualization dashboard. There are also two available themes -- light and dark.*

Create a simple Node-RED Dashboard {#create-a-simple-node-red-dashboard .HeadingSubsection}
----------------------------------

In this section, you will create a simple dashboard for sensor data using new dashboard nodes installed.

![][60]

1.  In [the current]{.underline} Node-RED tab, import the file named **Lab3\_IoT\_Dashboard.v2.0.txt** (previously downloaded): **Menu** \> **Import** \> **Clipboard**.\
    Click **current flow** then **Import**.

![][61]

2.  Click on workspace to paste new nodes.

![][62]

3.  Connect new nodes to existing...

    1.  **Temperature** to **temp**

    2.  **Humidity** to **IBM Iot App In**

    3.  **Object Temperature** to **IBM Iot App In**

    4.  **chart** to **temp**

![][63]

4.  Deploy the flow: **Deploy**

5.  Connect to **http://\<YOUR\_APP\_HOSTNAME\>/ui** (URL generated by Node-red-dashboard node) to see your new dashboard. Change value on virtual sensor app. to see impact on gauges and lines.

![][64]

6.  Customize your dashboard using **Dashboard** tab

-   **Theme**: use Dark style

![][65]

-   **Site**: as below, change **Title**, **1x1 Widget Size**, **Group spacing**

![][66]

7.  Deploy and check your dashboard.

 Add voice alert on dashboard {#add-voice-alert-on-dashboard .HeadingSubsection}
----------------------------

In this section, you will add a voice node allowing your app to tell say message when temperature change. To do that, you will deploy a new Watson service: Text to Speech.

Note: If you don't run Node-RED on IBM Cloud, you will have to copy the (Watson) service credentials and use them in each (Watson) node configuration for accessing the service remotely. The instructions below are designed for running on IBM Cloud.

1.  In the IBM Cloud window, click **Catalog** then **Watson** category.\
    Click on **Text to Speech** service to create your own instance.

![][67]

2.  Enter a name for your service and click **Create**.

3.  If running on Node-RED on IBM Cloud:\
    On new service dashboard, click **Connection**, and connect Text to speech service to your Node-RED application: **Connect**.

![][68]

> Accept to restage application and wait for your Node-RED application to restart.

![][69]

If running outside IBM Cloud, copy the Watson Service credentials (API key...) and use them in Step 8 -- node Configuration. Step 4 is only for IBM Cloud.

4.  When restarted, in Node-RED environment, add an **Audio out** node (category **Dashboard**) and a **text to speech** node (category **Watson**) to your flow

![][70]![][71]

8.  Connect nodes as below

    1.  **Test text change** to **language translator**

    2.  **Test text change** to **text to speech**

    3.  **text to speech** to **audio out**

![][72]

9.  Configure **text to speech** node to

    -   use the language your translated to (as configured before in **language translator** node)

    -   place the output on msg.payload (check box)

![][73]

10. Configure **audio out** node

    -   Set **group** to **chart \[IoT sensor\]**

    -   Check **Play audio when window not in focus**

![][74]

11. Deploy the flow: **Deploy**

12. Can you hear something?\
    Try to change temperature in virtual sensor app.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ END OF LAB \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Appendices: Architecture & Schema on IBM Cloud {#appendices-architecture-schema-on-ibm-cloud .HeadingPartorSection}
----------------------------------------------

![][75]

![][76]

  [/var/folders/71/fyvbq3v924l147w4g99kqt200000gn/T/com.microsoft.Word/WebArchiveCopyPasteTempFiles/leaders\_IoT.jpg]: media/image1.jpeg {width="3.034482720909886in" height="3.0517246281714785in"}
  [Before Starting 4]: #before-starting
  [Hands-on presentation 5]: #hands-on-presentation
  [Section 1. Overview 5]: #overview
  [Section 2. Prerequisites 5]: #prerequisites
  [Cloud: Create Node-RED application and Login 6]: #cloud-create-node-red-application-and-login
  [1. Cloud: Create a virtual sensor and a new flow 10]: #cloud-create-a-virtual-sensor-and-a-new-flow
  [Section 1. Sensors & IoT 10]: #sensors-iot
  [Section 2. Node-RED flow: creation & import 11]: #node-red-flow-creation-import
  [2. Node-RED on PI: Your first MQTT Simulator 14]: #node-red-on-pi-your-first-mqtt-simulator
  [3. Cloud & Raspberry PI: Visual Recognition 16]: #cloud-raspberry-pi-visual-recognition
  [4. Node-RED on PI: Grovepi Hat & Sensors (Optional \#1) 17]: #node-red-on-pi-grovepi-hat-sensors-optional-1
  [Section 1. GrovePi Hat Setup 17]: #grovepi-hat-setup
  [Section 2. First flow with Grovepi 18]: #first-flow-with-grovepi
  [5. Insert IoT Data in Cloudant DB (Optional \#2) 21]: #insert-iot-data-in-cloudant-db-optional-2
  [6. Process IoT Data with Watson Language Translator (Optional \#3) 25]: #process-iot-data-with-watson-language-translator-optional-3
  [7. Create a dashboard application in Node-RED (optional \#4) 29]: #create-a-dashboard-application-in-node-red-optional-4
  [Section 1. Import Node-RED Dashboarding capability 29]: #import-node-red-dashboarding-capability
  [Section 2. Create a simple Node-RED Dashboard 32]: #create-a-simple-node-red-dashboard
  [Section 3. Add voice alert on dashboard 35]: #add-voice-alert-on-dashboard
  [8. Appendices: Architecture & Schema on IBM Cloud 39]: #appendices-architecture-schema-on-ibm-cloud
  [1]: media/image2.png {width="6.666666666666667in" height="1.8055555555555556in"}
  [2]: media/image3.png {width="5.276189851268591in" height="2.202117235345582in"}
  [3]: media/image4.png {width="6.429166666666666in" height="0.8083333333333333in"}
  [4]: media/image5.png {width="2.787234251968504in" height="1.8217246281714785in"}
  [5]: media/image6.png {width="1.6604024496937884in" height="2.2446806649168853in"}
  [6]: media/image7.png {width="5.12038823272091in" height="2.904761592300962in"}
  [7]: media/image8.png {width="5.307384076990377in" height="3.9619050743657045in"}
  [8]: media/image9.png {width="5.138021653543307in" height="1.3047626859142607in"}
  [9]: media/image10.png {width="5.076303587051618in" height="1.238096019247594in"}
  [10]: media/image11.png {width="3.7708989501312336in" height="3.2952384076990375in"}
  [11]: media/image12.png {width="2.685715223097113in" height="2.371692913385827in"}
  [12]: media/image13.png {width="5.4380960192475944in" height="2.7401935695538056in"}
  [13]: media/image14.png {width="6.429166666666666in" height="0.7486111111111111in"}
  [[http://ibm.biz/iotsensor]{.underline}]: http://ibm.biz/iotsensor
  [14]: media/image15.png {width="2.466666666666667in" height="3.1714282589676293in"}
  [15]: media/image16.png {width="3.6952384076990374in" height="1.286428258967629in"}
  [16]: media/image17.png {width="2.8285717410323707in" height="1.7795647419072615in"}
  [17]: media/image18.png {width="1.2380949256342957in" height="1.9176213910761155in"}
  [18]: media/image19.png {width="4.533333333333333in" height="1.6452788713910762in"}
  [19]: media/image20.tiff {width="2.933333333333333in" height="0.9499136045494313in"}
  [[http://ibmcloud-watson-day.mybluemix.net/files/Lab3\_IoT\_Flow.v2.1.json]{.underline}]: http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Flow.v2.1.json
  [20]: media/image21.png {width="5.034389763779528in" height="2.409523184601925in"}
  [21]: media/image22.png {width="2.87619094488189in" height="1.968403324584427in"}
  [22]: media/image23.png {width="3.7367694663167104in" height="1.190475721784777in"}
  [23]: media/image24.png {width="5.086310148731409in" height="1.4157939632545933in"}
  [24]: media/image25.png {width="4.760827865266841in" height="1.8106364829396326in"}
  [25]: media/image26.png {width="5.125196850393701in" height="2.8645669291338582in"}
  [26]: media/image27.png {width="4.104724409448819in" height="4.149998906386702in"}
  [27]: media/image28.png {width="6.433464566929134in" height="2.900787401574803in"}
  [28]: media/image29.png {width="6.433464566929134in" height="4.644488188976378in"}
  [Résultat de recherche d\'images pour \"grovepi connection\"]: media/image30.jpeg {width="6.429166666666666in" height="2.3in"}
  [29]: media/image31.jpeg {width="4.3875in" height="3.29086176727909in"}
  [30]: media/image32.jpeg {width="4.5404133858267715in" height="3.4055555555555554in"}
  [here]: https://github.com/bmarolleau/watson-iot-labs/blob/master/lab6_grovepi_watson.json
  [31]: media/image33.png {width="4.4847222222222225in" height="1.4396839457567805in"}
  [32]: media/image34.png {width="3.6194444444444445in" height="2.7464457567804024in"}
  [33]: media/image35.png {width="6.429166666666666in" height="1.523611111111111in"}
  [34]: media/image36.jpeg {width="3.9700371828521437in" height="2.977742782152231in"}
  [35]: media/image37.png {width="5.772024278215223in" height="2.3167902449693787in"}
  [36]: media/image38.png {width="2.885715223097113in" height="2.0342989938757654in"}
  [37]: media/image39.png {width="3.5310279965004376in" height="2.826195319335083in"}
  [38]: media/image40.png {width="5.929166666666666in" height="2.1973392388451445in"}
  [39]: media/image41.png {width="4.470833333333333in" height="2.9028062117235347in"}
  [40]: media/image42.png {width="3.190475721784777in" height="2.2334700349956256in"}
  [41]: media/image43.png {width="4.619642388451443in" height="1.011451224846894in"}
  [42]: media/image44.png {width="6.429166666666666in" height="1.4388888888888889in"}
  [43]: media/image45.png {width="6.429166666666666in" height="1.5965277777777778in"}
  [44]: media/image46.png {width="5.133611111111111in" height="1.4666666666666666in"}
  [45]: media/image47.png {width="4.047619203849519in" height="1.5922911198600176in"}
  [46]: media/image48.png {width="4.114285870516185in" height="0.4519608486439195in"}
  [47]: media/image49.png {width="5.627718722659668in" height="2.59259186351706in"}
  [48]: media/image50.tiff {width="4.666666666666667in" height="2.52in"}
  [49]: media/image51.png {width="4.013888888888889in" height="5.074373359580052in"}
  [50]: media/image52.png {width="5.841734470691164in" height="2.0740748031496063in"}
  [51]: media/image53.png {width="3.686169072615923in" height="1.0531911636045495in"}
  [52]: media/image54.png {width="2.095744750656168in" height="2.910247156605424in"}
  [53]: media/image55.png {width="5.2347222222222225in" height="1.5955511811023622in"}
  [54]: media/image56.png {width="4.978079615048119in" height="2.0744674103237095in"}
  [55]: media/image56.png {width="5.00360564304462in" height="2.085107174103237in"}
  [56]: media/image57.png {width="4.446053149606299in" height="1.52127624671916in"}
  [57]: media/image58.emf {width="2.3617016622922136in" height="2.0381944444444446in"}
  [58]: media/image59.png {width="1.5744674103237095in" height="2.611072834645669in"}
  [59]: media/image60.png {width="4.329787839020122in" height="0.7983344269466317in"}
  [60]: media/image61.png {width="1.6543208661417323in" height="1.649381014873141in"}
  [61]: media/image62.png {width="2.8829790026246718in" height="1.962156605424322in"}
  [62]: media/image63.png {width="4.585106080489939in" height="2.5481069553805775in"}
  [63]: media/image64.png {width="6.429166666666666in" height="3.6493055555555554in"}
  [64]: media/image65.png {width="5.723404418197726in" height="2.978543307086614in"}
  [65]: media/image66.png {width="3.13580271216098in" height="2.2821675415573055in"}
  [66]: media/image67.png {width="3.111111111111111in" height="4.748992782152231in"}
  [67]: media/image68.png {width="5.934740813648294in" height="3.1923753280839895in"}
  [68]: media/image69.png {width="6.227736220472441in" height="2.1808508311461066in"}
  [69]: media/image70.png {width="2.5744685039370077in" height="1.3186570428696414in"}
  [70]: media/image71.png {width="1.0425535870516185in" height="0.6287160979877515in"}
  [71]: media/image72.png {width="6.908013998250219in" height="2.5638298337707788in"}
  [72]: media/image73.png {width="4.237293307086614in" height="2.3703696412948383in"}
  [73]: media/image74.png {width="3.7654319772528435in" height="3.176791338582677in"}
  [74]: media/image75.png {width="3.8764402887139107in" height="2.3914359142607173in"}
  [75]: media/image76.emf {width="6.429166666666666in" height="3.6166666666666667in"}
  [76]: media/image77.emf {width="6.429166666666666in" height="3.6166666666666667in"}
