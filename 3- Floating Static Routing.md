<img width="772" height="161" alt="image" src="https://github.com/user-attachments/assets/de29f5a5-2115-48cf-b180-76d7bf18671a" />

---------

## **Lab: Floating Static Routing**

### **1. Lab Objective**

Configure R1 to reach the `2.2.2.2/32` network through its `FastEthernet1/0` interface using a Static Route.

If the primary interface `FastEthernet1/0` goes down, R1 should automatically use `FastEthernet0/0` as a backup path through a Floating Static Route.

Once the primary interface becomes available again, R1 should automatically switch back to the primary route.

### **2. Lab Steps**

1. Build the network topology using two routers (R1 and R2).

2. Configure IP addresses on the physical interfaces of both routers.

3. Configure Loopback1 on R1 with IP address `1.1.1.1/32`.

4. Configure Loopback2 on R2 with IP address `2.2.2.2/32`.

5. Configure a Primary Static Route on R1 using `FastEthernet1/0` with Administrative Distance `1`.

6. Configure a Floating Static Route on R1 using `FastEthernet0/0` with a higher Administrative Distance.

7. Configure the corresponding Primary and Floating Static Routes on R2 to ensure return connectivity.

8. Verify the active routes using `show ip route`.

9. Test connectivity between the two Loopback interfaces using `ping`.

10. Simulate a primary link failure by shutting down `FastEthernet1/0`.

11. Verify that the Floating Static Route becomes active automatically.

12. Restore the primary interface using `no shutdown`.

13. Verify that R1 switches back to the Primary Static Route automatically.

14. Save the configurations using `write memory`.

### **3. Expected Results**

- Initially traffic uses the Primary Static Route.
    
- When the primary link fails the Floating Static Route becomes active.
    
- Connectivity remains available through the backup link.
    
- When the primary link is restored the router returns to the Primary Static Route.
    
- End-to-end connectivity remains successful throughout the failover test.

----
## **4. Devices Configuration**

## **1. R1 Configuration**

```
! Device: R1

! Mode: Global Configuration
R1(config)# hostname R1

! Mode: Interface Configuration
R1(config)# interface FastEthernet0/0
               description PRIMARY_LINK_TO_R2
               ip address 10.0.0.1 255.255.255.252
               no shutdown
            exit

R1(config)# interface FastEthernet1/0
               description BACKUP_LINK_TO_R2
               ip address 11.0.0.1 255.255.255.252
               no shutdown
            exit

R1(config)# interface Loopback1
               ip address 1.1.1.1 255.255.255.255
            exit

! Mode: Global Configuration
R1(config)# ip route 2.2.2.2 255.255.255.255 FastEthernet0/0
R1(config)# ip route 2.2.2.2 255.255.255.255 FastEthernet1/0 200

! Mode: Privileged EXEC
R1# write memory
```

## **2. R2 Configuration**

```
! Device: R2

! Mode: Global Configuration
R2(config)# hostname R2

! Mode: Interface Configuration
R2(config)# interface FastEthernet0/0
               description PRIMARY_LINK_TO_R1
               ip address 10.0.0.2 255.255.255.252
               no shutdown
            exit

R2(config)# interface FastEthernet1/0
               description BACKUP_LINK_TO_R1
               ip address 11.0.0.2 255.255.255.252
               no shutdown
            exit

R2(config)# interface Loopback2
               ip address 2.2.2.2 255.255.255.255
            exit

! Mode: Global Configuration
R2(config)# ip route 1.1.1.1 255.255.255.255 FastEthernet0/0
R2(config)# ip route 1.1.1.1 255.255.255.255 FastEthernet1/0 250

! Mode: Privileged EXEC
R2# write memory
```

## **3. Verification Commands**

### **R1**

```
! Mode: Privileged EXEC
R1# show ip interface brief
R1# show ip route
R1# ping 2.2.2.2
```

### **R2**

```
! Mode: Privileged EXEC
R2# show ip interface brief
R2# show ip route
R2# ping 1.1.1.1
```


## **4. Floating Static Routing Test**

**Before failure – Primary Route**

R1:

```
S  2.2.2.2/32 [1/0] is directly connected, FastEthernet0/0
```

R2:

```
S  1.1.1.1/32 [1/0] is directly connected, FastEthernet0/0
```



- Test

R1:

```
! Mode: Interface Configuration
R1(config)# interface FastEthernet0/0
R1(config-if)# shutdown
```

R2:

```
! Mode: Interface Configuration
R2(config)# interface FastEthernet0/0
R2(config-if)# shutdown
```

**After failure – Backup Route**

R1:

```
S  2.2.2.2/32 [200/0] is directly connected, FastEthernet1/0
```

R2:

```
S  1.1.1.1/32 [250/0] is directly connected, FastEthernet1/0
```

- Verification

```
! Mode: Privileged EXEC
R1# ping 2.2.2.2
R2# ping 1.1.1.1
```
