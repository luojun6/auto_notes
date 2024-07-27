# Unified Diagnostic Services (UDS)

## 1 Overview

### 1.1 Automotive Diagnostic System

![uds_0](./images/uds_0.png)

Automotive Diagnostic System consists of

- a diagnostic tester
- a vehicle communication interface
- OBD2 cable connected with vehicle OBD2 port
  - Also called as J1962 port

**The diagnostic tester**, could be an end user application running on a PC or a laptop, or in a tablet or in some special handheld devices.

The diagnostic software is connected with the vehicle communication interface via a USB cable. And the other side of the USB cable is RS232 connector which connected to the VCI.

The other side of the VCI is RS232 connector and the vehicle side is a J1962 connector.

**The communication interface**, is also called as a protocol converter which converts the USB signal into CAN, or any other protocol which are supported by the vehicle communication interface
and the vehicle issues together.

**From UDS perspective:**

- **Client**, as the UDS specification the diagnostic tester, whhich consists of the PC software or the VCI and the cable together.
- **Server**, the ECUs within the diagnostic server with the diagnostic server application, or the software which takes care of the diagnostic related functions.

### 1.2 UDS Diagnostic Clients

![uds_1](./images/uds_1.png)

- Peak CAN USB VCA from **_Peak system_**
- VIN1000 from **Softing AG**
- Multi-diagram
- ADS525x from Bosch

Generally these diagnostic clients, are developed by OEM, and it's provided to their dealer vehicle service stations.

### 1.3 ECUs in a Car (on CAN Bus)

![veh_ecus](./images/veh_ecus.png)

The CAN communication lines are extended to OBD2 port via diagnostic CAN.

The ECU can be connected in different. You know topologies in the vehicle. But every individual ECU can be accssed by the tester from this diagnostic port.

As we discussed previously, every ECU insdie the vehicle as assigned unique identifier their address, that means every ECU inside the vehicle must be assigned with an unique ID.

The Same address can be also used the physical address of these ECUs, can be also used as diagnostic server address.

### 1.4 UDS Diagnostic Servers

![uds_2](./images/uds_2.png)

Here are two examples of ECUs which can act as diagnostic server:

- A Bosch engine management system ECU
- A Visteon digital cockpit ECU with instrument cluster, entertainment, connectivity, etc.

The OEM shall ensure that each of the ECU in the vehicle has a unique server identifier. The OEM must also ensure that the client in the system has also a client identifier that means the diagnostic tester also should be assigned with a unique ID.

The physical or the target address of the ECU shall be the same address of the diagnostic server.

### 1.5 UDS Diagnostic Servers - AUTOSAR

What are the elements you know which forms the diagnostic server inside the ECU?

![uds_3](./images/uds_3.png)

By taking the example of AUTOSAR architecture, the highlited areas where you find different components, can act as a diagnostic server for the tester, which will be connected to the diagnostic port, and number of service identifies are requested from the tester.

In this software architecture, the diagnostic software components acts a server for the diagnostic tester.

### 1.6 Automotive Diagnostic Addressing

![uds_4](./images/uds_4.png)

So the diagnostic tester are the client request different services from ECU services of the ECU diagnostic servers, with the well defined structured protocol called UDS or unified diagnstic services.

There is a client address assigned to each of the tester, and there is should be an address for every ECU inside the vehicle.

So the client can always address the ECU with two types of addressing mechanism:

- physical addressing
- functional addressing

#### 1.6.1 Physical Addressing

When the tester uses the physical addressing mode, the physical addressing shall always be a dedicated message to a diagnostic server software which is implemented in a particular ECU.

When a physical addressing is used the communication is a **point to point** communication that means the diagnostic test is talking to a specific ECU in the vehicle.

#### 1.6.2 Functional Addressing

The next addressing mechanism of functional addressing is used by the client. If it does not know the physical address of the server function that shall respond to a diagnostic service request, or if the server function is implemented as a distributed function in several ECUs, then the functional addressing method will be helpful to get the required data from each of the ECU or required functions in different ECU to perform actually.

When a functional addressing is used, the communication is a broadcast communication from the client to all the servers present in the vehicle.

![uds_5](./images/uds_5.png)

### 1.7 Automotive Diagnostic Request

![uds_6](./images/uds_6.png)

