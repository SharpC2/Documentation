# C2 Profile

SharpC2 uses **C2 Profiles** to customise some of its behaviours (inspired by Cobalt Strike's Malleable C2).  Profiles are defined in YAML format and have 3 "blocks": **Stage**, **PostExploitation** and **ProcessInjection**.

Example profile:

```yaml
Stage:
  DllExport: Execute
PostExploitation:
  BypassAmsi: false
  BypassEtw: false
  SpawnTo: C:\Windows\System32\notepad.exe
  AppDomain: SharpC2
ProcessInjection:
  Allocation: NtWriteVirtualMemory
  Execution: RtlCreateUserThread
```

## Stage Block
### DllExport
This is the name of the export in the DLL payload format.  This export name is used with rundll32 to run the payload, e.g:

```
rundll32 drone.dll,Execute
```

## Post Exploitation Block
### BypassAmsi / BypassEtw
These tell the Drone whether or not it should attempt to bypass AMSI and ETW on post-ex tasks by default.  This directive can be toggled during runtime with the `bypass` command.

### SpawnTo
Some post-ex tasks require the Drone to spawn another process - this setting tells the Drone what process that should be.

### AppDomain
Post-ex tasks such as `execute-assembly` will create a disposable AppDomain to execute the given .NET assembly.  The "friendly name" of those AppDomains can be controlled here.  If you want this name to be random each time, use the literal string `random`.  This directive is also used when generating the raw shellcode payload.

## Process Injection Block
This block is used to control the behaviour of process injection by the Drone, i.e. which APIs are used.

### Allocation
Controls the API(s) used to allocate the shellcode into a target process.  Options are `NtWriteVirtualMemory` and `NtMapViewOfSection`.

#### NtWriteVirtualMemory
Allocate a RW region of memory in the target process using `NtAllocateVirtualMemory`; `NtWriteVirtualMemory` to write the shellcode and `NtProtectVirtualMemory` to change the memory permission to RX.

#### NtMapViewOfSection
Uses `NtCreateSection` to create a section in the local process, `NtMapViewOfSection` to map that local section to the target process, then `NtUnmapViewOfSection` to unmap the section from the local process.

### Execution
Controls the API(s) used to execute shellcode once it's been written into the target process.  Options are `NtCreateThreadEx`, `RtlCreateUserThread` and `CreateRemoteThread`.

## Using a Profile
To use a profile, you must start the Team Server with the `--profile` option.  Only one profile may be used at a time.

```
$ dotnet TeamServer.dll --password Passw0rd! --profile /Users/rasta/Desktop/example-profile.yaml
```

## C2Lint
The SharpC2 solution includes a C2Lint tool to help validate YAML profiles.

```
$ dotnet C2Lint.dll /Users/rasta/Desktop/example-profile.yaml 

Stage Block
===========
Dll Export: Execute

Post Exploitation Block
=======================
Bypass AMSI: False
Bypass ETW: False
SpawnTo: C:\Windows\System32\notepad.exe
AppDomain: SharpC2

Process Injection Block
=======================
Allocation: NtWriteVirtualMemory
Execution: RtlCreateUserThread
```

If any errors are detected, they'll be printed to the console.

```
Stage Block
===========
Dll Export: Execute

Post Exploitation Block
=======================
Bypass AMSI: False
Bypass ETW: False
SpawnTo: C:\Windows\System32\notepad.exe
AppDomain: random
AppDomain will have a random value each time.

Process Injection Block
=======================
Allocation: NtWriteVirtualMemory
Execution: QueueUserAPC
[!!!] Execution Technique is not valid.  Options are NtCreateThreadEx, RtlCreateUserThread, CreateRemoteThread.
```