# Configuration for connection of Flexconnect Access Points

## Logic of the solution:
Don't use Autoconf since it's not required:
1. If ISE is available:<br>
Service-template applied from ISE (it must contain "Interface Template" and "DACL" (the latter is needed to overwrite the default ACL on the interface (ip access-group)
2. If the ISE becomes unavailable:<br>
Switch executes the Critical service-template configured in the policy-map type subcriber (service-template applied contains interface-template and access-group).

## Congiration on ISE

### 1. Configure Authorization Profiles
The following Authorization Profiles to be created:
- "Access Point" - applied after profiling an Access Point on ISE
- "WiredMAB_Wireless_Clients"
**Policy > Policy Elements > Results > Authorization Profiles**:<br>
### Authorization profile "Access Point":
![image](https://user-images.githubusercontent.com/60174786/177613838-e69e3a11-f58f-4644-9542-50a1e70ba228.png)
### Authorization profile "WiredMAB_Wireless_Clients":
![image](https://user-images.githubusercontent.com/60174786/177616347-c108e0e4-4ce9-450b-992f-3db4e4a80295.png)

### 2. Configure Authentication Policy
Since MAB is used for 2nd authentication (on a switchport), similar to CWA case, the following must be configured in "Authentication Policy":<br>
**if User not found - CONTINUE**
![image](https://user-images.githubusercontent.com/60174786/177613298-9554ea3f-5e05-4724-9975-29f7385a8b21.png)

### 3. Create Authorization Policy for Access Points
Create Authorization Policy to apply Authorization Profile "Access Point":
![image](https://user-images.githubusercontent.com/60174786/177614108-9d81b872-3131-402a-82fe-242d402b6311.png)

### 4. Create Authorization Policy for Wireless clients
Create Authorization Policy to apply Authorization Profile "Permit Access" for wireless clients connecting via switchport (authenticated using MAB):
![image](https://user-images.githubusercontent.com/60174786/177615219-1e1a4bb2-cdbb-4292-89c4-9e7a3c212ec3.png)

## Congiration on switch
### 1. Configure interface-template
To be applied when FlexConnect AP identified on a switchport:
```
template FLEXAP_TRUNK
 spanning-tree portfast trunk
 switchport trunk native vlan [native-vlan-id]
 switchport mode trunk
 access-session interface-template sticky timer 30
```

### 2. Configure service-template
Applies FlexConnect AP interface-template and access-list:
```
ip access-list extended ACL-ISE-PERMIT-ANY
 10 permit ip any any

service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
 access-group ACL-ISE-PERMIT-ANY
 interface-template FLEXAP_TRUNK
```

### 3. Configure additional class-maps
```
class-map type control subscriber match-all AAA-DOWN-UNAUTH-FLEXAP-TIMEOUT-AP
 match result-type aaa-timeout
 match device-type "Cisco-AIR-AP"
class-map type control subscriber match-all AAA-DOWN-UNAUTH-FLEXAP-TIMEOUT-LAP
 match result-type aaa-timeout
 match device-type "Cisco-AIR-LAP"
class-map type control subscriber match-all AAA-DOWN-UNAUTH-FLEXAP-UNAUTH-AP
 match authorization-status unauthorized
 match device-type "Cisco-AIR-AP"
class-map type control subscriber match-all AAA-DOWN-UNAUTH-FLEXAP-UNAUTH-LAP
 match authorization-status unauthorized
 match device-type "Cisco-AIR-LAP"
```

### 4. Configure additional actions for event
```
policy-map type control subscriber DOT1X-DEFAULT
 event authentication-failure match-first
  11 class AAA-DOWN-UNAUTH-FLEXAP-TIMEOUT-AP do-until-failure
   10 activate service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
   30 authorize
   40 pause reauthentication
  12 class AAA-DOWN-UNAUTH-FLEXAP-TIMEOUT-LAP do-until-failure
   10 activate service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
   30 authorize
   40 pause reauthentication
  13 class AAA-DOWN-UNAUTH-FLEXAP-UNAUTH-AP do-until-failure
   10 activate service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
   30 authorize
   40 pause reauthentication
  14 class AAA-DOWN-UNAUTH-FLEXAP-UNAUTH-LAP do-until-failure
   10 activate service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
   30 authorize
   40 pause reauthentication
```

### 5. (Optional) Disable device-tracking for wireless client's VLAN
Configuration to disable device-tracking for wireless client's VLAN (VLAN 3):
```
device-tracking policy device-tracking-wireless-clients-policy
 trusted-port
 device-role switch
 no protocol udp
 tracking disable

vlan configuration 3
 device-tracking attach-policy device-tracking-wireless-clients-policy
```

## Check RADIUS live logs on ISE
There would be 2 sessions in RADIUS Live logs on ISE for each wireless client:
- 1st authentication - on AP via dot1x / MAB (might be after 2nd authentication)
- 2nd authentiction - on switchport via MAB

![image](https://user-images.githubusercontent.com/60174786/177612944-25a5c3a8-ec9d-4ff9-9a83-9b3be5ef9bac.png)

## Check correct config applied on a switchport with AP

**show derived-config interface [interface-name]**<br>

Example:
```
show derived-config int gig1/0/3

interface GigabitEthernet1/0/3
 description AP
 switchport access vlan 3
 switchport trunk native vlan 248
 switchport mode trunk
 switchport nonegotiate
 switchport voice vlan 198
 ip access-group ACL-ISE-DEFAULT in
 authentication periodic
 authentication timer reauthenticate server
 access-session port-control auto
 access-session interface-template sticky timer 30
 mab
 dot1x pae authenticator
 dot1x timeout tx-period 7
 dot1x max-req 3
 dot1x max-reauth-req 3
 spanning-tree portfast trunk
 service-policy type control subscriber DOT1X-DEFAULT

```

```
show run int gig1/0/3
interface GigabitEthernet1/0/3
 description AP
 switchport access vlan 3
 switchport voice vlan 198
 ip access-group ACL-ISE-DEFAULT in
 source template 1X_MAB
 spanning-tree portfast trunk
```