The client always request for a specific service which needs to be executed in the ECU diagnostics server application.

And the server respond to the tester with the response and allows ECU software functions to perform necessary diagnostic operations.

There are two direct responses are possible from the ECU:

- A positive response
- A negative response

#### 1.7.1 Positive Response

A positive response inform the client or the diagnostic tester that the server as the requested data are the server or the ECU can perform the requested operation from the tester.

#### 1.7.2 Negative Response

A negative response from the ECU diagnostic servr informs the client that the requested service from the tester is not supported. z

Due to the invalid form of request, or some data length is not supprted and few more reasons are available.

#### 1.7.3 SID - Service Identifiers

Why an active response is sent by the ECU server? The diagnostic tester requests the ECU to perform diagnostic related software execution with a unique ID.

These unique ID numbers are called as service identifiers or SIDS, which is a one byte unsigned integer value.

There are 26 unique ID request can sent by the client to the vehicle ECU. Each unique request as called as service identifiers are SIDS.

### 1.8 UDS: SID Functional Units

![uds_7](./images/uds_7.png)

The 26 SIDS are formed into 6 different functional units. These 6 functionals units of differnet set of SIDS are assigned to them.

This grouping of SIDS are done to perform a specific set of operation with the help of each SIDS within one functional unit.

![uds_8](./images/uds_8.png)

#### 1.8.1 Diagnostic and Communiction Managment functional unit (10 SIDs)

![uds_9](./images/uds_9.png)

#### 1.8.2 Data Transmission functional unit (7 SIDs)

![uds_10](./images/uds_10.png)

#### 1.8.3 SID Functional Units

![uds_11](./images/uds_11.png)

#### 1.8.4 Upload Download functional unit (5 SIDs)

![uds_12](./images/uds_12.png)

## 2 UDS: Diagnostic Session Control (SID: 0x10)

### 2.1 UDS: ECU Diagnostic Sessions

| Service ID(s) | Session Definition               |
| ------------- | -------------------------------- |
| 0x01          | Default Session                  |
| 0x02          | Programming Session              |
| 0x03          | Extended Diagnostic Session      |
| 0x04          | Safety System Diagnostic Session |
| 0x40 - 0x5F   | Vehicle Manufactureer Sepecific  |
| 0x60 - 0xyE   | System Supplier Specific         |

Session control service 0x10 is used to enable different diagnostic sessions in the ECU.

### 2.2 UDS: ECU Diagnostic Default Session

In normal operating condition of an ECU, default diagnostic session (**DEFAULT_SESSION**) is always active or in other terms whenever ECU is powered-on, its current active diagnostic session is set to **DEFAULT_SESSION**.

![uds_13](./images/uds_13.png)

So the **DEFAULT_SESSION** is the first session of the diagnostic session.

### 2.3 Non-default Sessions

- **Programming Session**

  - This diagnostic sesion enables all diagnostic services required to support the memory programming of a server.
  - is used for flashing
  - the ECU at the EOL, or a software ban or updated the ECU garage
  - remote software updates like FOTA, SOTA are happening with the help of programming sessions

- **Extended Diagnostic Session**

  - This diagnostic session can be used to enable all diagnostic services required to support the adjustment of functions like "Idel Speed, CO Value, etc." in the server's memory.
  -

- **Safety System Diagnostic Session**

  - This diagnostic sesion enables all diagnostic services required to support safety system related functions (e.g., airbog deployment).

- **Vehicle Manufacturer Specific**

  - This range of value is reserved for vehicle manufacturer specific use.

- **System Supplier Specific**

  - This range of value is reserved for system supplier specific use.

### 2.4 UDS: SIDs Supported by Different Sessions

![uds_14](./images/uds_14.png)

### 2.5 UDS: Non-Default Session SIDs

![uds_15](./images/uds_15.png)

### 2.6 UDS: ECU Diagnostic Session State Diagram

![uds_16](./images/uds_16.png)

### 2.7 UDS: DiagnosticSessionControl(ox10) Service

#### 2.7.1 Positive Response 0x50

![uds_17](./images/uds_17.png)

What do we do with the help of tester is that we are able to get into these different kinds of session with the help of a request message from the tester.

And the ECU responds with a postive response, by informing that the ECU server diagnostics services are successully executed, and you know it got into those sessions.

