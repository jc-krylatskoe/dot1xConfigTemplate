
# Device configuration

## SGACL configuration
```
cts authorization list CTSLIST
aaa authorization network CTSLIST group CTS-ISE
cts role-based enforcement
cts role-based enforcement vlan-list <VLAN_list_where_to_enforce>

cts logging verbose

radius server <RADIUS-SERVER-NAME>
 pac key [secretkey]

cts credentials id [device-ts-hostname] password [secretpass]
```

Notes:
**pac key [secretkey]** - [secretkey] - it's RADIUS "Shared Secret" configured in NAD settings on ISE
**cts credentials id [device-ts-hostname] password [secretpass]** - [device-ts-hostname] and [secretpass] same as "Device Id" and "Password" in "Device Authentication Settings" in NAD settings on ISE


## SXP configuration
```
cts sxp enable
cts sxp default password [secretpassword]
cts sxp connection peer [ISE_IP] password default mode local listener hold-time 0 0 vrf Mgmt-vrf
cts sxp connection peer [ISE_IP] password default mode local listener hold-time 0 0
cts sxp log binding-changes
```

Notes:
**cts sxp default password [secretpassword]** - same as "default password for SXP" in "SXP Global Settings" on ISE
**cts sxp connection peer [ISE_IP]** - must be configured for each VRF in which need to get IP-SGT bindings

# ISE configuration

## SGACL configuration

### 1. Add TrustSec settings to NAD:

**Work Centers > TrustSec > Components > Network Devices**

![image](https://user-images.githubusercontent.com/60174786/177295010-b8fab291-6b2a-481a-a188-e77f9217fb69.png)

Device ID and Password must be the same as configured on a device:
```
cts credentials id <device-ts-hostname> password <secretpass>
```

### 2. Configure test SGACLs:

**Work Centers > TrustSec > Components > Security Groups ACLs**

- DENY_ANY: deny ip
- PERMIT_ANY: permit ip

![image](https://user-images.githubusercontent.com/60174786/177298262-fb42036c-51ef-4dd0-905c-50c2340bda7e.png)

### 3. Configure TrustSec Matrix:

**Work Centers > TrustSec > TrustSec Policy > Egress Policy > Matrix**
When finished - click "Deploy"

![image](https://user-images.githubusercontent.com/60174786/177298706-174995f5-1371-4caf-b91b-2fb8c1b1e0a8.png)

### 4. Configure Authorization Policy:

**Work Centers > TrustSec > Policy Sets**

Assign Security Groups in Authorization Policy rules:

![image](https://user-images.githubusercontent.com/60174786/177299602-f27ae8d5-e0e3-42d3-990f-47af8f322bd0.png)


## SXP configuration

### 1. Enable SXP on ISE PSNs

**Administration > System > Deployment**

Enable SXP on each PSN where required:
![image](https://user-images.githubusercontent.com/60174786/177302212-2bcf6c17-982e-40c6-aa10-1da13e984c34.png)


### 2. Configure global SXP settings

**Work Centers > TrustSec > Settings > SXP Settings**

- Uncheck "Add radius mappings into SXP IP SGT mapping table" (switches already know about those mappings)
- Set default password for SXP ("Global Password")

This password must be the same as configured on a device:
```
cts sxp default password <secretpassword>
cts sxp connection peer <ISE_IP> password default mode local listener hold-time 0 0
```

![image](https://user-images.githubusercontent.com/60174786/177296918-f3e984d5-143d-49e0-a332-03987a7be009.png)

### 3. Configure SXP connections to switches
**Work Centers > TrustSec > SXP > SXP Devices**
Add SXP listeners
![image](https://user-images.githubusercontent.com/60174786/177301896-3ce7fa39-8675-4e2f-bb4a-5308fdebd05e.png)

### 4. Add test IP-SGT binding

**Work Centers > TrustSec > Components > IP SGT Static Mapping**

![image](https://user-images.githubusercontent.com/60174786/177297828-9b20c6da-4981-4879-818b-246e0a912e58.png)
