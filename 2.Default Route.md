
<img width="762" height="202" alt="image" src="https://github.com/user-attachments/assets/7209a081-6429-4a41-9466-126f3a4782a0" />


-----

##   Lab: Static Routing – Default Route

### **1. Lab Objectives**

- Understand the concept of Default Static Routing.
    
- Configure IPv4 addresses on router interfaces.
    
- Configure Loopback interfaces.
    
- Configure a Default Static Route.
    
- Verify end-to-end connectivity between different networks.
    
- Troubleshoot connectivity issues using `ping` and `show ip route`.
    

### **2. Lab Steps**

1. Build the network topology using two routers (R1 and R2).

2. Configure IP addresses on all physical interfaces.

3. Configure two Loopback interfaces on R2 to simulate remote networks.

4. Enable the physical interfaces using `no shutdown`.

5. Configure a Default Static Route on R1 pointing to R2 (`11.0.0.2`).

6. Configure a Specific Static Route on R2 pointing to R1 (`10.0.0.1`).

7. Verify interface status using `show ip interface brief`.

8. Verify routing tables using `show ip route`.

9. Test connectivity between R1 and R2 using `ping`.

10. Test connectivity from R1 to both Loopback interfaces on R2.

11. Save the configurations using `write memory`.

### **3. Expected Results**

- All physical interfaces should be `up/up`.
    
- R1 should have a Default Route (`S*`).
    
- R2 should have a Specific Static Route (`S`).
    
- R1 should successfully reach all remote networks.
    
- All end-to-end ping tests should succeed.


### **4. Devices Configuration**

## **1. R1 Configuration**

```
! Device: R1

! Mode: Global Configuration
R1(config)# hostname R1

! Mode: Interface Configuration
R1(config)# interface FastEthernet0/0
               description LAN
               ip address 10.0.0.1 255.255.255.0
               no shutdown
            exit

R1(config)# interface FastEthernet1/0
               description TO_R2
               ip address 11.0.0.1 255.255.255.0
               no shutdown
            exit

! Mode: Global Configuration
R1(config)# ip route 0.0.0.0 0.0.0.0 11.0.0.2

! Mode: Privileged EXEC
R1# write memory
```


- Routing Table R1

```
S*  0.0.0.0/0 [1/0] via 11.0.0.2
C   10.0.0.0/24 is directly connected, FastEthernet0/0
L   10.0.0.1/32 is directly connected, FastEthernet0/0
C   11.0.0.0/24 is directly connected, FastEthernet1/0
L   11.0.0.1/32 is directly connected, FastEthernet1/0
```

## **2. R2 Configuration**

```
! Device: R2

! Mode: Global Configuration
R2(config)# hostname R2

! Mode: Interface Configuration
R2(config)# interface FastEthernet0/0
               description TO_R1
               ip address 11.0.0.2 255.255.255.0
               no shutdown
            exit

R2(config)# interface FastEthernet1/0
               description LAN
               ip address 12.0.0.1 255.255.255.0
               no shutdown
            exit

R2(config)# interface Loopback1
               ip address 13.0.0.2 255.255.255.0
            exit

R2(config)# interface Loopback2
               ip address 14.0.0.2 255.255.255.0
            exit

! Mode: Global Configuration
R2(config)# ip route 10.0.0.0 255.255.255.0 11.0.0.1

! Mode: Privileged EXEC
R2# write memory
```


- Routing table R2
```
S   10.0.0.0/24 [1/0] via 11.0.0.1
C   11.0.0.0/24 is directly connected, FastEthernet0/0
C   12.0.0.0/24 is directly connected, FastEthernet1/0
C   13.0.0.0/24 is directly connected, Loopback1
C   14.0.0.0/24 is directly connected, Loopback2
```


## **3. Verification**

**R1 and R2**

```
! Mode: Privileged EXEC
R1# show ip interface brief
R1# show ip route

R2# show ip interface brief
R2# show ip route
```

**Connectivity Test**

```
! Mode: Privileged EXEC
R1# ping 11.0.0.2
R1# ping 12.0.0.1
R1# ping 13.0.0.2
R1# ping 14.0.0.2
```