In DiagnosticSessionControl(0x10),support these 6 sub-functions which are nothing but the different sessions of ECU diagnostic server.

With the help of this sub-function, the client can request the ECU diagnostic server to all possible diagnostic sessions, that means the tester or the client sends the SID request 0x10 DiagnosticSessionControl service request with the sub-function 0x01, to set the current session to default session.

If you replace the sub-function 0x01 with 0x02 or 0x03 or 0x04 ... or 0x70 or any other diagnostic session with that particular ID. Then it is possible to get into those sessions you know which is requested by the diagnositc tester.

So the request message starts with 0x10 which is nothing but diagnostic session control 0x10, and sub-function ID where you can see start with 0x1, 0x02, 0x04 and 0x40 to 0x5F, 0x60 to 0x7E. When the request is sent then the ECU diagnostic server responds back to the diagnostic tester with the positive response and the positive value is 0x50.

There is a relation between the tester SID and the response which is received from the diagnostic server. So whenever you have a positive response, the server always adds a value 0x40 into the SID request. So that means if you receive a value of 50 that means it's a positive response, and whatever the session that have been asked from the diagnostic tester is successfully executed in the ECU.

#### 2.7.2 Negative Response 0x7F

![uds_18](./images/uds_18.png)

Note: The 0x22 - contions NOT Correct is a unqiue situation, where it is expected that whenever you perform a diagnstic session in the vehicle. It is expected that the vehicle is not moving or the engine should not be running. These are few environmental conditons, that will impact your diagnostic session.

## 3 UDS: Input Output By Identifier - SID: 0x2F

### 3.1 Service Description

The client requests the control of an input/output specific to the server. The server

### 3.2 Engine Management System - Block Diagram

![uds_19](./images/uds_19.png)

- Left hand side -> a number of sensors
- Right hand side -> a number of actuators

The understanding of InputOutputByIdentifier_0x2F in this example is to substitute a value for the sensor, or take a momentory control of the actuators directly from the tester by providing a SID along with some parameter values.

So these are helpful in the garage diagnostic to identify some of the faults in some particular components. Either both in the sensor side as well as on the actuator side.

### 3.3 Bosch ESP ECU

![uds_20](./images/uds_20.png)

The input unit on the left side and output unit on the right side, the values can be substituted with the help InputOutputByIdentifier_0x2F, and momentarily we can take control of these values from diagnostic tester, thereby creating some scenario to understand how the given or observed component is working.

### 3.4 The Input Output Conrol by Identifier

The Input Output Conrol by Identifier service is used by the client to substitute a value for an input signal, internal server function, either it could be an input signal directly which is comming from the sensor. You can change the value of it, or any sub-function which is using this sensor signal.

![uds_21](./images/uds_21.png)

Then there is some calculation happens internally even then with the help of a specifiic data indentifiers you can replace those values.

That is also possible, so either you can directly change the input signal and then use. The client to substitude the value of input signal that means either it could be a input signal.

Force control to a value for an output (actuator) of an electronic system.

![uds_22](./images/uds_22.png)

The DID here are used to forced the control to an output actuator of an electronic system. Anything like motors, solenoid wall, pumps, heaters and relays all these comes under actuator.

And these can be directly controlled by the diagnostic tester with the help of assigned DID for each of the element, with some specific parameter value we should be in a position to override the function from the ECU server.

And the diagnostic tester takes over the control of this component momentarily, we call it short-term-adjustment.

### 3.5 dataidentifier - Input Signals

The client request message contains a **dataidentifier** to reference the input server funciton, and/or output signal(s) - acuators(s) (in case of a device control might reference a group of signals) of the server.

![uds_23](./images/uds_23.png)

Before going into the request response, understanding this particular SID, there are few impact and calibration parameter which is happening inside the ECU need to be understood, because these are the data element which we are going to control from the diagnostic tester.

So a dataidentifier is nothing but you can say an array of signal packed into a data packet with unique dataidentifier value. So for example you can view this as like a one-dimension array with n number of elements in it.

In this example, we have defined DID `0x00` which has 10 signals in it, and every signal as a unique value assigned to it. These unique values are previously well defined within the control unit itself, so that when you give the reference value of `0x0C`, it's always understood that is engine speed, these are mapped internally in the software.

