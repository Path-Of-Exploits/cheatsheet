## Forwarding port using ssh, chisel & socat

### SSH

### Local Port Forwarding
```bash
ssh -L 8888:localhost:8888 -L 8888:localhost:8888 $USER@$TARGET
```

### Remote Port Forwarding
```bash
ssh -R 1234:127.0.0.1:8000 $USER@$TARGET
ssh -R 192.168.1.1:1234:192.168.1.2:8000 $USER@$TARGET
```

### Chisel
```bash
# Local
chisel server -p 9999 --reverse
# Target
chisel client <ip_attacker>:9999 R:<port_exposé>:<ip_service>:<port_service>
```

## Socat
```bash
# Local
socat tcp-listen:8080,fork tcp:$TARGET:80
# Target
socat tcp-listen:1234,fork,reuseaddr tcp:localhost:8080
```