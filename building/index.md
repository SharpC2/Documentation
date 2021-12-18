# Building

The team server and client are written in .NET 6.0 (LTS) and are therefore cross-platform.  To build the projects, use `dotnet publish` and target the runtime for the OS you want that component to run on.  Use either `--self-contained` or `--no-self-contained` to control whether or not the .NET runtime is included in the final package.

To build the team server for Linux:

```text
rasta@dev ~/SharpC2 (main)> dotnet publish TeamServer -c Release -r linux-x64 --self-contained
rasta@dev ~/SharpC2 (main)> file TeamServer/bin/Release/net6.0/linux-x64/publish/TeamServer
TeamServer/bin/Release/net6.0/linux-x64/publish/TeamServer: ELF 64-bit LSB pie executable, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=8d70ea905d53818ded326e69d5b3cf6dd031dab1, for GNU/Linux 2.6.32, stripped
```

To build the client for macOS:

```text
rasta@dev ~/SharpC2 (main)> dotnet publish Client -c Release -r osx-x64 --self-contained
rasta@dev ~/SharpC2 (main)> file Client/bin/Release/net6.0/osx-x64/publish/SharpC2
Client/bin/Release/net6.0/osx-x64/publish/SharpC2: Mach-O 64-bit executable x86_64
```

See [https://docs.microsoft.com/en-us/dotnet/core/rid-catalog](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog) for more information on the dotnet RID catalog.