# RADIUS tracing on IOS-XE
Features like Radius run both on IOSd Process and on R0 Location (for example, smd process).<br>
Standard debug procedure (**debug radius**) will not show radius debugs for authentication/dot1x.


## Check what traces enabled:
```
show platform software trace level smd switch active R0
```
Default Trace level for modules is "Notice".<br>
Tracing always on.<br>

## Clear trace logging:
```
request platform software trace rotate all
```

## Enable debugs for dot1x / radius / policy type subscriber application:
```
set platform software trace smd switch 1 R0 dot1x-all debug
set platform software trace smd switch 1 R0 radius debug
set platform software trace smd switch 1 R0 epm debug
```

## Display traces:
### New way (starting from IOS-XE 16.11):
```
show logging process smd internal
```

### Legacy way (before IOS-XE 16.11):
```
show platform software trace message smd switch 1 R0
```

### Where traces stored:
Tracelog files are stored in crashinfo tracelogs/ in binary:
```
show crashinfo: all | i smd
```

Check filesystem where 'crashinfo' mounted:<br>
**show crashinfo: filesys**
```
Filesystem: crashinfo
Filesystem Path: /mnt/sd1
Filesystem Type: ext2
Mounted: Read/Write
```

## Disable debugs for dot1x / radius / policy type subscriber application:
```
set platform software trace smd switch 1 R0 dot1x-all notice
set platform software trace smd switch 1 R0 radius notice
set platform software trace smd switch 1 R0 epm notice
```