So when you use a DID with the value `0x0C` or `0x0D` or `0x05` then it means that engine speed value, vehicle speed value and engine temperature value, and so on. Like this you can have n number of DID defined inside the ECU.

Why these DIDs are required, so since the ECU as several inputs and there are several calculated values which are created internally by the software.

When the vehicle is going for a svervice, if there are some issue with specific circuit or with specific components in the vehicle, then these data are read through a service called `ReadDataByIdentifier`, in that service also this DIDs are used to read the real-time values of the signals which are there as part of the ECU.

In this example this DID `0x00` consists of 10 signals, so I can define one more DID `0x01` and I can have another 10 signals of different values variables.

These DIDs can be called outside from a service called `ReadDataByIdentifier` where you can act, this can act like you know a snapshot of values coming out of ECU.

But in our specific data IO control any data by IO control, our idea is to change override this value with the help of our parameters in order to momentarily sit and see what's happening by the consumers of these signals thereby understanding the software flow or the effect of execution in the software.

So that is the whole idea of doing this short-term adjustments.

### 3.6 dataidentifier - Actuators

| DID | Parameter                              |
| --- | -------------------------------------- |
| 4C  | Commanded throttle actuator            |
| 69  | Actual EGR, Command EGR, and EGR Error |
| 74  | Turbocharger RPM                       |
| A5  | Commanded Diesel Exhaust Fluid Dosing  |

These individual component value can be also set from the diagnostic tester and override this parameters in the ECU server.

This is also can be done with the short-term adjustment parameters.

### 3.7 Control Option

![uds_24](./images/uds_24.png)

### 3.8 controlEnabledMaskRecord

![uds_25](./images/uds_25.png)

There is one more parameter which should be sent as part of our request is that controlled enable mask assigning.

Sometimes I don't want to change the `0x0C`, but I want to change only `0x0D`. In that case, there is one more parameter called **controlEnabledMaskRecord**.

This parameter is used along with the request where it says that what are all the parameter which I want to change as part of the request.

### 3.9 controlEnabledMaskRecord

![uds_26](./images/uds_26.png)

### 3.10 Input Output Control Identifier Request SID: 0x2F

![uds_27](./images/uds_27.png)

![uds_28](./images/uds_28.png)

![uds_29](./images/uds_29.png)

- `0x03` is the **shortTermAdjustment** identifier
- `0x3C` is the presentage of vale which we are expecting the air inlet door position.

![uds_30](./images/uds_30.png)

## 4 UDS: Security Access - SID: 0x27

### 4.1 Service Description

The purpose of this service is to provide a means to access data and/or diagnostic service, wihch have restricted for access security, emissions, or safety reasons.

The Direct Services for downloading, uploading routines are data into a server and reading specific memory locations from a server. A situation where the security access is requied.

That means in simple terms the diagnostic test should access to some of the diagnostic server controls, or reading data by changing a security mechanism between the tester and the ECU.

The security mechanism will understand that there is an authenticated request is happening on the critical parameters of the ECU. And the issue will allow the tester to read such data from the server or it actually accept the reqeust and send the data back to the diagnostic.

![uds_31](./images/uds_31.png)

### 4.2 Seed & Key Algorithms

The security mechanism in the ECU is called as **Seed & Key Algorithms**.

![uds_32](./images/uds_32.png)

### 4.3 Security Access Type = request Seed

![uds_33](./images/uds_33.png)

### 4.4 Security Levels

![uds_34](./images/uds_34.png)

![uds_35](./images/uds_35.png)

### 4.5 Client Request message format

![uds_36](./images/uds_36.png)

### 4.6 Request message - Request Seed

![uds_37](./images/uds_37.png)

### 4.7 Request message - Send Seed

![uds_38](./images/uds_38.png)

### 4.8 Supported Negative Response Codes (NRC)

| Data Byte | Parameter Name                             |
| --------- | ------------------------------------------ |
| 0x12      | sub-function Not Supported                 |
| 0x13      | Incorrect Message Length Or Invalid Format |
| 0x22      | Conditions Not Correct                     |
| 0x24      | Request out of Range                       |
| 0x31      | Security Access Denied                     |
| 0x35      | Invalid Key                                |
| 0x36      | Exceeded Number Of Attempts                |
| 0x37      | Required Time Delay Not Expired            |

