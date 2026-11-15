
<img width="1217" height="267" alt="image" src="https://github.com/user-attachments/assets/21339716-acdd-485c-a01f-0329c717d7b3" />

----

## **1. Lab Overview**

**Objective:** Configure RIPv2 between two routers (R1 and R2) to dynamically exchange routing information and enable communication between their Loopback interfaces.

## **2. Lab Instructions – Steps**

1. **Build the Network Topology**

Connect R1 and R2 through their FastEthernet0/0 interfaces.

2. **Configure Router Interfaces**

Configure Loopback1 on R1 with `1.1.1.1/32` and Loopback2 on R2 with `2.2.2.2/32`.

Assign IP addresses to FastEthernet0/0 interfaces`.

3. **Configure RIPv2**

Enable RIP on both routers and configure `version 2`.

4. **Advertise Connected Networks**

On R1 advertise `1.0.0.0` and `10.0.0.0`.

On R2 advertise `2.0.0.0` and `10.0.0.0`.

5. **Verify RIP Routing**

Use `show ip protocols` and `show ip route rip` to verify RIP configuration and dynamically learned routes.

6. **Test End-to-End Connectivity**

Ping `2.2.2.2` from R1 and `1.1.1.1` from R2 to verify bidirectional connectivity.

Is this conversation helpful so far?


------
## **3. Devices Configuration **


##  R1 Configuration

```
! Device: R1

! Mode: Global Configuration
R1# configure terminal
R1(config)# hostname R1

! Mode: Interface Configuration
R1(config)# interface Loopback1
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Mode: Router Configuration
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# network 1.0.0.0
R1(config-router)# network 10.0.0.0
R1(config-router)# no auto-summary
R1(config-router)# end
```

##  R2 Configuration

```
! Device: R2

! Mode: Global Configuration
R2# configure terminal
R2(config)# hostname R2

! Mode: Interface Configuration
R2(config)# interface Loopback2
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# no shutdown
R2(config-if)# exit

R2(config)# interface FastEthernet0/0
R2(config-if)# ip address 10.0.0.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit

! Mode: Router Configuration
R2(config)# router rip
R2(config-router)# version 2
R2(config-router)# network 2.0.0.0
R2(config-router)# network 10.0.0.0
R2(config-router)# no auto-summary
R2(config-router)# end
```

## **4. Checks and Expected Outputs**

**On both routers:**

```
! Mode: Privileged EXEC
R1# show ip protocols

Routing Protocol is "rip"
  Sending updates every 30 seconds
  Default version control: send version 2, receive version 2
  Automatic network summarization is not in effect
```

**Check RIP routes on R1:**

```
R1# show ip route rip

R  2.0.0.0/8 [120/1] via 10.0.0.2, 00:00:xx, FastEthernet0/0
```

**Check RIP routes on R2:**

```
R2# show ip route rip

R  1.0.0.0/8 [120/1] via 10.0.0.1, 00:00:xx, FastEthernet0/0
```

## **5. End-to-End Connectivity Tests**

**From R1:**

```
R1# ping 2.2.2.2
```

Expected: `!!!!!`

**From R2:**

```
R2# ping 1.1.1.1
```

Expected: `!!!!!`

## **6. Troubleshooting**

If the Loopback routes are not appearing:

```
show ip interface brief
show ip protocols
show ip route
show ip rip database
```
