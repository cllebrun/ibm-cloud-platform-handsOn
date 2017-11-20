
# Introduction
<img src="./images/iotp_icon_64.png" width="20%"/> 

This tutorial demonstrates how to subscribe to device events from an app


1. Fork the source code: 
```
  $ git clone https://github.com/cllebrun/dashboard-iot-demo.git
```
2. Modify credentials in the server.js file and in the index.js file with your IoT organization credentials
3. Modify the manifest file with the own appname and service names of your choice
4. Push your app to bluemix in the same space than your Watson IoT service is created

  ```
  $ bx app push
  ```

5. To do more: look at the doc and at the service documentation: https://console.bluemix.net/docs/services/IoT/applications/mqtt.html#mqtt

5. To do more: look at the doc and at the API documentation: https://console.bluemix.net/docs/services/IoT/reference/api.html#api_overview

6. Try to: add a button to list the several devices in your organisation !

