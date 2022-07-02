# show access-session outputs
Examples of **show access-session** command outputs on interface,
where flexconnect AP is connected (IP = 3.45.248.102) and one wireless client via AP (IP = 198.18.3.16):

## When ISE server is not reachable
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

## When ISE server becomes reachable
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