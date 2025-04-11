```bash
███████ ██ ███    ██  ██████        ██████   ██████  ██   ██     ████████ ███████ ███    ███ ██████  ██       █████  ████████ ███████ ███████ 
██      ██ ████   ██ ██             ██   ██ ██    ██  ██ ██         ██    ██      ████  ████ ██   ██ ██      ██   ██    ██    ██      ██      
███████ ██ ██ ██  ██ ██   ███ █████ ██████  ██    ██   ███          ██    █████   ██ ████ ██ ██████  ██      ███████    ██    █████   ███████ 
     ██ ██ ██  ██ ██ ██    ██       ██   ██ ██    ██  ██ ██         ██    ██      ██  ██  ██ ██      ██      ██   ██    ██    ██           ██ 
███████ ██ ██   ████  ██████        ██████   ██████  ██   ██        ██    ███████ ██      ██ ██      ███████ ██   ██    ██    ███████ ███████
```
# Tek server config, 2 senaryo

# Server
```bash
{
  "inbounds": [
    {
      "type": "vmess",
      "listen": "0.0.0.0",
      "listen_port": 1923,
      "users": [
        {
          "uuid": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12"
        }
      ],
      "transport": {
        "type": "ws",
        "path": "/vmess-ws-public"
      }
    }
  ],
  "outbounds": [
    {
      "type": "direct",
      "tag": "direct"
    }
  ],
  "route": {
    "rules": [
      {
        "ip_cidr": "127.0.0.53/32",
        "port": 53,
        "action": "route",
        "outbound": "direct"
      },
      {
        "ip_is_private": true,
        "action": "reject",
        "method": "drop"
      }
    ]
  }
}
```
# Senaryo 1: VMess over WebSocket
```bash
{
  "dns": {
    "servers": [
      {
        "address": "tcp://127.0.0.53",
        "detour": "vmess"
      }
    ]
  },
  "inbounds": [
    {
      "type": "tun",
      "address": "172.18.0.1/30",
      "auto_route": true
    }
  ],
  "outbounds": [
    {
      "type": "vmess",
      "tag": "vmess",
      "server": "SERVERIP",
      "server_port": 80,
      "uuid": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12",
      "security": "auto",
      "transport": {
        "type": "ws",
        "path": "/vmess-ws-public",
        "headers": {
          "Host": "SNI SPPOFING BURDA"
        }
      }
    }
  ],
  "route": {
    "rules": [
      {
        "action": "sniff"
      },
      {
        "protocol": "dns",
        "action": "hijack-dns"
      }
    ],
    "auto_detect_interface": true
  }
}
```
# SENARYO 2: VMess over WebSocket over TLS
```bash
{
  "dns": {
    "servers": [
      {
        "address": "tcp://127.0.0.53",
        "detour": "vmess"
      }
    ]
  },
  "inbounds": [
    {
      "type": "tun",
      "address": "172.18.0.1/30",
      "auto_route": true
    }
  ],
  "outbounds": [
    {
      "type": "vmess",
      "tag": "vmess",
      "server": "SERVER IP",
      "server_port": 443,
      "uuid": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12",
      "security": "zero",
      "tls": {
        "enabled": true,
        "server_name": "SNI SPOOFING BURDA",
        "insecure": true
      },
      "transport": {
        "type": "ws",
        "path": "/vmess-ws-public"
      }
    }
  ],
  "route": {
    "rules": [
      {
        "action": "sniff"
      },
      {
        "protocol": "dns",
        "action": "hijack-dns"
      }
    ],
    "auto_detect_interface": true
  }
}
```
