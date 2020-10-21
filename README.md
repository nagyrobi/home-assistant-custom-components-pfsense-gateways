Support for pfsense gateways status monitoring using alexpmorris / pfsense-status-gateways-json script

Check https://github.com/alexpmorris/pfsense-status-gateways-json on how to install it on pfSense

## Example config

```yaml
sensor:
- platform: pfsense_gateways
  host: 172.22.215.254
  name: pfSense gateway
  key: pfsense
  monitored_gateway_interfaces:
    - wan
    - opt2
```

## Example automations
```yaml
- alias: "pfSense GW-Down"
  trigger:
    - platform: state
      entity_id: sensor.pfsense_gateway_gw_wan1
      to: 'False'
  action:
    - service: notify.filelog
      data:
        message: "{{ now().timestamp() | timestamp_custom('%Y-%m-%d %H:%M:%S') }} My internet just died"

- alias: "pfSense GW-Down 3min"
  trigger:
    - platform: state
      entity_id: sensor.pfsense_gateway_gw_wan1
      to: 'False'
      for:
        minutes: 3
        seconds: 10
  action:
    - service: notify.filelog
      data:
        message: "{{ now().timestamp() | timestamp_custom('%Y-%m-%d %H:%M:%S') }} My internet just died more than 3 minutes ago, rebooting ISP crap"
    - service: switch.turn_on
      entity_id: switch.relay_powercycle_modem
```
