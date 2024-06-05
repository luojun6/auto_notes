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

![uds_2](./images/uds_2.png)

The CAN communication lines are extended to OBD2 port via diagnostic CAN.

The ECU can be connected in different. You know topologies in the vehicle. But every individual ECU can be accssed by the tester from this diagnostic port.

As we discussed previously, every ECU insdie the vehicle as assigned unique identifier their address.
