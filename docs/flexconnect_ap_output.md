# Sample command outputs
## Index
[Configuration for connection of Flexconnect Access Points](#configuration-for-connection-of-Flexconnect-Access-Points)<br>
[show access-session outputs](#show-access-session-outputs)<br><br>
- [When ISE server is not reachable](#when-ise-server-is-not-reachable)<br>
- [When ISE server becomes reachable](#when-ise-server-becomes-reachable)<br>
- [When device-tracking disabled for wireless client's VLAN](#when-device-tracking-disabled-for-wireless-clients-vlan)<br>

## Configuration for connection of Flexconnect Access Points
Details about solution for Flexconnect Access Points:<br>
[docs/flexconnect_ap_configuration.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/flexconnect_ap_configuration.md)

## show access-session outputs
Examples of **show access-session** command outputs on interface,
where flexconnect AP is connected (IP = 3.45.248.102) and one wireless client via AP (IP = 198.18.3.16).

### When ISE server is not reachable
```
c3850-lab#show access-session int gig1/0/3 details
            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x158F06D4
          MAC Address:  0c8b.fdf4.d55d
         IPv6 Address:  Unknown
         IPv4 Address:  198.18.3.16
          Device-type:  Intel-Device
          Device-name:  INTEL CORPORATE
               Status:  Authorized
               Domain:  UNKNOWN
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 171667s
    Common Session ID:  032DF9FE0000A4C7BE55C6A7
      Acct Session ID:  0x0000229e
               Handle:  0x6e0004eb
       Current Policy:  DOT1X-DEFAULT


Local Policies:
        Service Template: CRITICAL_TEMPLATE (priority 150)
       Filter-ID: ACL-ISE-PERMIT-ANY

Server Policies:


Method status list:
       Method           State
        dot1x           Stopped
          mab           Authc Failed

----------------------------------------

            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x17ADE99B
          MAC Address:  2cf8.9bb6.b770
         IPv6 Address:  Unknown
         IPv4 Address:  3.45.248.102
          Device-type:  Cisco-AIR-LAP
          Device-name:  cisco AIR-AP1815W-B-K9
               Status:  Authorized
               Domain:  UNKNOWN
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 167322s
    Common Session ID:  032DF9FE0000A427BE1379AE
      Acct Session ID:  0x000021ff
               Handle:  0x1700044b
       Current Policy:  DOT1X-DEFAULT


Local Policies:
        Service Template: SERVICE_TEMPLATE_FLEXAP_TRUNK (priority 150)
   Interface Template:  FLEXAP_TRUNK
       Filter-ID: ACL-ISE-PERMIT-ANY

Server Policies:


Method status list:
       Method           State
        dot1x           Stopped
          mab           Authc Failed
```

### When ISE server becomes reachable
```
c3850-lab#show access-session int gig1/0/3 details
            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x11201CF3
          MAC Address:  0c8b.fdf4.d55d
         IPv6 Address:  Unknown
         IPv4 Address:  198.18.3.16
            User-Name:  0C-8B-FD-F4-D5-5D
          Device-type:  Intel-Device
          Device-name:  INTEL CORPORATE
               Status:  Authorized
               Domain:  DATA
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 172776s
    Common Session ID:  032DF9FE0000A4C9BE69DF90
      Acct Session ID:  0x000022a0
               Handle:  0xaf0004ed
       Current Policy:  DOT1X-DEFAULT


Server Policies:


Method status list:
       Method           State
        dot1x           Running
          mab           Authc Success

----------------------------------------

            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x13346ABC
          MAC Address:  2cf8.9bb6.b770
         IPv6 Address:  Unknown
         IPv4 Address:  3.45.248.102
            User-Name:  2C-F8-9B-B6-B7-70
          Device-type:  Cisco-AIR-LAP
          Device-name:  cisco AIR-AP1815W-B-K9
               Status:  Authorized
               Domain:  DATA
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 172759s
    Common Session ID:  032DF9FE0000A4C8BE699DA1
      Acct Session ID:  0x0000229f
               Handle:  0xc10004ec
       Current Policy:  DOT1X-DEFAULT


Server Policies:
            SGT Value:  4
        Service Template: Access_Point (priority 100)
   Interface Template:  ISE_FLEXAP_TRUNK
              ACS ACL: xACSACLx-IP-PERMIT_ALL_IPV4_TRAFFIC-57f6b0d3


Method status list:
       Method           State
        dot1x           Stopped
          mab           Authc Success
```

### When device-tracking disabled for wireless client's VLAN
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

Wireless client 0c8b.fdf4.d55d connected via AP in VLAN 3:

```
c3850-lab#show access-session interface gig1/0/3 details
            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x1E8427B1
          MAC Address:  0c8b.fdf4.d55d
         IPv6 Address:  Unknown
         IPv4 Address:  Unknown
            User-Name:  0C-8B-FD-F4-D5-5D
          Device-type:  Intel-Device
          Device-name:  INTEL CORPORATE
               Status:  Authorized
               Domain:  DATA
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 172779s
    Common Session ID:  032DF9FE0000050BD3C38F68
      Acct Session ID:  0x000004f2
               Handle:  0x54000501
       Current Policy:  DOT1X-DEFAULT


Server Policies:
              ACS ACL: xACSACLx-IP-PERMIT_ANY-62c4c5f9


Method status list:
       Method           State
        dot1x           Running
          mab           Authc Success

----------------------------------------

            Interface:  GigabitEthernet1/0/3
               IIF-ID:  0x1B89F7F7
          MAC Address:  2cf8.9bb6.b770
         IPv6 Address:  Unknown
         IPv4 Address:  3.45.248.102
            User-Name:  2C-F8-9B-B6-B7-70
          Device-type:  Cisco-AIR-LAP
          Device-name:  cisco AIR-AP1815W-B-K9
               Status:  Authorized
               Domain:  DATA
       Oper host mode:  multi-auth
     Oper control dir:  both
      Session timeout:  N/A
  Acct update timeout:  172800s (local), Remaining: 170014s
    Common Session ID:  032DF9FE00000505D3995D6E
      Acct Session ID:  0x000004ec
               Handle:  0xf90004fb
       Current Policy:  DOT1X-DEFAULT


Server Policies:
   Interface Template:  FLEXAP_TRUNK
              ACS ACL: xACSACLx-IP-PERMIT_ANY-62c4c5f9


Method status list:
       Method           State
        dot1x           Stopped
          mab           Authc Success
```

No IP addresses learned by device-tracking in VLAN 3:
```
c3850-lab#show device-tracking database interface gig1/0/3 vlanid 3
portDB has 1 entries for interface Gi1/0/3, 1 dynamic
Codes: L - Local, S - Static, ND - Neighbor Discovery, ARP - Address Resolution Protocol, DH4 - IPv4 DHCP, DH6 - IPv6 DHCP, PKT - Other Packet, API - API created
Preflevel flags (prlvl):
0001:MAC and LLA match     0002:Orig trunk            0004:Orig access
0008:Orig trusted trunk    0010:Orig trusted access   0020:DHCP assigned
0040:Cga authenticated     0080:Cert authenticated    0100:Statically assigned


    Network Layer Address               Link Layer Address Interface        vlan prlvl  age   state     Time left
```
