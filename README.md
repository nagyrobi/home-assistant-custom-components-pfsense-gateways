## Gateways status for pfSense

Support for pfSense gateways status monitoring using alexpmorris / pfsense-status-gateways-json script. Check https://github.com/alexpmorris/pfsense-status-gateways-json on how to install it on pfSense.

Returns `True` or `False` depending on the status of the gateway. Useful to trigger an external reboot or power-cycle of the ISP modem, or similar.

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
    - service: notify.me
      data:
        message: "My internet just died"

- alias: "pfSense GW-Down 5min"
  trigger:
    - platform: state
      entity_id: sensor.pfsense_gateway_gw_wan1
      to: 'False'
      for:
        minutes: 5
  action:
    - service: notify.me
      data:
        message: "My internet just died more than 5 minutes ago, rebooting ISP crap"
    - service: switch.turn_on
      entity_id: switch.relay_powercycle_modem
```

Support forum: https://community.home-assistant.io/t/pfsense-stat-monitor/61070/55?u=robi
