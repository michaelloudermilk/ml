## How The Discovery Works

UNMS sends _discovery packet_ to every IP address in the given range and waits for device reply. The device sends back basic information shown in the Discovery Manager - name, SSID, model, firmware version, mac address and list of all interfaces.

The discovery packet is sent using UDP to port 10001, it consists of four bytes `0x1 0x0 0x0 0x0`.

The device only responds to packets sent from private network ranges.

## Blocking The Discovery

Preventing your device from being _discovered_.

### AirMax

Go to `SETTINGS` -> `Services` -> `Device Discovery` -> set checkbox to `OFF` and click `Save Settings`

### EdgeRouter

Currently this can be done only through the firewall rule:

To setup the rule
```
configure
edit firewall name disable-discover
set default-action accept
set rule 100 action 'drop'
set rule 100 description 'Drop discovery packets'
set rule 100 protocol 'udp'
set rule 100 destination port 10001
exit
# apply rule to the interface that is connected to the Internet
set interfaces ethernet eth0 firewall local name 'disable-discover'
commit
save

show interfaces
```

NOTE: The rule will block entire incoming UDP communication to port 10001, potentially disabling more than just the discovery

To disable the rule
```
configure
delete interfaces ethernet eth0 firewall local name 'disable-discover'
commit
save
```
