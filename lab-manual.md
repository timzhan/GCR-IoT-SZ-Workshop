IoT hub and Time Series Insights Hands on Lab - Guide
-----------------------------------------
因为使用的是Azure Global，我们建把Portal语言使用英文显示。参考下图。

<image src="media\mc\00-set-UI-english.png" width="50%">

Azure Time Series Insights (TSI) is an end-to-end PaaS offering to *ingest, process, 
store, and query highly contextualized, time-series-optimized, IoT-scale
data.* Time Series Insights is tailored towards the unique needs of industrial
IoT deployments with capabilities including multi-layered storage, time series
modeling, and cost-effective queries over decades of data. Time Series Insights
provides rich asset-based operational intelligence together with ad-hoc data
exploration to address the IoT analytics needs.

**  **

**Step 1: Create an IoT hub**

Time Series Insights uses an IoT hub or an Event Hub as an event source. In this
lab, you will plug in your IoT Hub to a TSI environment.

*Create a new IoT hub:*

1.  Sign into Azure Portal using your subscription account

2.  Select **+ Create a resource** in the upper left.

    <img src="media\mc\01-create-a-resource.png" width="75%">

3.  Input "IoT Hub", then punch Enter to the IoT Hub blade.

    <img src="media\mc\02-iot-hub.png" width="50%">

4.  Enter the following:

    | **Property**       | Description                                                                                                                          |
    |--------------------|--------------------------------------------------------------------------------------------------------------------------------------|
    | **Subscription**   | Select the subscription to use for your IoT hub.                                                                                     |
    | **Resource Group** | Select your resource group(or click "Create new" to create a new resource group) for your IoT hub                                                                                          |
    | **Region**         | This is the region in which you want your hub to be located. Select the location closest to you from the dropdown list.              |
    | **IoT Hub Name**   | Put in the name for your IoT Hub. This name must be globally unique. If the name you enter is available, a green check mark appears. |

    <img src="media\mc\03-iot-hub-basics.png" width="75%">

    Click on "Next: Networking", leave it as default. Click on "Next: Size and Scale".

5.  In the ‘size and scale’ tab, select S1 as the Pricing and scale tier, 1 as
    the total number of units and 4 default device-to-cloud partitions (**all
    default settings**)

    <img src="media\mc\04-iot-hub-size-and-scale.png" width="75%">

    Click on “Review + Create”.

6.  Review and click on “Create”

    <img src="media\mc\05-iot-hub-review+create.png" width="75%">


7. Deployment complete

    <img src="media\mc\06-iot-hub-deployment-complete.png" width="100%">


8. Go to your IoT hub
   
   Click "Go to resource" to your IoT hub.
    <img src="media\mc\07-iot-hub-blade.png" width="100%">


*Define a new consumer group*

TSI requires a unique consumer group for your hub. The following steps detail
how you can create a new consumer group in your hub.

1.  In your IoT hub, go to “Built-in endpoints”, under the “Consumer Groups” section, type a unique name for your TSI consumer group to create one.

    <img src="media\mc\08-create-tsi-cg.png" width="100%">


** **

**Step 2: Send Events to your hub**

In this step, you will simulate 3 devices and send data to your IoT Hub

1.  In your IoT hub, go to “IoT devices” and click on “+ New”

    <img src="media\mc\09-iot-hub-create-iot-device.png" width="100%">

2.  Enter “device-01” in the “deviceId” textbox and click "Save" button (you can leave the
    default settings enabled)

    <img src="media\mc\10-create-iot-device-01.png" width="75%">

3.  Save the primary connection string for the device. Go to “IoT
    devices” and click on “device-01”

    <img src="media\mc\11-select-iot-device-01.png" width="100%">

4.  Copy the “Connection String (primary key)” for this device and save it to the notepad. You are going to
    use this endpoint to send events.

    <img src="media\mc\12-copy-connection-string.png" width="100%">

5.  Repeat the above steps (step 1 – 4) to create 2 more devices with Ids: “device-02” and “device-03”. Save their primary connection strings to the notepad as well.
   
    <img src="media\mc\13-iot-devices-list.png" width="100%">

