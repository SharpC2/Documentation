# C2 Profile

SharpC2 uses **C2 Profiles** to customise some of its behaviours (inspired by Cobalt Strike's Malleable C2).  Profiles are defined in YAML format and have 3 "blocks": **Stage**, **PostExploitation** and **ProcessInjection**.

Example profile:

```yaml
Stage:
  SleepTime: 60
  SleepJitter: 20
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
### SleepTime
The sleep time (in seconds) of the HTTP drone - it defines how often it checks into the team server. Does not effect P2P drones.

### SleepJitter
A percentage offset by which to randomise the sleep time. For example, a 60 second sleep with a 20% jitter means the drone will randomly sleep between 48 and 72 seconds.

### DllExport
The name of the export in the DLL payload format.  This export name is used with rundll32 to run the payload, e.g: `rundll32 drone.dll,Execute`.

## Post Exploitation Block
### BypassAmsi / BypassEtw
These tell the drone whether or not it should attempt to bypass AMSI and ETW on post-ex tasks.  This directive can be toggled during runtime with the `bypass` command.

### SpawnTo
Some post-ex tasks require the drone to spawn another process - this defines what process that should be.

### AppDomain
This sets the "friendly name" of AppDomain used by the donut-generated (raw) shellcode payload. If you want this name to be random each time, use the literal string `random`.

## Process Injection Block
### Allocation
Controls the API(s) used to allocate the shellcode into a target process.  Options are `NtWriteVirtualMemory` and `NtMapViewOfSection`.

#### NtWriteVirtualMemory
Allocate a RW region of memory in the target process using `NtAllocateVirtualMemory`; `NtWriteVirtualMemory` to write the shellcode and `NtProtectVirtualMemory` to change the memory permission to RX.

#### NtMapViewOfSection
Uses `NtCreateSection` to create a section in the local process, `NtMapViewOfSection` to map that local section to the target process, then `NtUnmapViewOfSection` to unmap the section from the local process.

### Execution
Controls the API(s) used to execute shellcode once it's been written into the target process.  Options are `NtCreateThreadEx`, `RtlCreateUserThread` and `CreateRemoteThread`.

## Using a Profile
To use a profile, you must start the team server with the `-y` (--profile) option.  Only one profile may be used at a time.

## C2Lint
The SharpC2 solution includes a C2Lint tool to help validate YAML profiles.

```
$ dotnet run /path/to/profile.yaml

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
Execution: RtlCreateUserThrea
[!!!] Execution Technique is not valid.  Options are NtCreateThreadEx, RtlCreateUserThread, CreateRemoteThread.
```