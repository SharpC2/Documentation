# Command Reference

This page provides a simple reference for the default commands provided by the framework.

## core

### sleep
Sets the sleep interval (in seconds) and jitter (percentage) of the Drone.  This dictates the frequency at which the Drone attempts to talk to the Team Server.  Whilst a Drone is "sleeping" it cannot receive new tasks or send results back.  It will continue to execute any task already given to it.

### load-module
Loads an external Module into the Drone.  Modules must inherit from the `DroneModule` class and are covered in detail [here](../customising/index.html#custom-drone-modules).

### exit
Instructs the Drone to terminate itself.

## stdapi

The "Standard API" module is automatically pushed to new Drones on initial check-in and provides most of the everyday commands.

### pwd
Prints the current working directory of the Drone.

### cd
Change the current working directory of the Drone.  Returns the new working directory.

### ls
List the file and directory information in the current working directory of the Drone, or the specified path.

### ps
List running processes.

### getuid
Returns the current user's identity.

### shell
Execute a command via the Command Prompt.  Returns output.

### run
Execute the specified binary with optional args.  Does not use cmd.exe.  Returns output.

### execute-assembly
Loads a .NET assembly in a disposable AppDomain and calls its Entry Point.

### overload
Maps a native DLL into memory of the Drone and calls the specifed export with optional args.  It attempts to find a legitimate signed module to overload and will fail execution if a suitable module cannot be found.  There is currently no way of modifying that behaviour.

### bypass
Instructs the Drone whether or not it should attempt to bypass AMSI and/or ETW when executing post-ex commands.  The Drone uses the in-built MinHook.NET library to hook `amsi.dll!AmsiScanBuffer` and `ntdll.dll!EtwEventWrite` respectively.  The functions are hooked just prior to a command being executed and unhooked immediately afterwards.

### shinject
Inject arbitrary shellcode into the specified PID.  The style of injection is governed by the [Process Injection](../c2profile/index.html#process-injection-block) block of the C2 Profile.

## incognito

The "Incognito Module" provides functionality for utilising Windows tokens.  It acts as a "token store" where tokens can be created or stolen and then reused multiple times.  Not loaded into the Drone by default - must be loaded with the `load-module` command.

### list-tokens
List the tokens currently in the store.

### use-token
Use the specified token from the store.  Tokens are referred to by a human-readable handle format.

### delete-token
Remove a token from the store.

### make-token
Uses the `LogonUserA` API to create a new token with the provided plaintext credentials (domain, username and password).  Uses the `LOGON32_LOGON_NEW_CREDENTIALS` logon type which makes the token suitable for network interactions, but not local ones.  Token is automatically impersonated and added to the token store.  Credentials are not verified.

### steal-token
Calls `DuplicateTokenEx` to duplicate the access token of the specified process.  Token is automatically impersonated and added to the token store.

### rev2self
Drops any impersonatation and reverts you back to your own token.  Tokens are kept in the token store and re-used with the `use-token` command.

## sockets

The "Sockets Module" provides various port forwarding functionality.  Not loaded into the Drone by default - must be loaded with the `load-module` command.

### list-fportfwds
Lists all the active reverse port forwards.

### start-rportfwd
Start a new reverse port forward.  The Drone will bind and listen on the specified port and tunnel all traffic back to the Team Server.  The Team Server forwards the traffic for the forward host & port, and returns the response to the Drone.  The Drone then forwards that response back to the original requester.

### stop-rportfwd
Stop the reverse port forward by the given bind port.

### purge-rportfwd
Remove all active reverse port forwards on the Drone.