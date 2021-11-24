# Command Reference

This page provides a simple reference for the default commands provided by the framework.

## core

### abort
Cancels a running task.  Tasks are referred to by a short GUID which is provided when a task starts running.

### exit
Instructs the drone to terminate itself.

### link
Link to an SMB drone.

### load-module
Loads an external module into the drone.  Modules must inherit from the `DroneModule` class and are covered in detail [here](../customising/index.html#custom-drone-modules).

### sleep
Sets the sleep interval (in seconds) and jitter (percentage) of the drone.  This dictates the frequency at which the drone attempts to talk to the team server.  Whilst a drone is sleeping it will continue to execute any task already given to it, but cannot receive new tasks or send results back.

## stdapi
The "Standard API" module is automatically pushed to new drones on initial check-in and provides most of the everyday commands.

### bypass
Instructs the drone whether or not it should attempt to bypass AMSI and/or ETW when executing post-ex commands.  The drone uses the in-built MinHook.NET library to hook `amsi.dll!AmsiScanBuffer` and `ntdll.dll!EtwEventWrite` respectively.  The functions are hooked just prior to a command being executed and unhooked immediately afterwards.

### cat
Reads the specified file as text (`File.ReadAllText()`).

### cd
Change the current working directory of the drone.  Returns the new working directory.

### execute-assembly
Loads a .NET assembly into memory and calls its Entry Point.

### getuid
Get's the Windows Identity of the current user.  Output is not changed based on token impersonation.

### ls
List the file and directory information in the current working directory of the drone, or the specified path.

### mkdir
Creates a directory at the provided path.

### overload
Maps a native DLL into memory of the drone and calls the specifed export with optional arguments.  It attempts to find a legitimate signed module to overload and will fail execution if a suitable module cannot be found.  There's currently no way of modifying that behaviour.

### ps
List running processes.

### pwd
Prints the current working directory of the drone.

### rm
Deletes a file.

### rmdir
Deletes a directory recursively.

### run
Execute the specified binary with optional arguments.  Does not use cmd.exe.  Returns output.

### services
Lists services on the current or target machine.

### shell
Execute a command via the Command Prompt.  Returns output.

### shinject
Inject arbitrary shellcode into the specified PID.  The style of injection is governed by the [Process Injection](../c2profile/index.html#process-injection-block) block of the C2 Profile.

### upload
Uploads a file to the current working directory of the drone.

## incognito
The "Incognito Module" provides functionality for utilising Windows tokens.  It acts as a "token store" where tokens can be created or stolen and then reused multiple times.  Not loaded into the drone by default - must be loaded with the `load-module` command.

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
Drops any impersonatation and reverts you back to your own token.  Tokens are kept in the token store and can be re-used with the `use-token` command.

## sockets
The "Sockets Module" provides various port forwarding functionality.  Not loaded into the drone by default - must be loaded with the `load-module` command.

### list-fportfwds
Lists all the active reverse port forwards.

### start-rportfwd
Start a new reverse port forward.  The drone will bind and listen on the specified port and tunnel all traffic back to the team server.  The team server forwards the traffic to the forward host & port, and returns the response to the drone.  The drone then forwards that response back to the original requester.

### stop-rportfwd
Stop the reverse port forward by the given bind port.

### purge-rportfwd
Remove all active reverse port forwards on the drone.