

<img width="785" height="178" alt="image" src="https://github.com/user-attachments/assets/a41afee8-45cb-4c4d-b48f-7edf8cd9077c" />

-------
1. Lab Overview**

**Objective:** Configure Static Routing between two routers (R1 and R2) to allow communication between two different LAN networks.


## **2. Lab Instructions – Steps**

1. **Build the Network Topology**
    
    Connect PC1 to R1 and PC2 to R2. Then connect R1 to R2.
    
2. **Configure Router Interfaces**
    
    Assign IP addresses to all router interfaces and enable them using `no shutdown`.
    
3. **Configure PC IP Addresses**
    
    Configure IP address, Subnet Mask and Default Gateway on both PCs.
    
4. **Configure Static Routes**
    
    On R1 add a route to `12.0.0.0/24` through `11.0.0.2`.
    
    On R2 add a route to `10.0.0.0/24` through `11.0.0.1`.
    
5. **Verify Routing Table**
    
    Use `show ip route static` to verify that the Static Routes have been installed.
    
6. **Test End-to-End Connectivity**
    
    Ping PC2 from PC1 and ping PC1 from PC2 to verify bidirectional communication.


## **3. Devices Configuration **
## **1. R1 Configuration**

R1

```
! Device: R1

! Mode: Global Configuration
R1(config)# hostname R1

! Mode: Interface Configuration
R1(config)# interface FastEthernet0/0
R1(config-if)# description LAN_TO_PC1
               ip address 10.0.0.1 255.255.255.0
               no shutdown
               exit

! Mode: Interface Configuration
R1(config)# interface FastEthernet1/0
R1(config-if)# description LINK_TO_R2
               ip address 11.0.0.1 255.255.255.0
               no shutdown
               exit

! Mode: Global Configuration
R1(config)# ip route 12.0.0.0 255.255.255.0 11.0.0.2
            end
```


## **2. R2 Configuration**

R2

```
! Device: R2

! Mode: Global Configuration
R2(config)# hostname R2

! Mode: Interface Configuration
R2(config)# interface FastEthernet0/0
R2(config-if)# description LINK_TO_R1
               ip address 11.0.0.2 255.255.255.0
               no shutdown
               exit

! Mode: Interface Configuration
R2(config)# interface FastEthernet1/0
R2(config-if)# description LAN_TO_PC2
               ip address 12.0.0.1 255.255.255.0
               no shutdown
               exit

! Mode: Global Configuration
R2(config)# ip route 10.0.0.0 255.255.255.0 11.0.0.1
            end
```

## **3. PC Configurations**

**PC1**

```
PC1> ip 10.0.0.100 255.255.255.0 10.0.0.1
```

**PC2**

```
PC2> ip 12.0.0.100 255.255.255.0 12.0.0.1
```

## **4. Checks and Expected Outputs**

**R1 – Privileged EXEC Mode**

```
R1# show ip route static

S    12.0.0.0/24 [1/0] via 11.0.0.2
```

**R2 – Privileged EXEC Mode**

```
R2# show ip route static

S    10.0.0.0/24 [1/0] via 11.0.0.1
```

**PC1 – Connectivity Test**

```
PC1> ping 12.0.0.100

84 bytes from 12.0.0.100
84 bytes from 12.0.0.100
84 bytes from 12.0.0.100
84 bytes from 12.0.0.100
84 bytes from 12.0.0.100
```

**PC2 – Connectivity Test**

```
PC2> ping 10.0.0.100

84 bytes from 10.0.0.100
84 bytes from 10.0.0.100
84 bytes from 10.0.0.100
84 bytes from 10.0.0.100
84 bytes from 10.0.0.100
```