6.  To send events you will need a Raspberry Pi web simulator. Go to
    [https://azure-samples.github.io/raspberry-pi-web-simulator/\#GetStarted](https://azure-samples.github.io/raspberry-pi-web-simulator/)

    <img src="media\mc\14-online-rpi-simulator.png" width="100%">


7.  Press "Next" button twice to Step 3. In the “coding area”, replace the placeholder text in Line 15 with the
    primary connection string of the device-01 you saved.

    <img src="media\mc\15-rpi-code-connection-string.png" width="100%">

8.  Click on “Run” at the bottom right of the page to execute the code. You
    should see messages being sent to the hub.

    <img src="media\mc\16-code-run.png" width="75%">

9.  Follow below instructions to repeat the above steps (Step 6-8) for “device-02” and “device-03”

    > Open 2 new tabs in your browser and go to
    >[https://azure-samples.github.io/raspberry-pi-web-simulator/\#GetStarted](https://azure-samples.github.io/raspberry-pi-web-simulator/).
    > Replace the connection string for devices: “device-02” and “device-03” and
    > click on run to send events to the hub.

    > **Do not close any of the 3 browser tabs for this lab.**

5 minutes later, you can go to "Overview" panel of IoT Hub to check how many telemetry massages are sent to IoT Hub, as below:

<img src="media\mc\17-view-telemetry.png" width="100%">

> You can also use the "Azure IoT Explorer" tool to view devices' telemetries. Go to this link: https://github.com/Azure/azure-iot-explorer/releases to download and install the most recent version of the app.  
> 
> The first time you run Azure IoT explorer, you're prompted for your IoT hub's connection string. You can find the connection string in "Shared access policies" in your iot hub Settings. 

<img src="media\mc\18-iot-hub-owner-sas.png" width="100%">

>Copy and add your iot hub connection string to Azure IoT Explorer, click "Connect". 
>
><img src="media\mc\19-add-iot-hub-sas-iotexplorer.png" width="100%">
> 
> Select your device by clicking the device name
>
><img src="media\mc\20-select-device-01.png" width="100%">
>
> Select "Telemetry" and click "Start" button, now you can see the telemetries that the device sent.
>
><img src="media\mc\21-view-telemetry.png" width="100%">
>

**  **  
**Step 3: Create a Time Series Preview Environment**

This section describes how to create a Time Series Insights update environment
using the Azure Portal.

1.  Sign into Azure Portal using your subscription account

2.  Select **+ Create a resource** in the upper left.

3.  Select the **Internet of Things** category, then select **Time Series
    Insights.**
    ![](media\mc\22-search-tsi.png)

4.  On this page, you will configure your TSI environment. Fill in the fields on
    the page as follows:

| **Property**             | Description                                                                                                                                                                                                                                                    |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Environment name**     | Choose a unique name fo­r the Azure Time Series Insights environment.                                                                                                                                                                                  |
| **Subscription**         | Enter your subscription where you want to create the Azure Time Series Insights environment. It's a best practice to use the same subscription as the rest of your IoT hub.                                                                            |
| **Resource group**       | A resource group is a container for Azure resources. Choose an existing resource group, or create a new one, for the Azure Time Series Insights environment resource. It's a best practice to use the same resource group as the rest of your IoT hub. |
| **Location**             | Choose a datacenter region for your Azure Time Series Insights Preview environment. To avoid added bandwidth costs and latency, it's best to keep the Azure Time Series Insights environment in the same region as your IoT hub.                       |
| **Tier**                 | Select **PAYG**, which stands for pay-as-you-go. This is the SKU for the Azure Time Series Insights product.                                                                                                                                           |
| **Property name**        | **Enter something that uniquely identifies your time series. Note that this field is immutable and can't be changed later. For this lab, use “iothub-connection-device-id”.**                                                                                  |
| **Storage account name** | Enter a globally unique name for a new storage account to be created.                                                                                                                                                                                          |
 Click on “Next: Event Source”. 
    ![](media\mc\23-create-tsi-basics.png)

1.  On this page, you will configure your event source for TSI. Fill the
    following fields below:

| **Property**                   | Description                                                                                                                |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| **Create an event source?**    | Enter **Yes**.                                                                                                             |
| **Name**                       | Enter a unique name that's used to identify the event source.                                                              |
| **Source type**                | Enter **IoT Hub**.                                                                                                         |
| **Select a hub?**              | Enter **Select existing**.                                                                                                 |
| **Subscription**               | Enter your subscription.                                                                                                   |
| **IoT hub name**               | Select your IoT hub that you created in step 1                                                                             |
| **IoT Hub Access Policy name** | Select “iothubowner”                                                                                                       |
| **IoT Hub consumer group**     | Select the consumer group that you created in step 1                                                                       |
| **Timestamp property**         | This field is used to identify the timestamp property in your incoming telemetry data. For this lab, leave the field blank |

Click on “Next: Review + Create”.
    ![](media\mc\24-tsi-event-source.png)

6.  Review all the details and click “Create” to start provisioning your
    environment.

    ![](media\mc\25-tsi-review-create.png)

7.  You will receive a notification once the deployment has completed
    successfully.

** **

**Step 4: Explore data in your TSI Environment**

In this section, you will perform basic analysis on your new TSI environment.

1.  Go to your Azure Time Series Insights environment first,and then click TSI Explorer.

    ![](media\mc\26-tsi-overview.png)

2.  Wait for the environment to load in the browser. Once it loads, on the
    left-hand pane, expand “Time Series Instances” to see all the time series in the environment.

    ![](media\mc\27-tsi-explorer-view.png)

3.  Click on the first time series “device-01”, and then tick on humidity” and add. You will see a time series chart to the right. EventCount, humidity, messageId and temperature are the default variables created in the environment. Details can be found in “Step 5: Analyze data by defining a Model” the section below.

    ![](media\mc\28-tsi-explorer-device-01.png)

4. Repeat the above steps (step 1-3) to add device-02 and device-03 the TSI Explorer Insights.

    ![](media\mc\29-tsi-explorer-devices-01-03.png)

** **
**Step 5: Contextualize and Analyze data**

In the previous section, you explored raw data without contextualization. In this section, you will add time series model entities to contextualize your IoT data.

Time Series Model has 3 components: Types, Hierarchies and Instances.

- Types* allow users to define calculations and aggregates over raw telemetry data and specify a tag for the sensor (example: Temperature sensor, Pressure sensor). In this lab you will use the ‘default type’ that performs a count operation.

- *Hierarchies* allow users to specify the structure of their assets. For
    example, an organization has buildings and buildings have rooms which
    contain IoT devices. The hierarchy structure in this case will be (Building -\> Room).

- *Instances* enrich incoming IoT data with device metadata. An instance links to 1 type definition and multiple hierarchy definitions.

1. In the upper left part of the explorer select the Model tab:
    ![](media\mc\30-tsi-explorer-model.png)

2. For this lab we will use the default Type created by TSI.

3. The next step is to add a hierarchy. In the Hierarchies section, select +Add Hierachy.

    ![](media\mc\31-tsi-model-hierachies.png)

4. A tab will open on the right-hand side. Add the following values:

| Name    | Enter **Location Hierarchy** |
|---------|------------------------------|
| Level | Enter **City**                   |
| Level | Enter **Building**               |
| Level | Enter **Floor**                  |

   ![](media\mc\32-tsi-add-new-hierarchy.png)

Click “Save”.

5. You will see a hierarchy created:

    ![](media\mc\33-tsi-hierarchy-view.png)

6. Next, click on “Instances” and click on “device-01”. To edit this Instance, click on “Edit”.

    ![](media\mc\34-tsi-instance-view.png)

7. On the Right-Hand Side, enter the following fields

| **Type**        | Select **Default Type**        |
|-----------------|--------------------------------|
| **Name**        | Enter **“Chiller_01”**          |
| **Description** | Enter **any description, like Chiller_01**      |
| **Hierarchies** | Enable **Location Hierarchy**. |
| **City**        | Enter **Seattle**.             |
| **Building**    | Enter **Space Needle**.        |
| **Floor**       | Enter **Floor 40**             |

   <img src="media\mc\35-tsi-edit-device-properties.png" width="50%"><img src="media\mc\36-tsi-edit-device-instance-fields.png" width="50%"> 

Click “Save”.

8. You will see the instances populated:

    ![](media\mc\37-tsi-instance-populated.png)

9. Repeat above steps(step 5-7) for device-02 and device-03 with the following values:

*Device-02:*

| **Type**        | Select **Default Type**           |
|-----------------|-----------------------------------|
| **Name**        | Enter **“Chiller_02”**             |
| **Description** | Enter **any description**         |
| **Hierarchies** | Enable **Location Hierarchy**.    |
| **City**        | Enter **Seattle**.                |
| **Building**    | Enter **Pacific Science Center**. |
| **Floor**       | Enter **Floor 20**                |

*Device 3:*

| **Type**        | Select **Default Type**         |
|-----------------|---------------------------------|
| **Name**        | Enter **“Chiller_03”**           |
| **Description** | Enter **any description**       |
| **Hierarchies** | Enable **Location Hierarchy**.  |
| **City**        | Enter **New York**.             |
| **Building**    | Enter **Empire State Building** |
| **Floor**       | Enter **Floor 30**              |

10. Navigate back to the “Analyze tab”. You will see a hierarchy that enables easy navigation and that provides context to the raw sensor data. See below:
   ![](media\mc\38-tsi-analytics-with-hierarchy.png)

11. Now navigate the hierarchy by going to “New York” -\> “Empire State
    Building” -\> “Floor 30”. Click on Chiller_3 and select “humidity” and click Add. 

    ![](media\mc\39-tsi-analytics-add-chiller3.png)

12. To compare “Chiller_3” with “Chiller_2” humidity, add Chiller_2 humidity as follows:

    ![](media\mc\40-tsi-analytics-show-chiller2-3.png)

At the end of the lab, cleanup resources that you created – IoT hub and
TSI.

Reference: https://github.com/rangv/MarchWorkshop/tree/master/AzureTimeSeriesInsights