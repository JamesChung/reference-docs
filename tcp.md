# TCP

## Opening a TCP connection

> With telnet
```
telnet <URL>:<port>

# Example:
telnet api.example.com 80
```

> With OpenSSL
```
openssl s_client --quiet --connect <URL>:<port>

# Example:
openssl s_client --quiet --connect api.example.com:443
```
