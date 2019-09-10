 {#section .ListParagraph}

{ Lab 3 }

**IoT application in IBM Cloud**

**with Node-RED and IBM Watson**

![/var/folders/71/fyvbq3v924l147w4g99kqt200000gn/T/com.microsoft.Word/WebArchiveCopyPasteTempFiles/leaders\_IoT.jpg]

June 2018 - v2.2

Authors: B. Marolleau, C. Bacle, C. Lalevée

June 2018 - v2.2

© Copyright IBM Corp. 2018

Materials may not be reproduced in whole or in part without the prior written permission of IBM

**Agenda**

[Before Starting 4]

[1. Hands-on presentation 5]

[Section 1. Overview 5]

[Section 2. Prerequisites 7]

[2. Create Node-RED application and Login 8]

[3. Create sensor and a new flow 11]

[Section 1. Sensors & IoT 11]

[Section 2. Node-RED flow: creation & importation 12]

[Section 3. Insert IoT Data in Cloudant DB 15]

[Section 4. Process IoT Data with Watson 17]

[4. Create a dashboard application in Node-RED (optional \#1) 19]

[Section 1. Import Node-RED Dashboarding capability 19]

[Section 2. Create a simple Node-RED Dashboard 22]

[Section 3. Add voice alert on dashboard 25]

[5. Create a dashboard in Watson IoT Platform (optional \#2) 28]

[Section 1. Create new device in Watson IoT Platform 28]

[Section 2. Node-RED: redirect sensor data to IBM IoT Platform 32]

[Section 3. IBM Watson IoT Platform: create dashboard 35]

![][1]Before Starting
---------------------

> This hands-on required to have an IBM Cloud account. If you don't, you can create one here: <http://bluemix.net/>.

-   Open a browser and access to IBM Cloud: <https://console.bluemix.net>.

-   If you have an IBM Cloud account, click **Log in**, and enter your IBM ID credentials.\
    If you don't have an IBM Cloud account, click **Create a free account**. Enter your email address, and additional information required. You will receive an email with activation link. Once activated, you could use your new free IBM cloud account: log in.

![][2]

-   Select organization, location and space to use during this lab.

![][3]

-   If needed, free resources (GB / \#Services) in your IBM Cloud Organization & Spaces to run the lab exercises.\
    If you encounter a resource contention (error message saying you are out of resources), clean up your spaces by deleting existing Apps or Services.

Hands-on presentation  {#hands-on-presentation .HeadingPartorSection}
----------------------

Overview  {#overview .HeadingSubsection}
---------

In this hands-on session, you will create a Node-RED application in IBM Cloud to collect, store and display virtual sensor data.

Node-RED (<https://nodered.org/>) is a flow-based programming tool, originally developed by the IBM Emerging Technology Services team (in early 2013) and now a part of JS Foundation.

Traditional development can be very technical, but Node-RED enables you to concentrate on the logic of your workflow and allows fast prototyping.

Node-RED consists of a Node.js-based runtime with a flow editor accessed through a web browser. Within the browser, you create your application by dragging nodes from a customizable palette into a workspace and start to wire them together. With a single click, the application is deployed back to its runtime.

Session objectives are:

-   Create & modify an application using Node-RED in IBM Cloud

-   Discover new services & Node-RED to consume or create services (IoT / database...)

-   Discover Watson services

-   Discover Watson IoT Platform

Find below lab overview (with exercise numbers).

![][4]

Expected results are :

-   Your Node-RED application is operational (using Node.js runtime), accessing Cloudant & IoT platform (QuickStart)

![][5]

-   Your Node-RED app is online (reachable from the Internet), & will be connected to a temperature simulator (sensor)

![][6]

-   Optionally, you are able to provide 2 dashboards: one with voice alert implemented in Node-RED, the second designed and hosted in Watson IoT Platform

> ![][7]![][8]

Prerequisites  {#prerequisites .HeadingSubsection}
--------------

The required file used during this lab can be downloaded the JSON files from

-   <http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Flow.v2.1.json>

-   <http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Dashboard.v2.1.json>

Create Node-RED application and Login {#create-node-red-application-and-login .HeadingPartorSection}
-------------------------------------

1.  In IBM Cloud Catalog, choose **Boilerplate** category

![][9]

2.  Click on **Internet of Things Platform Starter** to create an instance:\
    Fill in the App Name & Host name fields.\
    Note: Node-RED is a Node.js based application: using this boilerplate will instantiate a Node.js runtime + a Cloudant (NoSQL DB) service.

![][10]

3.  Click **Create**.\
    Wait for the environment to be created & the app to start (\~4 minutes).

![][11]

4.  By clicking **Visit App URL**, access the Node-RED application

![][12]

5.  Run the wizard to configure authentication: secure your editor with your own credentials so only authorized users can access it (Node-RED has its own authentication system).

> Don't check **Allow anyone to view the editor, but not make any changes** and **Allow anyone to view the editor**.

![][13]

6.  On wizard last step screen, click on **Finish** to start Node-RED.

![][14]

7.  Node-RED is a browser-based editor that makes it easy to wire together flows that can be deployed to the runtime. In the case of IoT, Node-RED is really powerful to quickly test all the possibilities that IBM Cloud offers with different kind of services.\
    Your Node-RED app has a public URL like any web app (you defined it in step 2).\
    Click on **Go to your Node-RED flow editor** and use the credentials provided before.

![][15]

8.  You now have access to the Node-RED UI.\
    Keep the existing default flow and create a new flow: click **+**. You will use it in next exercise.

![][16]

Create sensor and a new flow {#create-sensor-and-a-new-flow .HeadingPartorSection}
----------------------------

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

![][17]

> Note: Instead of using your desktop browser, you can use your smartphone.

3.  Identify your virtual device ID (top right corner) : copy it. You will use it in next section.\
    Warning: if you reload this page, the device ID changes.

Node-RED flow: creation & importation {#node-red-flow-creation-importation .HeadingSubsection}
-------------------------------------

1.  Go back to Node-RED window

2.  From left panel, drag and drop nodes to the workspace

    -   Chose the Input node **ibmiot**

    -   Add an output **debug** node

    -   Link them

![][18]

3.  Configure IBM IoT by double clicking on it :

    -   Authentication: Quickstart (means it is a simple authentication -- for demo purposes)

    -   Device ID : \<The value from Section 1, step 3 - Generated by the Simulator\>

![][19]

4.  Click **Done** & deploy your flow by clicking the **Deploy** button (top right).

5.  Check the **Debug** Panel on the right side while you are playing with the sensor simulator. You should receive Device (sensor = web app. you opened in other window) data as the IBM IoT Node subscribed to this particular Device topic.

> ![][20]![][21]

6.  Delete the whole flow by selecting all the nodes & pressing the 'Delete' key.

![][22]

7.  Now import a new flow. A flow can be exported and imported using JSON file.\
    Open link [[http://ibmcloud-watson-day.mybluemix.net/files/Lab3\_IoT\_Flow.v2.1.json]{.underline}] to display code in JSON format. You can also open the file you downloaded previously.

![][23]

8.  Select all and copy it

9.  Click on the top right button near **Deploy**.\
    Select **Import**, **Clipboard** & copy/paste the content of the JSON file in **Current flow**.

> ![][24]![][25]

10. Click on workspace to paste imported nodes

11. Fill in the **Device ID** field in the **IBM IoT App In** node.

![][26]

12. Click **Deploy** to deploy the new Flow.\
    Modify the Device Temperature & check the **Debug** logs.

![][27]

Insert IoT Data in Cloudant DB {#insert-iot-data-in-cloudant-db .HeadingSubsection}
------------------------------

Let's insert the event data coming from the Device sensors in a Cloudant database!

Remember that you already have a Cloudant service deployed for Node-RED. You will use it to store your data.

1.  Add a Cloudant Node (**Cloudant OUT** node in the **Storage** Category) & link it to the **temp** function node

![][28]

2.  Configure it:

    -   Service : Cloudant service name bound to your Node.js runtime.\
        > *As Node.js is already bound to a Cloudant Service, the service name should appear in the Drop-down list.*

    -   Database: name of your choice ([lower case]{.underline})

    -   Name (node): name of your choice

> Click **Done**.

![][29]

3.  Deploy your new flow (**Deploy** button)

4.  From your IBM Cloud Dashboard (IBM Cloud window of your browser), start the Cloudant dashboard by clicking on the line of Cloudant service

![][30]

5.  Click **Launch** button to start Cloudant console

![][31]

6.  Select **Database** icon in left panel, then your database name (defined in step 2).

![][32]

7.  Have a look to the inserted data in the database. Click on record to see content.

![][33]

Process IoT Data with Watson {#process-iot-data-with-watson .HeadingSubsection}
----------------------------

1.  In IBM Cloud windows, add a « Watson Language Translator » service to your existing Node-RED application. From **Catalog**, click on **Language Translator** in Watson category.\
    Select **Lite** plan, then click **Create**. Service is deployed.

2.  From Language Translator dashboard, **Connections** menu, click on **Create connection**.

![][34]

3.  Connect service to your Node-RED application

![][35]

4.  Accept the restage step to actually bind the service to the app.

5.  While it is restaging (\~3 minutes), take a look at **Service Credentials**. This information is useful if you want to invoke your Watson Service from any program (running in IBM Cloud or outside IBM Cloud)

6.  Go back to Node-RED window.\
    If you get a connection error message, your application restaging is not finished. Wait. Try to refresh page.

![][36]

7.  Go back to the Node-RED environment.\
    Add **Language translator** node and link it between the template (*safe & danger) & debug* nodes (*cpustatus*)*.*

![][37]

8.  Configure the Watson language translator node:

    -   Name (of your choice)

    -   Mode: **Translate**

    -   Domains: **Conversational**

    -   Source: **English**

    -   Target: **French** (or Spanish, Portuguese & Arabic)

> Note: The user/password fields are not necessary & do not appear in the node settings if a Watson Language Translator service is properly bound to Node-RED application.

9.  **Deploy** your flow & check the logs (**debug** tab).

![][38]

Create a dashboard application in Node-RED (optional \#1) {#create-a-dashboard-application-in-node-red-optional-1 .HeadingPartorSection}
---------------------------------------------------------

Import Node-RED Dashboarding capability  {#import-node-red-dashboarding-capability .HeadingSubsection}
----------------------------------------

1.  At the top right-hand side of the page, click the 'burger' menu:

![][39]

2.  Click **Manage palette**:

![][40]

3.  In the sidebar that appears on the left-hand side of the page, click the Install tab:

![][41]

4.  In the search field, type **node-red-dash**:

![][42]

5.  Next to the nod-red-dashboard result, click **install**

![][43]

6.  Click **Install**

![][44]

7.  Wait for the new nodes to be deployed.\
    A popup shows which nodes have been installed.

![][45]

8.  Click **Close**.

9.  Note the additional dashboard nodes on the palette : **Dashboard** category.

![][46]

10. Note also that there is a new dashboard tab in the right-hand sidebar:

![][47]

*TIP: This dashboard tab may be used to add new tabs, menus etc. to the visualization dashboard. There are also two available themes -- light and dark.*

Create a simple Node-RED Dashboard {#create-a-simple-node-red-dashboard .HeadingSubsection}
----------------------------------

In this section, you will create a simple dashboard for sensor data using new dashboard nodes installed.

![][48]

1.  In [the current]{.underline} Node-RED tab, import the file named **Lab3\_IoT\_Dashboard.v2.0.txt** (previously downloaded): **Menu** \> **Import** \> **Clipboard**.\
    Click **current flow** then **Import**.

![][49]

2.  Click on workspace to paste new nodes.

![][50]

3.  Connect new nodes to existing...

    1.  **Temperature** to **temp**

    2.  **Humidity** to **IBM Iot App In**

    3.  **Object Temperature** to **IBM Iot App In**

    4.  **chart** to **temp**

![][51]

4.  Deploy the flow: **Deploy**

5.  Connect to **http://\<YOUR\_APP\_HOSTNAME\>/ui** (URL generated by Node-red-dashboard node) to see your new dashboard. Change value on virtual sensor app. to see impact on gauges and lines.

![][52]

6.  Customize your dashboard using **Dashboard** tab

-   **Theme**: use Dark style

![][53]

-   **Site**: as below, change **Title**, **1x1 Widget Size**, **Group spacing**

![][54]

7.  Deploy and check your dashboard.

 Add voice alert on dashboard {#add-voice-alert-on-dashboard .HeadingSubsection}
----------------------------

In this section, you will add a voice node allowing your app to tell say message when temperature change. To do that, you will deploy a new Watson service: Text to Speech.

1.  In the IBM Cloud window, click **Catalog** then **Watson** category.\
    Click on **Text to Speech** service to create your own instance.

![][55]

2.  Enter a name for your service and click **Create**.

3.  On new service dashboard, click **Connection**, and connect Text to speech service to your Node-RED application: **Connect**.

![][56]

4.  Accept to restage application and wait for your Node-RED application to restart.

![][57]

5.  When restarted, in Node-RED environment, add an **Audio out** node (category **Dashboard**) and a **text to speech** node (category **Watson**) to your flow

![][58]![][59]

8.  Connect nodes as below

    1.  **Test text change** to **language translator**

    2.  **Test text change** to **text to speech**

    3.  **text to speech** to **audio out**

![][60]

9.  Configure **text to speech** node to

    -   use the language your translated to (as configured before in **language translator** node)

    -   place the output on msg.payload (check box)

![][61]

10. Configure **audio out** node

    -   Set **group** to **chart \[IoT sensor\]**

    -   Check **Play audio when window not in focus**

![][62]

11. Deploy the flow: **Deploy**

12. Do you hear something ?\
    Try to change temperature in virtual sensor app.

Create a dashboard in Watson IoT Platform (optional \#2) {#create-a-dashboard-in-watson-iot-platform-optional-2 .HeadingPartorSection}
--------------------------------------------------------

Create new device in Watson IoT Platform {#create-new-device-in-watson-iot-platform .HeadingSubsection}
----------------------------------------

1.  Switch back to your browser window open on IBM Cloud environment.

2.  From your dashboard, click on name of application created in exercise 2, to open application dashboard.

![][63]

3.  From left panel, click on **Connections** to see bound services.\
    Click **Internet of Things Platform** service.

![][64]

4.  Click **Launch** button to open your Watson IoT organization dashboard in a new browser tab. You are now connected to the IBM Watson IoT Platform dashboard. With the platform you can manage your devices, store and access your data.

![][65]

5.  From left panel **Settings** menu, activate the **Last Event Cache** feature :\
    By using the Watson IoT Platform Last Event Cache API, you can retrieve the last event that was sent by a device. This works whether the device is online or offline, which allows you to retrieve device status regardless of the device\'s physical location or use status. Last event data of a device can be retrieved for any specific event that occurred up to 7/45 days ago.

> ![][66]![][67]

6.  You are now going to register a new device in your organization: first adding a device type, then the device. Click on **Devices** menu from left panel.\
    Click on **Device Type**, then **Add device Type** button

![][68]

7.  Choose **Device** and put a name (case sensitive) for your device : *virtualsensor*\
    Click **Next**.\
    You don't need to add Device Information.\
    Click **Done**.

![][69]

8.  Click **Browse** and click on **Add Device** button on the right

![][70]

9.  Select **Device Type** you created before.\
    Give **Sensor1** as device' name (it will be your **Device ID**), click **Next**.

![][71]

10. Click **Next** again: you don't need any metadata.

11. For the security part, it is recommended to you to provide a simple token (between 8 and 36 characters long and should contain a mix of lower and upper-case letters, numbers and symbols). If you skip this, a token will be automatically generated but this one won't be easy to use for the next steps of this hands-on.\
    Click **Next**.

![][72]

12. A summary of your device details appears. Copy all these information in a text editor or as a screenshot. The token is unrecoverable.\
    Click **Done**.

13. In the **Security** menu on the left panel, click **Connection Security and** change security settings to accept non-SSL connections : **TLS Optional**

![][73]

14. Your device is created. You are now going to update your Node-RED application.

Node-RED: redirect sensor data to IBM IoT Platform {#node-red-redirect-sensor-data-to-ibm-iot-platform .HeadingSubsection}
--------------------------------------------------

1.  Switch back to your browser window opened on Node-RED development interface.

2.  Add a **Function** node and connect it to **IBM IoT App In** existing node

![][74]

3.  Open new node. Name it **Format Device Payload** and insert following code

> // Create MQTT message in JSON
>
> msg = {
>
> payload: JSON.stringify(
>
> {
>
> d:{
>
> \"temp\" : msg.payload.d.temp,
>
> \"humidity\" : msg.payload.d.humidity,
>
> \"objectTemp\": msg.payload.d.objectTemp
>
> }
>
> }
>
> )
>
> };
>
> return msg;
>
> Click **Done.**

**\
**

4.  Add an **ibmiot out** node and connect it to **Format Device Payload** node**.**

![][75]

5.  Open **ibmiot out** node and configure it as below**.**

-   Authentication : **Bluemix Service**

-   Output Type: **Device Event**

-   Device Type: virtualsensor (cf. Section 1, step 7)

-   Device Id: Sensor1 (cf. Section 1, step 9)

-   Event Type: update

-   Format: JSON

-   Data: *\<json sample\>\
    > *(Enter simple JSON sample like **{d:{"temp\":28,\"humidity\":79,\"objectTemp\":24}}**

-   Name: Send to IoT Platform

![][76]

> Click **Done.**

6.  Click **Deploy** to deploy your updated Node-RED flow **: ibmiot** nodes should now be connected.

![][77]

IBM Watson IoT Platform: create dashboard {#ibm-watson-iot-platform-create-dashboard .HeadingSubsection}
-----------------------------------------

1.  Switch back to your browser window opened on IBM Watson IoT Platform interface.

2.  In left panel, click **Device** menu

![][78]

3.  Click on **Sensor1**, your previously created device. Click on **State** to see the last device status, and last values of your Sensor1 device (remember that Sensor1 receives data from Node-RED, Node-RED receives data from your IoT sensor Web app).

![][79]

4.  You can also click on **Recent events** to see the last 5 payloads

> ![][80]![][81]

5.  We are now going to create a simple dashboard using IBM Watson IoT Platform.\
    From left panel, click on **Board**.

![][82]

6.  You can see that you already have some card. Have a look at **Usage Overview**. You can see cards about devices connected and data transferred.

![][83]

7.  Click **\<** (top left corner).\
    Click on **Create New Board.**

![][84]

8.  Click **\<** (top left corner).\
    Click on **Create New Board.\
    **Enter a name to your new board: *Sensor1*

![][85]

9.  Click **Next**, then **Summit.**

10. Click on your new board

![][86]

11. Click on **Add new card** button\
    Select **Line Chart**. Click **Next**.

![][87]

12. To collect data from your device, select Sensor1. Click **Next**.

![][88]

13. To select value to display, click on Connect new data set, and fill form as below.\
    Click **Next**.

![][89]

14. Change the card size to XL (you can change others parameters, if you want).\
    Click **Next**.

![][90]

15. Enter a card title. Click **Summit**.

![][91]

16. Your new card is available and displays live temperature values from Sensor1

![][92]

17. Following same steps (from 11 to 16), create 2 cards (gauges) to display **Humidity** and **Object temperature**.

![][93]

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ END OF LAB \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

  [/var/folders/71/fyvbq3v924l147w4g99kqt200000gn/T/com.microsoft.Word/WebArchiveCopyPasteTempFiles/leaders\_IoT.jpg]: media/image1.jpeg {width="3.034482720909886in" height="3.0517246281714785in"}
  [Before Starting 4]: #before-starting
  [1. Hands-on presentation 5]: #hands-on-presentation
  [Section 1. Overview 5]: #overview
  [Section 2. Prerequisites 7]: #prerequisites
  [2. Create Node-RED application and Login 8]: #create-node-red-application-and-login
  [3. Create sensor and a new flow 11]: #create-sensor-and-a-new-flow
  [Section 1. Sensors & IoT 11]: #sensors-iot
  [Section 2. Node-RED flow: creation & importation 12]: #node-red-flow-creation-importation
  [Section 3. Insert IoT Data in Cloudant DB 15]: #insert-iot-data-in-cloudant-db
  [Section 4. Process IoT Data with Watson 17]: #process-iot-data-with-watson
  [4. Create a dashboard application in Node-RED (optional \#1) 19]: #create-a-dashboard-application-in-node-red-optional-1
  [Section 1. Import Node-RED Dashboarding capability 19]: #import-node-red-dashboarding-capability
  [Section 2. Create a simple Node-RED Dashboard 22]: #create-a-simple-node-red-dashboard
  [Section 3. Add voice alert on dashboard 25]: #add-voice-alert-on-dashboard
  [5. Create a dashboard in Watson IoT Platform (optional \#2) 28]: #create-a-dashboard-in-watson-iot-platform-optional-2
  [Section 1. Create new device in Watson IoT Platform 28]: #create-new-device-in-watson-iot-platform
  [Section 2. Node-RED: redirect sensor data to IBM IoT Platform 32]: #node-red-redirect-sensor-data-to-ibm-iot-platform
  [Section 3. IBM Watson IoT Platform: create dashboard 35]: #ibm-watson-iot-platform-create-dashboard
  [1]: media/image2.png {width="6.666666666666667in" height="1.4722222222222223in"}
  [2]: media/image3.png {width="5.276189851268591in" height="2.202117235345582in"}
  [3]: media/image4.png {width="6.429166666666666in" height="0.8083333333333333in"}
  [4]: media/image5.png {width="7.301663385826772in" height="4.167935258092738in"}
  [5]: media/image6.png {width="2.787234251968504in" height="1.8217246281714785in"}
  [6]: media/image7.png {width="1.6604024496937884in" height="2.2446806649168853in"}
  [7]: media/image8.png {width="3.2940496500437444in" height="1.736687445319335in"}
  [8]: media/image9.png {width="3.180851924759405in" height="1.6726902887139108in"}
  [9]: media/image10.png {width="5.12038823272091in" height="2.904761592300962in"}
  [10]: media/image11.png {width="5.307384076990377in" height="3.9619050743657045in"}
  [11]: media/image12.png {width="5.138021653543307in" height="1.3047626859142607in"}
  [12]: media/image13.png {width="5.076303587051618in" height="1.238096019247594in"}
  [13]: media/image14.png {width="3.7708989501312336in" height="3.2952384076990375in"}
  [14]: media/image15.png {width="2.685715223097113in" height="2.371692913385827in"}
  [15]: media/image16.png {width="5.4380960192475944in" height="2.7401935695538056in"}
  [16]: media/image17.png {width="6.429166666666666in" height="0.7486111111111111in"}
  [[http://ibm.biz/iotsensor]{.underline}]: http://ibm.biz/iotsensor
  [17]: media/image18.png {width="2.466666666666667in" height="3.1714282589676293in"}
  [18]: media/image19.png {width="3.6952384076990374in" height="1.286428258967629in"}
  [19]: media/image20.png {width="2.8285717410323707in" height="1.7795647419072615in"}
  [20]: media/image21.png {width="1.2380949256342957in" height="1.9176213910761155in"}
  [21]: media/image22.png {width="4.533333333333333in" height="1.6452788713910762in"}
  [22]: media/image23.tiff {width="2.933333333333333in" height="0.9499136045494313in"}
  [[http://ibmcloud-watson-day.mybluemix.net/files/Lab3\_IoT\_Flow.v2.1.json]{.underline}]: http://ibmcloud-watson-day.mybluemix.net/files/Lab3_IoT_Flow.v2.1.json
  [23]: media/image24.png {width="5.034389763779528in" height="2.409523184601925in"}
  [24]: media/image25.png {width="2.87619094488189in" height="1.968403324584427in"}
  [25]: media/image26.png {width="3.7367694663167104in" height="1.190475721784777in"}
  [26]: media/image27.png {width="5.086310148731409in" height="1.4157939632545933in"}
  [27]: media/image28.png {width="4.760827865266841in" height="1.8106364829396326in"}
  [28]: media/image29.png {width="5.772024278215223in" height="2.3167902449693787in"}
  [29]: media/image30.png {width="2.885715223097113in" height="2.0342989938757654in"}
  [30]: media/image31.png {width="3.190475721784777in" height="2.2334700349956256in"}
  [31]: media/image32.png {width="4.619642388451443in" height="1.011451224846894in"}
  [32]: media/image33.png {width="6.429166666666666in" height="1.4388888888888889in"}
  [33]: media/image34.png {width="6.429166666666666in" height="1.5965277777777778in"}
  [34]: media/image35.png {width="5.133611111111111in" height="1.4666666666666666in"}
  [35]: media/image36.png {width="4.047619203849519in" height="1.5922911198600176in"}
  [36]: media/image37.png {width="4.114285870516185in" height="0.4519608486439195in"}
  [37]: media/image38.png {width="5.627718722659668in" height="2.59259186351706in"}
  [38]: media/image39.png {width="5.841734470691164in" height="2.0740748031496063in"}
  [39]: media/image40.png {width="3.686169072615923in" height="1.0531911636045495in"}
  [40]: media/image41.png {width="2.095744750656168in" height="2.910247156605424in"}
  [41]: media/image42.png {width="5.2347222222222225in" height="1.5955511811023622in"}
  [42]: media/image43.png {width="4.978079615048119in" height="2.0744674103237095in"}
  [43]: media/image43.png {width="5.00360564304462in" height="2.085107174103237in"}
  [44]: media/image44.png {width="4.446053149606299in" height="1.52127624671916in"}
  [45]: media/image45.emf {width="2.3617016622922136in" height="2.0381944444444446in"}
  [46]: media/image46.png {width="1.5744674103237095in" height="2.611072834645669in"}
  [47]: media/image47.png {width="4.329787839020122in" height="0.7983344269466317in"}
  [48]: media/image48.png {width="1.6543208661417323in" height="1.649381014873141in"}
  [49]: media/image49.png {width="2.8829790026246718in" height="1.962156605424322in"}
  [50]: media/image50.png {width="4.585106080489939in" height="2.5481069553805775in"}
  [51]: media/image51.png {width="6.429166666666666in" height="3.6493055555555554in"}
  [52]: media/image52.png {width="5.723404418197726in" height="2.978543307086614in"}
  [53]: media/image53.png {width="3.13580271216098in" height="2.2821675415573055in"}
  [54]: media/image54.png {width="3.111111111111111in" height="4.748992782152231in"}
  [55]: media/image55.png {width="5.934740813648294in" height="3.1923753280839895in"}
  [56]: media/image56.png {width="6.227736220472441in" height="2.1808508311461066in"}
  [57]: media/image57.png {width="2.5744685039370077in" height="1.3186570428696414in"}
  [58]: media/image58.png {width="1.0425535870516185in" height="0.6287160979877515in"}
  [59]: media/image59.png {width="6.908013998250219in" height="2.5638298337707788in"}
  [60]: media/image60.png {width="4.237293307086614in" height="2.3703696412948383in"}
  [61]: media/image61.png {width="3.7654319772528435in" height="3.176791338582677in"}
  [62]: media/image62.png {width="3.8764402887139107in" height="2.3914359142607173in"}
  [63]: media/image63.png {width="4.83117782152231in" height="1.5425535870516185in"}
  [64]: media/image64.png {width="4.5531911636045495in" height="2.184628171478565in"}
  [65]: media/image65.png {width="4.308510498687664in" height="1.961586832895888in"}
  [66]: media/image66.png {width="5.0in" height="1.6326421697287838in"}
  [67]: media/image67.png {width="2.5638298337707788in" height="4.213739063867017in"}
  [68]: media/image68.png {width="3.414893919510061in" height="1.4713101487314086in"}
  [69]: media/image69.png {width="5.067465004374453in" height="2.996800087489064in"}
  [70]: media/image70.png {width="6.429166666666666in" height="0.6340277777777777in"}
  [71]: media/image71.png {width="5.234043088363954in" height="2.325863954505687in"}
  [72]: media/image72.png {width="6.429166666666666in" height="3.2263888888888888in"}
  [73]: media/image73.png {width="4.3723403324584424in" height="2.5167639982502186in"}
  [74]: media/image74.png {width="5.388041338582677in" height="3.291139545056868in"}
  [75]: media/image75.png {width="6.041666666666667in" height="1.8333333333333333in"}
  [76]: media/image76.png {width="3.201431539807524in" height="3.5063298337707787in"}
  [77]: media/image77.png {width="5.288009623797025in" height="1.3924048556430446in"}
  [78]: media/image78.png {width="2.169507874015748in" height="1.4148939195100612in"}
  [79]: media/image79.png {width="7.304405074365704in" height="3.234041994750656in"}
  [80]: media/image80.png {width="2.9999978127734033in" height="1.5in"}
  [81]: media/image81.png {width="4.777340332458443in" height="3.04255249343832in"}
  [82]: media/image82.png {width="1.8055555555555556in" height="1.0277777777777777in"}
  [83]: media/image83.png {width="6.429166666666666in" height="2.0055555555555555in"}
  [84]: media/image84.png {width="1.5744674103237095in" height="0.34315288713910763in"}
  [85]: media/image85.png {width="5.6933070866141735in" height="2.9361701662292212in"}
  [86]: media/image86.png {width="3.677810586176728in" height="1.7021270778652668in"}
  [87]: media/image87.png {width="4.585761154855643in" height="3.3404254155730535in"}
  [88]: media/image88.png {width="5.338035870516186in" height="3.1489359142607176in"}
  [89]: media/image89.png {width="5.8435684601924756in" height="4.5531911636045495in"}
  [90]: media/image90.png {width="4.265956911636046in" height="3.156003937007874in"}
  [91]: media/image91.png {width="3.9906003937007872in" height="2.585106080489939in"}
  [92]: media/image92.png {width="4.212318460192476in" height="3.3510640857392824in"}
  [93]: media/image93.png {width="6.429166666666666in" height="2.6875in"}
