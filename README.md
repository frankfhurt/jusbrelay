## jusbrelay ##

Multiplatform Java library for USB relay control

### Device examples ###

[5v 2 Channel USB Relay Module](http://www.amazon.com/Channel-Module-Programmable-Computer-Control/dp/B00NXLT6ZS/ref=sr_1_14?ie=UTF8&qid=1450713845&sr=8-14&keywords=usb+relay)<br>
[5v 1 Channel USB Relay Module](http://www.amazon.com/Channel-Module-Programmable-Computer-Control/dp/B00NXLN32U/ref=sr_1_4?ie=UTF8&qid=1450713845&sr=8-4&keywords=usb+relay)

### Supported platforms ###
- Windows (using vendors API library)
- Linux (need [libusb](http://www.libusb.org/) to be installed)
- Apple OS X (with pre-installed Xcode and plain IOKit libraries)
<br>Also Python (CPython) library is available for testing purposes.

### How to use? ###
Add in your **pom.xml** repository part:

```xml
<repository>
   <id>jusbrelay-mvn-repo</id>
   <url>https://raw.github.com/creepid/jusbrelay/mvn-repo/</url>
   <snapshots>
       <enabled>true</enabled>
       <updatePolicy>always</updatePolicy>
   </snapshots>
</repository>
```
And dependency:
```xml
<dependency>
   <groupId>by.creepid</groupId>
   <artifactId>jusbrelay</artifactId>
   <version>0.1</version>
</dependency>
```
### Code examples ###

```JAVA
		//Create relay manager
 		UsbRelayManager manager = NativeUsbRelayManager.getInstance();
        try {
			//init manager
            manager.relayInit();

			//enumerate devices 
            UsbRelayDeviceInfo[] devices = manager.deviceEnumerate();

            for (int i = 0; i < devices.length; i++) {
                UsbRelayDeviceInfo usbRelayDeviceInfo = devices[i];

				//retrieve device handler            
                UsbRelayDeviceHandler handler = manager.deviceOpen(usbRelayDeviceInfo.getSerialNumber());

                //change relay status
				//manager.openRelayChannel(UsbRelayDeviceHandler handler, int index); - for turning on, index - channel number
				//manager.closeRelayChannel(UsbRelayDeviceHandler handler, int index) - for turning off, index - channel number
				//manager.closeRelayChannels(UsbRelayDeviceHandler handler) - for turning off all channels
				//manager.openRelayChannels(UsbRelayDeviceHandler handler) - for turning on all channels

				//Get device status
                UsbRelayStatus[] statuses = manager.getStatus(usbRelayDeviceInfo.getSerialNumber(), handler);

				//close relay
                manager.closeRelay(handler);
            }

        } catch (UsbRelayException ex) {
            //catch exceptions
        } finally {
		   //close manager
           manager.relayExit();
        }
```
