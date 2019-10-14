---
name: USB-CAN Analyzer
category: 
bzurl: 
oldwikiname: 
prodimagename: 
surveyurl: 
sku: 114991193

---

![enter image description here](https://www.seeedstudio.site/media/catalog/product/cache/ab187aaa5f626ad16c8031644cd2de5b/h/t/httpsstatics3.seeedstudio.comseeedfile2017-06bazaar487719_1.jpg)

The USB-CAN Analyzer is a cost-effective high quality and easy to use USB to CAN adapter. Compared with the 50$ or more expensive CAN Bus Analyzer, CAN Bus Tester, or CAN Bus Sniffer, the USB-CAN Analyzer only needs half the price, but it is even more convenient and easy to use. You can just communicate your CAN Bus device with your computer via the USB cable, then this tiny case will work as your can bus analyzer tool, can bus diagnostic tools, or can bus scanner.
 
<p style="text-align:center"><a href="https://www.seeedstudio.com/USB-CAN-Analyzer-p-2888.html" target="_blank"><img src="https://github.com/SeeedDocument/wiki_english/raw/master/docs/images/300px-Get_One_Now_Banner-ragular.png" /></a></p>



## Features

- Optimized the conversion protocol, improved conversion efficiency.
- Saving the customized settings automatically.
- Hex number converts to Decimal number visualized in the software. Decimal number show both value with symbol and without symbol, don’t need to use calculator.
- Customized receiving ID, easy to debug.
- Visualized CAN bus status. Convenient for analyze CAN Bus problem.
- Can be saved as Excel or TXT file.
- Receiving data can be refreshed and checked in order.


## Hardware Overview

- Integrated TVS Surge Protection
- Including 120 ohm matched resistance.


![enter image description here](https://github.com/SeeedDocument/USB-CAN-Analyzer/raw/master/img/120-re.jpg)


You can see an onboard green jumper cap as the following figure shown. When you add this jumper on, a 120Ω terminating resistor will be added to the circuit. Conversely, when you remove the jumper cap, the resistor will not be added into the circuit. Normally, you need to keep this jumper cap because the CAN device under test and the CAN analyzer are at the ends of the CAN network. If you want to know more, please check [120-ohm terminating resistor](https://www.ni.com/zh-cn/innovations/white-papers/09/can-physical-layer-and-termination-guide.html) page from NI.



## Software in winwdows

### Basic Function

- Support CAN2.0A(Standard) and CAN2.0B（Expansion）

- CAN baud rate（5K~1M）, customized CAN baud rate.

- CAN send and receive data with time tag，can show the receiving data in order，can refresh data easily.

- Data can be sent by single frame, multiple frames, manually, regularly, you can even set a certain time to send.

### Enhanced Function

- Response to data received from a certain ID.

- Can check CAN Bus status manually.

- Can set to receive from the wanting ID directly, without setting filtering ID or shield ID.

4 working mode

- Standard Mode: CAN communication

- Loop Mode: Self-testing, in this mode, the analyzer will send and receive data itself, and also send data to CAN Bus.

- Quiet Mode: Only use to monitor CAN Bus without influence.

- Loop Quiet Mode: Warm testing

- Data can be saved as TXT or Excel.
Baud rate of virtual COM port can also be modified, default baud rate over 1M, so don’t need to worry about conversion efficiency.

### Advanced function

- All customized settings saved automatically.

- Easy secondary development，only need to handle 1 command.

- Transparent Transition function

- Software supports English

## Getting Started

### Materials required

| Raspberry pi | USB-CAN Analyzer| Arduino Board |CAN-BUS Shield V2 |
|--------------|-------------|-----------------|-----|
|![enter image description here](https://github.com/SeeedDocument/wiki_english/raw/master/docs/images/rasp.jpg)|![enter image description here](https://github.com/SeeedDocument/USB-CAN-Analyzer/raw/master/img/analyzer%20-%20210-157.jpg)|![enter image description here](https://raw.githubusercontent.com/SeeedDocument/Grove_Light_Sensor/master/images/gs_1.jpg)|![](https://github.com/SeeedDocument/2-Channel-CAN-BUS-FD-Shield-for-Raspberry-Pi/raw/master/img/CAN_BUS_Shield_V2.jpg)|
|[Get ONE Now](https://www.seeedstudio.com/Raspberry-Pi-3-Model-B-p-2625.html)|[Get ONE Now](https://www.seeedstudio.com/USB-CAN-Analyzer-p-2888.html)|[Get ONE Now](https://www.seeedstudio.com/Seeeduino-V4-2-p-2517.html)|[Get ONE Now](https://www.seeedstudio.com/CAN-BUS-Shield-V2.html)|


Also we need to two [male to male jumper](https://www.seeedstudio.com/Breadboard-Jumper-Wire-Pack-241mm-200mm-160mm-117m-p-234.html) and power cables to power those boards.



### Hardware Connection

 
- **Step 1**. Plug the CAN BUS Shield V2 into the Seeeduino(or Arduino) Board

- **Step 2**. Use the jumpers to connect the CAN terminal of both shield.

|USB-CAN Analyzer|CAN-BUS Shield V2|
|---|---|
|CAN_0_L|CANL|
|CAN_0_H|CANH|

!!!Tip
    You can find the silkscreen at the back of the shield.

- **Step 3**. Power the USB-CAN Analyzer and Seeeduino from computer.


![](https://github.com/SeeedDocument/USB-CAN-Analyzer/raw/master/img/analyzer-connect-CAN-BUS-Shield-V2.jpg)


### Software

#### Install driver

**1.For Linux**

Get the CH341SER driver code. and install all linux kernel drivers

```bash
git clone https://github.com/SeeedDocument/USB-CAN-Analyzer.git
cd ~/USB-CAN-Analyzer/res/Driver
unzip CH341SER_LINUX.ZIP
cd /CH341SER_LINUX
make load 
```

!!!Tip
    It is not required for Raspberry to install this driver . Because OS of Raspberry has installed this driver 

**2.For Windows**

You should click the [link](https://github.com/SeeedDocument/USB-CAN-Analyzer/raw/master/res/Driver/driver%20for%20USBCAN(CHS40)/windows-driver/CH341SER.EXE) to get driver and  double-click
to install it

#### Communicate with Arduino CAN BUS Shield

For Arduino CAN BUS Shield, we provide the Arduino Code, if you don't know how to use Arduino, please check [here](http://wiki.seeedstudio.com/Getting_Started_with_Arduino/).

Arduino Code for CAN BUS Shield:

```C
// demo: CAN-BUS Shield, send data
// loovee@seeed.cc

#include <mcp_can.h>
#include <SPI.h>

// the cs pin of the version after v1.1 is default to D9
// v0.9b and v1.0 is default D10
const int SPI_CS_PIN = 9;

MCP_CAN CAN(SPI_CS_PIN);                                    // Set CS pin

void setup()
{
    Serial.begin(115200);

    while (CAN_OK != CAN.begin(CAN_500KBPS))              // init can bus : baudrate = 500k
    {
        Serial.println("CAN BUS Shield init fail");
        Serial.println(" Init CAN BUS Shield again");
        delay(100);
    }
    Serial.println("CAN BUS Shield init ok!");
}

unsigned char stmp[8] = {0, 0, 0, 0, 0, 0, 0, 0};
void loop()
{
    //send data:  id = 0x00, standrad frame, data len = 8, stmp: data buf
    stmp[7] = stmp[7]+1;
    if(stmp[7] == 100)
    {
        stmp[7] = 0;
        stmp[6] = stmp[6] + 1;
        
        if(stmp[6] == 100)
        {
            stmp[6] = 0;
            stmp[5] = stmp[6] + 1;
        }
    }
    
    CAN.sendMsgBuf(0x00, 0, 8, stmp);
    delay(100);                       // send data per 100ms
}
// END FILE
```

For USB-CAN Analyzer , we provide two way that include windows and linux .

**1.For Linux**

```bash
cd ~
git clone https://github.com/kobolt/usb-can
cd ~/usb-can
gcc -o canusb canusb.c
./canusb -d /dev/ttyUSB0 -s 500000 -b 9600
```

we assume that you only inset one USB to TTL on your raspberry.

**2.For Windows**


You should click the [link](https://github.com/SeeedDocument/USB-CAN-Analyzer/raw/master/res/Program/USB-CAN(V8.00).exe) to get software.



## Resources

- **[PDF]** [Software on Windows](https://github.com/SeeedDocument/USB-CAN-Analyzer/tree/master/res/Document)


## Tech Support
Please submit any technical issue into our [forum](http://forum.seeedstudio.com/) or drop mail to techsupport@seeed.cc

<br /><p style="text-align:center"><a href="https://www.seeedstudio.com/act-4.html?utm_source=wiki&utm_medium=wikibanner&utm_campaign=newproducts" target="_blank"><img src="https://github.com/SeeedDocument/Wiki_Banner/raw/master/new_product.jpg" /></a></p>