### 4.9 Message flow example - Security Access

![uds_39](./images/uds_39.png)

### 4.10 Example #2 - server is in an "unlock" state

![uds_40](./images/uds_40.png)

If the diagnostic tester again sending a request at the same security level - `0x01` level, if it is sending a request of a requesting seed then the response could be at the two five bytes byte the positive response could be `0x00` `0x00` from the diagnostic server, that means from the ECU telling that alrady the ECU has been unlocked and you can start performing the required operation.

So if the security level when the ECU is unlocked and you are trying to send apart from `0x01` to `0x03` or `0x05` something like that, then you will have a negative response. It won't allow you to go to the next level or it will throw.

## 5 UDS: Communication Control - SID: 0x28

### 5.1 Service Description

The purpose of this service is to switch on/off the transmission and/or the reception of certain messages of (a) server(s) (e.g. application communication messages or network management messages).

### 5.2 Communication Control SID 0x28

![uds_41](./images/uds_41.png)

![uds_42](./images/uds_42.png)

You can see where is the ECU label that square box indicates that these components are part of ECU, and external enable/disable, is the requesting from the tester.

There are two parts invovled in this particular communication control:

- The communication type, a normal communication or a network management communication.
- There is a **Recieve** and **Transmission**, these two modes are called as communication control that means by enabling/disabling a transmission or reception you can control the communication type, messages which are going out or getting inside the ECU.
  - You can enable a transmission of a particular communication type either a normal communication or a network management communication.
  - Or you can disable a particular communication type with a control type - enable/disable and receive/transmission.

How to understand this diagran? This is what happens when you send the request message to the ECU server. So you can either enable/disable the transmission/receive - control types. Or you can enable/disable the communication type that is normal communication or network management communication specific messages.

Apart from that you can also talk to a node like which we previously disucssed a subnet node like a LIN network if it is connected to this ECU. Then that also can be addressed through an addressing mechanism.

And the last way to control the ECU is you can completely disable the ECU communication itself with the help of communication control.

### 5.3 Subfunction - Communication Control type

| Value       | Control Type                                               |
| ----------- | ---------------------------------------------------------- |
| 0x00        | Enable Rx and Tx                                           |
| 0x01        | Eanble Rx and Disable Tx                                   |
| 0x02        | Disable Rx and Enable Tx                                   |
| 0x03        | Disable Rx and Tx                                          |
| 0x04        | Enable Rx and Disable Tx with Enhanced Address Information |
| 0x05        | Enable Rx and Tx with Enhanced Address Information         |
| 0x40 - 0x5E | Vhicle Manaufacturer Specific                              |

=> Communication Module - [Normal Communication] / [Network Management Communication]

![uds_43](./images/uds_43.png)

### 5.4 Node Identification Number

If you want to access a subnet like a LIN node in the [Figure 3: Hybird Network], where this CAN bus is not connected to this LIN node, then this [Central ECU] acts as a server for these other nodes. Then this node identification number shoul dbe sent as as part of the SID.

![uds_44](./images/uds_44.png)

With the help of this and the other parameter we should be in a position to have the communication control over the specific subnet nodes.

### 5.5 Request & Response Message Format

[SID][Sub-function][Communication type][Node Identification Number]

**Request Message Format**

| Data byte | Parameter Name                       | Byte Value  |
| --------- | ------------------------------------ | ----------- |
| #1        | CommunicationControl Request ID      | 0x28        |
| #2        | sub-function = [controlType]         | 0x00 - 0xFF |
| #3        | communicationType                    | 0x00 - 0xFF |
| #4        | nodeIdentificationNumber (high byte) | 0x00 - 0xFF |
| #5        | nodeIdentificationNumber (low byte)  | 0x00 - 0xFF |

![uds_45](./images/uds_45.png)

**Supported negative response codes (NRC)**

| Date byte | Parameter Name                             |
| --------- | ------------------------------------------ |
| 0x12      | sub-function Not Supported                 |
| 0x13      | Incorrect Message Length Or Invalid Format |
| 0x22      | Condtions Not Correct                      |
| 0x31      | Request Out Of Range                       |

### 5.6 Message flow example Communication Control

![uds_46](./images/uds_46.png)

