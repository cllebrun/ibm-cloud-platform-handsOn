
# Introduction
<img src="./images/arm1.jpeg" width="20%"/> 

This tutorial demonstrates how to register your device into a private IBM Watson IoT Platform organisation and start sending commands to it.


# Objectives
* You will register your device to the Watson IoT platform.
* You will send commands to it.


# Pre-Requisites
Material:
*   Complete lab 1 on ARM: https://github.com/cllebrun/bluemix-handsOn/tree/master/labs/7.1%20Lab%20IoT%20-%20with%20ARM%201

The Watson IoT Platform within your application creates a private Internet of Things Platform organization, where you can register your device(s).

Open the IBM Cloud Platform Dashboard and click on the Application service that you have currently deployed, if you have already moved away from it. You should see the Watson IoT Platform listed under the Connections. Click on the WIoTP service and choose the click on Launch button to launch the WIoTP Dashboard.

# 1. Adding your device to your Watson Internet of Things Organization

From the Internet of Things service dashboard, access your IoT organisation and add your device to it. You might want take a look at the recipe How to Register Devices in IBM Watson IoT Platform, that explains in detail on how to add a Device Type and Device ID, within the Watson IoT Platform.

1. Select the Devices tab.
2. Select Add Device.
3. Enter a device type.
4. Enter a device ID.
        The device ID can be found by scrolling down on the LCD screen using the joystick button on a connected board. Alternatively, the device ID can be derived from your boards MAC address. Copy this from the Quickstart visualization page. Then remove all colons and make sure all letters are lowercase (ex. 01:23:45:67:89:AB becomes 0123456789ab)
5. During the next step of the device registration process you will get file configuration information containing the following details, copy these when you get them as you will need them in the next steps.
        Organization ID
        Device Type
        Device ID
        Authentication Method
        Authentication Token

# 2. Adding registration credentials onto the device

When you added your device to your Watson Internet of Things organization you were given device credentials. Your device will not send data to your organization until you add these credentials. You will add these using the mbed development environment.

1. To get the registration credentials onto your device, navigate to the IBMWIoTPSecureClientSample code repository: https://os.mbed.com/teams/IBM_IoT/code/IBMWIoTPSecureClientSample/
2. Click ‘Import this program’ on the right. It will import the program into the online compiler, you will need to sign in, or create an account.
3. Expand the IBMWIoTPSecureClientSample project and open the main.cpp file. Add the configuration details you were given when adding the device to your Internet of Things service organization.
4. The configuration details must be added using the following format:
```
    char *orgId = “OrganizationID”; <== Replace with correct OrgID
    char *deviceType = “DeviceType”; <== Replace with correct DeviceType
    char *deviceId = “DeviceID”; <== Replace with correct DeviceId
    char *method = “token”;
    char *token = “DeviceToken”; <== Replace with correct device token
```

5. Save the above made changes to main.cpp in online compiler
6. Before compiling, ensure that the correct device target (FRDM-K64F) has been selected on the right of the online compiler toolbar (it will be its own platform).
7. Within the online compiler, click Compile and save the bin file to the mbed drive.
8. Wait for the LED to stop flashing, then press the reset button on the device, which is located to the left of the LCD screen on the top of your microcontroller. This will run the download program.
9. The device will then run in registered mode.


# 3. Link your application and registered device

Using your Node-RED work flow editor (accessed from your application URL), update the IBM IoT App In node to work with your registered device.

1. Access your Node-RED application by clicking on View App link and then choosing the option Go to you Node-RED flow editor, to access the editor.
2. Parallelly, you can also access the Node-RED editor directly by accessing the following URL
```
    http://<your-application-name>.mybluemix.net/red
```

3. Double click the IBM IoT App In node in the flow editor.
4. Ensure that the node is configured with the following parameters:
  - Authentication: Bluemix Service
  - Input Type: Device Event
  - Device Type: ALL
  - Device ID: ALL
  - Event: ALL
  - Format: ALL
5. Click OK then Deploy.
6. Look at the debug pane to see the events and messages received from the devices that are registered to your organization.
7. You can configure the IBM IoT App In node to subscribe to events from a specific device by clearing ALL and supplying the Device IC from earlier in this recipe.

# 4. Sending commands from your application

When an mbed device is running in registered mode you can send it commands from an Watson Internet of Things application. For example, the sample code that you have loadedinto the device spports a simple command that will make the device’s onboard LED blink at a specified rate.

1. You can try this out by writing an application which connects to the Watson Internet of Things and publishes a message to the device.
2. Go to the Node-RED instance and the flow that was used earlier in the recipe. Navigate to the menu at the top right of the screen and select Import from Clipboard. Copy the JSON string from the text area below and paste it into the dialog box in Node-RED and select OK. If there any issues, copy the contents from github: https://raw.githubusercontent.com/ibm-messaging/iot-device-samples/master/mbed/ARM-mbed-Blink-LED.json
```
[
 {
 "id": "6fc0879c.903f78",
 "type": "ibmiot out",
 "authentication": "boundService",
 "apiKey": "",
 "outputType": "cmd",
 "deviceId": "160490130025",
 "deviceType": "iotsample-mbed-k64f",
 "eventCommandType": "blink",
 "format": "json",
 "data": "4",
 "name": "IBM IoT App Out",
 "service": "registered",
 "x": 847,
 "y": 518,
 "z": "cb4121a0.34bee",
 "wires": [] },
 {
 "id": "bdf085cb.420f78",
 "type": "function",
 "name": "blink rate",
 "func": "rate = Math.round(msg.payload * 5);nmsg.payload = "{ "rate": "+rate+ " }";nreturn msg;",
 "outputs": 1,
 "x": 587,
 "y": 477,
 "z": "cb4121a0.34bee",
 "wires": [
 [
 "6fc0879c.903f78",
 "5bde5cc2.a421a4"
 ] ] },
 {
 "id": "5bde5cc2.a421a4",
 "type": "debug",
 "name": "Show Blink Rate",
 "active": true,
 "console": "false",
 "complete": "payload",
 "x": 838,
 "y": 414,
 "z": "cb4121a0.34bee",
 "wires": [] },
 {
 "id": "1dc589ed.e23a76",
 "type": "comment",
 "name": "Control LED blink rate using Potentiometer1",
 "info": "The subflow converts the reading from potentiometer1 to an LED blink rate between 0 and 5.nn0 turns the LED offnnA value between 1 and 5 is the rate per second that the LED on the K64F will blink at.nnFinally the IoT Output node sends the blink command to the device. ",
 "x": 682,
 "y": 374,
 "z": "cb4121a0.34bee",
 "wires": [] }
] 
```
3. Wire the output of the function node that extracts the value of Pot1 to the input of the node labelled ‘blink rate’.
<img src="./images/node2.png" width="50%"/> 

4. Open the IBM IoT Out node and edit the configuration to include the following details:
        - Authentication: Bluemix Service
        - Output Type: Device Command
        - Device Type: same as the IBM IoT App In node
        - Device ID: The MAC address as earlier (lower caes with colons removed)
        - Command Type: Blink
        - Data: 1
5. Select OK and Deploy.
6. Now twist potentiometer 1 (positioned to the left of the device if the ethernet cable is at the top of the device) to cause the blue LED on the main board (not the application shield with the sensors) to flash at a rate between 1 and 5 times per second, or turn it off.

Note: Sending a rate of 0 will cause the LED to stop blinking.
