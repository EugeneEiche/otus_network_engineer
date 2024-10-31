#Lesson_1(VLAN and inter Vlan Routing)
1) Base configuration router and switch
Enter configuration mode.
conf t
Assign a device name to the router
hostname R1 (router), S1-2 (switch)
Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
no ip domain-lookup
Assign class as the privileged EXEC encrypted password.
enable secret class
Assign cisco as the console password and enable login.
line con 0
password cisco
login
Assign cisco as the VTY password and enable login.
kine vty 0 4
password cisco
login
Encrypt the plaintext passwords.
service password-encripyion
Create a banner that warns anyone accessing the device that unauthorized access is prohibited.
banner modt #
Unauthorized access is strictly prohibited and prosecuted to the full extent of the law.#
Save the running configuration to the startup configuration file.
copy run start, or write (wr)
Configure PC hosts
2) Create VLANs and Assign Switch Ports
2.1) Switch S1
conf t
Create and name the required VLANs on each switch from the table above
vlan 3
name Managment
vlan 4
name Operations
vlan 7
name ParkingLot
vlan 8
name Native
int vlan 3
Configure the management interface and default gateway on each switch using the IP address information in the Addressing Table.
ip address 192.168.3.11 255.255.255.0
ip default gateway 192.168.3.1
Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode, and administratively deactivate them
int range F0/2-4
switchport access vlan 7
sh
int range F0/7-24
switchport access vlan 7
sh
int range G0/1-2
switchport access vlan 7
sh
int F0/5
switchport mode trunk
switchport trunk native vlan 8
switchport trunk allowed vlan 3-4,8
int F0/1
switchport mode trunk
switchport trunk native vlan 8
switchport trunk allowed vlan 3-4,8
int F0/6
switchport mode access
switchport access vlan 3
2.2) Switch S2
conf t
Create and name the required VLANs on each switch from the table above
vlan 3
name Managment
vlan 4
name Operations
vlan 7
name ParkingLot
vlan 8
name Native
int vlan 3
Configure the management interface and default gateway on each switch using the IP address information in the Addressing Table.
ip address 192.168.3.12 255.255.255.0
ip default gateway 192.168.3.1
Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode, and administratively deactivate them
int range F0/2-17
switchport access vlan 7
sh
int range F0/19-24
switchport access vlan 7
sh
int range G0/1-2
switchport access vlan 7
sh
int F0/1
switchport mode trunk
switchport trunk native vlan 8
switchport trunk allowed vlan 3-4,8
int F0/18
switchport mode access
switchport access vlan 4
Configure Inter-VLAN Routing on the Router
Activate interface G0/0/1 on the router
conf t
int Gi0/0/1
no sh
int G0/0/1.3
encapsulation dot1Q 3
ip address 192.168.3.1 255.255.255.0
int G0/0/1.4
encapsulation dot1Q 4
ip address 192.168.4.1 255.255.255.0
int G0/0/1.8
encapsulation dot1Q 8
Use the show ip interface brief command to verify the sub-interfaces are operational
R1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.3 192.168.3.1     YES manual up                    up 
GigabitEthernet0/0/1.4 192.168.4.1     YES manual up                    up 
GigabitEthernet0/0/1.8 unassigned      YES unset  up                    up 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
Complete the following tests from PC-A. All should be successful
Ping from PC-A to its default gateway
C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

Ping from PC-A to PC-B
C:\>ping 192.168.4.3

Pinging 192.168.4.3 with 32 bytes of data:

Request timed out.
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.4.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

Ping from PC-A to S2
C:\>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:

Request timed out.
Reply from 192.168.3.12: bytes=32 time=3ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 3ms, Average = 1ms

Complete the following test from PC-B
C:\>tracert 192.168.3.1

Tracing route to 192.168.3.1 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.3.1

Trace complete.