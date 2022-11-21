# Ethernet

## Backgrounds

### LLC & MAC(Sublayer of Data Link Layer)

![LLC&MAC](https://user-images.githubusercontent.com/56192918/202971527-79ff9f02-b59b-4d86-96a9-3df6bc054015.png)
- **LLC(Logical Link Control)**
  - Flow control
  - Error control
  - Part of framing(Framing is handled in both LLC sublayer and MAC sublayer)
  - One single data link control protocol

    
- **MAC(Media Access Control)**
  - different protocols for different LANs
    - CSMA/CD for Ethernet LANs
    - token-passing method for Token Ring and Token Bus LANs
  - Part of framing

![Frames](https://user-images.githubusercontent.com/56192918/202971862-c520d4e8-db87-4881-af85-6a250a38ce26.png)
- **Frames in LLC / MAC sublayer** 
  - MAC frame consists of MAC header, LLC PDU, FCS
    - LLC PDU consists of DSAP(Destination service access point 목적지), SSAP(Source service access point MAC 프레임 생성지), Control(Error / Flow control), upper-layer data)

## Ethernet
- A system for connecting a number of computer systems to form a LAN / WAN with protocols


- Features
  - Unreliable: NO ACK / NAK
  - Connectionless: No handshaking


- Four generations
  - Standard Ethernet(10Mbps), Fast Ethernet(100Mbps), Gigabit Ethernet(1Gbps), Ten-Gigabit Ethernet(10Gbps)
### Standard Ethernet
- **MAC sublayer**
  - governs operation of the access method
  - frames data received from the upper layer and passes them to the physical layer


- **Frame Format**
  - **ppt7 사진넣기**
  - contains seven fields
    - Preamble: 7-byte, alternating 0s and 1s, synchronize between sender and receiver(10101010)
      
    - SFD(Start Frame Delimiter): 1-byte, only in 802.3, signals the beginning of the frame, warns that this is the last chance of the frame, last 2 bits is 11(10101011) 
    - DA(Destination Address): 6-byte, physical address of the sender
    - SA(Source Address): 6-byte, physical address of the sender
    - Length / Type: 2-byte
      - less than 0x05dc: Translate as Type(DIX 2.0, upper-layer protocol using the MAC frame)
      - more than 0x0600: Translate as Length(IEEE 802.3, number of bytes in the data field)
        - **DIX2.0은 프로토콜이 여러개라 타입 표시, IEEE802.3은 표준화 한 것이라 길이를 나타냄**
    - Data: 46~1500-byte, data encapsulated from upper-layer protocols
    - CRC(Cyclic Redundancy Check): Error Detection
  
    
- **MAC Addressing**
  - Each station on an Ethernet network(PC, workstation or printer) has its own NIC(Network Interface Card)
  - NIC provides the station with 6-byte physical address
  - Cast
    - Unicast: 1:1
    - Broadcast: 1:ALL
    - Multicast: 1:MANY(not all)


- **CSMA(Carrier Sense Multiple Access)**
  - Developed to minimize _**the chance of collision**_ and increase the performance
    - Can be _**reduced**_ if a station senses the medium before trying to use(**"Listen Before Talk"**)
      - Can be reduced, not be eliminated becuase of propagation delay


  - **Persistent Methods**
    - 1-Persistent method
      - Station finds the line idle, it sends its frame immediately(with probability 1)
      - Advantage: simple, straightforward
      - Disadvantage: highest chance of collision(two or more stations may find the line idle and send their frames immediately)
<br></br>
    - Non-Persistent method
      - Station that has a frame to send senses the line
        1. if the line is idle, it sends immediately
        2. if the line is not idle, it waits a random amount of time and then senses the line again
      - Advantage: reduced chance of collision(it is unlikely that two or more stations will wait the same amount of time)
      - Disadvantage: reduced efficiency of network(medium remains idle when there may be stations with frames to send)
<br></br>
    - P-Persistent method
      - is used if channel has time slots with a slot duration equal to or greater than the maximum propagation time
      - combines the advantages of the other two methods
      - if station finds the line idle
        1. With probability p, the station sends its frame
        2. With probability q(1-p), the station waits for the beginning of the next time slot and checks the line again
          1) if the line is idle, it goes to step 1
          2) if the line is busy, it acts as though a collision has occurred and uses the back-off procedure