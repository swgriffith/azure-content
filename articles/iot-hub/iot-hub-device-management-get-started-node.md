<properties
	pageTitle="IoT Hub device management get started | Microsoft Azure"
	description="Azure IoT Hub for device management with C# getting started tutorial. Use Azure IoT Hub and C# with the Microsoft Azure IoT SDKs to implement device management."
	services="iot-hub"
	documentationCenter=".net"
	authors="ellenfosborne"
	manager="timlt"
	editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Get started with Azure IoT Hub device management using node.js (preview)

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Introduction
To get started with Azure IoT Hub device management, you need to create an Azure IoT Hub, provision devices in the IoT Hub, and start multiple simulated devices. This tutorial walks you through these steps.

> [AZURE.NOTE]  You need to create a new IoT Hub to enable device management capabilities even if you have an existing IoT Hub because existing IoT Hubs do not have device management capabilities yet. Once device management is generally available, all existing IoT Hubs will be upgraded to get device management capabilities.

## Prerequisites

You need the following installed to complete the steps:

- Git
- node
- npm
- CMake (version 2.8 or later). Install CMake from <https://cmake.org/download/>. Make sure to check the box to add CMake to the current user PATH variable.
- An active Azure subscription.

	If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see [Azure Free Trial][lnk-free-trial].

## Create a device management enabled IoT Hub

You need to create a device management enabled IoT Hub for your simulated devices to connect to. The following steps show you how to complete this task using the Azure portal.

1.  Sign in to the [Azure portal].
2.  In the Jumpbar, click **New**, then click **Internet of Things**, and then click **Azure IoT Hub**.

	![][img-new-hub]

3.  In the **IoT Hub** blade, choose the configuration for your IoT Hub.

	![][img-configure-hub]

  -   In the **Name** box, enter a name for your IoT Hub. If the **Name** is valid and available, a green check mark appears in the **Name** box.
  -   Select a **Pricing and scale tier**. This tutorial does not require a specific tier.
  -   In **Resource group**, create a new resource group, or select an existing one. For more information, see [Using resource groups to manage your Azure resources].
  -   Check the box to **Enable Device Management**.
  -   In **Location**, select the location to host your IoT Hub. IoT Hub device management is only available in East US, North Europe, and East Asia during public preview. In the future, it will be available in all regions.

  > [AZURE.NOTE]  If you don't check the box to **Enable Device Management** the samples won't work.

4.  When you have chosen your IoT Hub configuration options, click **Create**. It can take a few minutes for Azure to create your IoT Hub. To check the status, you can monitor the progress on the **Startboard** or in the **Notifications** panel.

	![][img-monitor]

5.  When the IoT Hub has been created successfully, open the blade of the new IoT Hub, make a note of the **Hostname**, and then click the **Keys** icon.

	![][img-keys]

6.  Click the **iothubowner** policy, then copy and make note of the connection string in the **iothubowner** blade. Copy it to a location you can access later because you will need it to complete the rest of this tutorial.

 	> [AZURE.NOTE] In production scenarios, make sure to refrain from using the **iothubowner** credentials.

	![][img-connection]

You have now created a device management enabled IoT Hub. You will need the connection string to complete the rest of this tutorial.

## Build the samples and provision devices in your IoT Hub

In this section, you will run a script that builds the simulated device and the samples and provisions a set of new device identities in the device registry of your IoT Hub. A device cannot connect to IoT Hub unless it has an entry in the device registry.

To build the samples and provision devices in you IoT Hub, follow the steps below:

1.  Open terminal.

2.  Clone the github repository. **Make sure to clone in a directory that does not have any spaces.**

	  ```
	  git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
	  ```

3.  From the root folder where you cloned the **azure-iot-sdks** repository, navigate to the **azure-iot-sdks/node/service/samples** directory and run, replacing the placeholder value with your connection string from the previous section:

	  ```
	  setup.bat <IoT Hub Connection String>
	  ```

This script does the following:

1.  Runs **cmake** to create the necessary make files to build the simulated device. The executable is in **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Note that the source files are in the folder **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.

2.  Builds the simulated device executable **iotdm\_simple\_sample**.

3.  Runs ``` npm install ``` to install the necessary packages.

4.  Runs ```node generate_devices.js``` to provision device identities in your IoT Hub. The devices are described in **sampledevices.json**. After the devices are provisioned, the credentials are stored in the **devicecreds.txt** file (located in the **azure-iot-sdks/node/service/samples** directory).

## Start your simulated devices

Now that the devices have been added to the device registry, you can start simulated managed devices. One simulated device needs to be started for each device identity provisioned in the Azure IoT Hub.

Using terminal, in the **azure-iot-sdks/node/service/samples** directory, run:

  ```
  simulate.sh
  ```

This script outputs the commands you need to run to start **iotdm\_simple\_sample** for each device listed in the **devicecreds.txt** file. Please run the commands individually from a separate terminal window for each simulated device. The simulated device will continue to run until you close the command window.

The **iotdm\_simple\_sample** application is built using the Azure IoT Hub device management client library for C, which enables the creation of IoT devices that can be managed by Azure IoT Hub. Device makers can use this library to report device properties and implement the execute actions required by device jobs. This library is a component delivered as part of the open source Azure IoT Hub SDKs.

When you run **simulate.sh**, you see a stream of data in the output window. This output shows the incoming and outgoing traffic as well as **printf** statements in the application specific callback functions. This allows you to see incoming and outgoing traffic along with how the sample application is handling the decoded packets. When the device connects to the IoT Hub, the service automatically starts to observe resources on the device. The IoT Hub DM client library then invokes the device callbacks to retrieve the latest values from the device.

Below is output from the **iotdm\_simple\_sample** sample application. At the top you see a successful **REGISTERED** message, showing the device with Id **Device11-7ce4a850** connecting to IoT Hub.

> [AZURE.NOTE]  To have less verbose output, build and run the retail configuration.

![][img-output]

Make sure to leave all the simulated devices running as you complete the tutorials in "Next steps".

## Next steps

To learn more about the Azure IoT Hub device management features you can go through the tutorials:

- [How to use the device twin][lnk-tutorial-twin]

- [How to find device twins using queries][lnk-tutorial-queries]

- [How to use device jobs to update device firmware][lnk-tutorial-jobs]

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started/image2.png
[img-monitor]: media/iot-hub-device-management-get-started/image3.png
[img-keys]: media/iot-hub-device-management-get-started/image4.png
[img-connection]: media/iot-hub-device-management-get-started/image5.png
[img-output]: media/iot-hub-device-management-get-started/image6.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md
