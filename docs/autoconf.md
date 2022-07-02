# Autoconf configuration
## Autoconf non-default configuration:

```
autoconf enable

parameter-map type subscriber attribute-to-service BUILTIN_DEVICE_TO_TEMPLATE
 60 map device-type regex "Cisco-AIR-AP"
  20 service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
 70 map device-type regex "Cisco-AIR-LAP"
  20 service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
```

## Autoconf resulting parameter-map configuration:
```
parameter-map type subscriber attribute-to-service BUILTIN_DEVICE_TO_TEMPLATE
 10 map device-type regex "Cisco-IP-Phone"
  20 interface-template IP_PHONE_INTERFACE_TEMPLATE
 20 map device-type regex "Cisco-IP-Camera"
  20 interface-template IP_CAMERA_INTERFACE_TEMPLATE
 30 map device-type regex "Cisco-DMP"
  20 interface-template DMP_INTERFACE_TEMPLATE
 40 map oui eq "00.0f.44"
  20 interface-template DMP_INTERFACE_TEMPLATE
 50 map oui eq "00.23.ac"
  20 interface-template DMP_INTERFACE_TEMPLATE
 60 map device-type regex "Cisco-AIR-AP"
  20 service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
 70 map device-type regex "Cisco-AIR-LAP"
  20 service-template SERVICE_TEMPLATE_FLEXAP_TRUNK
 80 map device-type regex "Cisco-TelePresence"
  20 interface-template TP_INTERFACE_TEMPLATE
 90 map device-type regex "Surveillance-Camera"
  10 interface-template MSP_CAMERA_INTERFACE_TEMPLATE
 100 map device-type regex "Video-Conference"
  10 interface-template MSP_VC_INTERFACE_TEMPLATE
 110 map device-type regex "Cisco-CAT-LAP"
  10 interface-template LAP_INTERFACE_TEMPLATE
```

## Autoconf built-in trigger for BUILTIN_DEVICE_TO_TEMPLATE:
```
policy-map type control subscriber BUILTIN_AUTOCONF_POLICY
 event identity-update match-all
  10 class always do-until-failure
   10 map attribute-to-service table BUILTIN_DEVICE_TO_TEMPLATE
```


# Autoconf troubleshooting
## Основные проверки:
1. **show device classifier attached**  # Обратить внимание на "Profile Name" (проверить, как коммутатор отпрофилировал endpoint)
2. **show template interface binding FLEXAP_TRUNK** # Проверить, на какие интерфейсы был назначен interface template
3. **show derived-config interface [interface-name]** # Проверить настройки, применённые на интерфейсе через autoconf

## Дополнительные проверки:
4. **show access-session interface [interface-name] details** # В этом выводе отображается Interface Template и Device Type, как его видит device classifier
5. **show template interface source user FLEXAP_TRUNK** # Проверить, что настроено в interface-template FLEXAP_TRUNK
6. **show policy-map type control subscriber BUILTIN_AUTOCONF_POLICY detail** # В этом выводе обратить внимание на счётчик "Executed", он всегда увеличивается, когда срабатывает device classifier, если он не нарастает, значит, есть проблемы с device classifier
7. **show parameter-map type subscriber attribute-to-service all** # Проверить настройки parameter-map (какие service-templates применять)