![uds_47](./images/uds_47.png)

## 6 UDS: ECU Reset Service - SID: 0x11

### 6.1 A simplified ECU - Battery connection

![uds_48](./images/uds_48.png)

![uds_49](./images/uds_49.png)

![uds_50](./images/uds_50.png)

### 6.2 ECU rest sub-functions

| Sl.# | Value       | Sub-function                  |
| ---- | ----------- | ----------------------------- |
| 1    | 0x01        | Hard Reset                    |
| 2    | 0x02        | Key Off-On Reset              |
| 3    | 0x03        | Soft Reset                    |
| 4    | 0x04        | Enable Rapid Power Shut Down  |
| 5    | 0x05        | Disable Rapid Power Shut Down |
| 6    | 0x40 - 0x5F | Vehicle Manufacturer Specific |
| 7    | 0x60 - 0x7E | System Supplier Specific      |

### 6.3 ECUReset Request SID - 0x11

![uds_51](./images/uds_51.png)

![uds_52](./images/uds_52.png)

## 7 UDS: Tester Present Service - SID: 0x3E

### 7.1 Description

This service is used to indicate to a server (or servers) that a client is still connected to the vehicle and that certain diagnostic services and/or communication that have been previously activated are to remain active.

![uds_53](./images/uds_53.png)

In such cases, a periodic tester present message normally sent, this periodic tester present message can be configured to request from the server or the ECU with the responsor without a response.

So the tester request for a routine control and its wait for the till the operaiton is completed by the ECU, during such time, a periodic tester present message will be sent till the time it completes the IO retain or in some cases where the security has to be unlocked in some of diagnostic session, and to maintain during that security unlocked state of that particular session.

And executing some further services in the security unlocked state, and also you can request the test present message.

### 7.2 Request Message

[SID][Sub-function]

| Data byte | Parameter Name                   | Byte Value  |
| --------- | -------------------------------- | ----------- |
| #1        | TesterPresent Request SID        | 0x3E        |
| #2        | sub-function = [zeroSubFunction] | 0x00 / 0x80 |

0x00 zero Sub Function
This parameter value is used to indicate that no sub-function value. If response from server is not required 0x80 value issent from tester.

### 7.3 Request & Response Message

![uds_54](./images/uds_54.png)

### 7.4 Keep Alive Logic - Concurrent Tester Present

It is possible, that functional "TesterPresent" commands are sent by the tester in parallel to physical requests/responses.

Functional TesterPresent (and only functional TesterPresent without response)

![uds_55](./images/uds_55.png)

## 8 UDS: Data Identifiers (DID)

### 8.1 DID parameter definitions

![uds_56](./images/uds_56.png)

### 8.2 An xemaple of DID in scan tool - Predefined

![uds_57](./images/uds_57.png)

![uds_58](./images/uds_58.png)

### 8.3 Dynamic Defined Data Identifier

![uds_59](./images/uds_59.png)

![uds_60](./images/uds_60.png)

### 8.4 Pre-defined DIDs

![uds_61](./images/uds_61.png)

## 9 UDS: Read Data By Identifier Service - SID:0x22

### 9.1 Description

The ead Data By Identifier Service allows the client to request "data record (Data Identifiers - DID)" values from the server.

### 9.2 DID parameter definitions

![uds_56](./images/uds_56.png)

### 9.3 Examples of ODB2 PIDS

![uds_62](./images/uds_62.png)

### 9.4 Request Message

![uds_63](./images/uds_63.png)

### 9.5 Request & Response Message

![uds_64](./images/uds_64.png)

### 9.6 Message flow example Read Data By Identifier

![uds_65](./images/uds_65.png)

![uds_66](./images/uds_66.png)

## 10 UDS: Write Data By Identifier - SID: 0x2E

### 10.1 Description

The Write Data By Identifier service allows the client to write information into the server memory location.

### 10.2 Request Message

![uds_67](./images/uds_67.png)

### 10.3 Positive response message

![uds_68](./images/uds_68.png)

### 10.4 Request & Response Message

![uds_69](./images/uds_69.png)

### 10.5 Message flow example WrietDataByIdentifier

#### 1o.5.1 Example #1: write data identifier 0xF190 (VIN)

![uds_70](./images/uds_70.png)
