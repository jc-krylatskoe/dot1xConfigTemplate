# dot1xConfigTemplate
Repo to store dot1x switch configurations and notes from ISE wired/wireless ieMentor lab

## Repo Index:
[dot1x-sw-current.cfg](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/dot1x-sw-current.cfg) - the most up to date switch's lab configuration<br>
[docs/](https://github.com/jc-krylatskoe/dot1xConfigTemplate/tree/main/docs) - documentation folder<br>
[docs/autoconf.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/autoconf.md) - details about autoconf configuration and troubleshooting<br>
[docs/flexconnect_ap_configuration.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/flexconnect_ap_configuration.md) - details about solution for FlexConnect APs<br>
[docs/flexconnect_ap_output.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/flexconnect_ap_output.md) - command outputs for FlexConnect APs solution<br>
[docs/radius_tracing.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/radius_tracing.md) - command outputs for FlexConnect APs solution<br>
[docs/trustsec.md](https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/docs/trustsec.md) - details about TrustSec configuration and troubleshooting<br>
[diagrams/](https://github.com/jc-krylatskoe/dot1xConfigTemplate/tree/main/diagrams) - folder with draw.io lab diagrams<br>


## Notes
### The following default commands were deleted from switch's configuration:
```
no service-template DEFAULT_CRITICAL_VOICE_TEMPLATE
no service-template DEFAULT_CRITICAL_DATA_TEMPLATE
no service-template DEFAULT_LINKSEC_POLICY_MUST_SECURE
no service-template DEFAULT_LINKSEC_POLICY_SHOULD_SECURE
```

## Lab Diagram
<img src="https://github.com/jc-krylatskoe/dot1xConfigTemplate/blob/main/diagrams/ieMentor_lab.drawio.png"/>
