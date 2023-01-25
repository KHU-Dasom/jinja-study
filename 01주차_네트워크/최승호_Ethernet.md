# Ethernet

## IEEE Standards
![Layer](https://user-images.githubusercontent.com/56192918/204189464-3b11d852-ee41-4371-bfb1-050cd77aaba7.png)
- Divide Data Link Layer into LLC and MAC sublayers
- Create several Physical Layer's standards

## LAN & WAN
- A type of communication protocol that connects computers within what's called a ***LAN*** and a ***WAN***
  ![LAN & WAN](https://user-images.githubusercontent.com/56192918/202984677-8ea5b1df-0385-41b8-9669-8e8374ecc9be.png)
<br></br>
  - **LAN(Local Area Network)**: network confined to a small, localized area
    - Connected to the Internet at a central point(Router)
    - Ethernet, WiFi
<br></br>
  - **WAN(Wide Area Network)**: large computer network
    - leased lines, VPNs, IP tunnels


## Standard Ethernet
- Network Interface Layer's protocol
- Transmit data from one Ethernet interface to other Ethernet interface

### MAC Address
![MAC address](https://user-images.githubusercontent.com/56192918/204188871-6d6ec55b-0e5a-4d2d-bbbc-ddda10dc301f.png)
- Necessary for specify Ethernet Interface
- 48-bit address
  - 24-bit: OUI, 24-bit: serial number
- primarily assigned

### Ethernet Frame
- Consists of Data, ***Ethernet Header***, FCS(Frame Check Sequence)
- ***Ethernet Header*** consists of DA, SA, Ether Type
  - DA: Destination MAC Address, SA: Source MAC Address, Ether Type(upper-layer protocol)

### CSMA/CD
- Carrier Sense Multiple Access/Collision Detector(**Listen before Talk**)
1. Station finds if the line is idle
2. If line is not idle, wait
3. If line is idle, send frame
4. In 3), if two or more stations sends data, **collision** occurs
- If collision occurs(called **multiple access**) wait for random time and send again

***Collsion and abortion in CSMA/CD***
![CSMA/CD](https://user-images.githubusercontent.com/56192918/204447883-0b4f7377-00d3-49b4-be6c-13bfac98d7a8.png)


## Bridged Ethernet

### Sharing bandwidth
![Bridge](https://user-images.githubusercontent.com/56192918/204441668-022cfce3-6b2c-4489-8cec-67b570626f27.png)
- The bridge divides the network
  - Bandwidth-wise, each network is independent


![Collision Domain](https://user-images.githubusercontent.com/56192918/204476526-733df6c3-21db-4e39-bb75-7bb18b618ae3.png)
- Separating Collision Domains
  - Probability of collision is reduced

## Switched Ethernet
![N-Port Switch](https://user-images.githubusercontent.com/56192918/204480902-e20d37d7-c3f5-44e7-a932-d2e7e5567c0f.png)
- Bandwidth is shared only between the station and the switch

### Full-Duplex Ethernet
![Full-Duplex Ethernet](https://user-images.githubusercontent.com/56192918/204483161-476cf4cc-4db5-42ea-af96-f90cced3e8c0.png)
- Increases the capacity of each domain
- No Need for CSMA/CD
- MAC Control Layer

## Fast Ethernet
1. Upgrade the data rate to 100 Mbps
2. Make it compatible with Standard Ethernet
3. Keep the same 48-bit address
4. Keep the same frame format
5. Keep the same minimum and maximum frame lengths

- ***Auto Negotiation:*** Allows two devices to negotiate the mode or data rate of operation

## Gigabit Ethernet
1. Upgrade the data rate to 1 Gbps.
2. Make it compatible with Standard or Fast Ethernet.
3. Use the same 48-bit address.
4. Use the same frame format.
5. Keep the same minimum and maximum frame lengths.
6. To support auto negotiation as defined in Fast Ethernet.

![Topology](https://user-images.githubusercontent.com/56192918/204519152-f4bccea0-cc97-40ab-b879-0c2c6a605193.png)