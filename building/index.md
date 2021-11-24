# Building

The team server and client are written in .NET 6.0 (LTS) and are therefore cross-platform.  To build the projects, you should use `dotnet publish` and target the runtime for the OS you want that component to run on.

The most common scenario is to run the Team Server on Linux - in which case, you would do:

```text
rasta@Rastas-MBP TeamServer % pwd
/Users/rasta/SharpC2/TeamServer

rasta@Rastas-MBP TeamServer % dotnet publish -c Release -r linux-x64
```

The built application and all dependencies will then be in:

```text
/Users/rasta/SharpC2/TeamServer/bin/Release/net5.0/linux-x64/publish/
```

The compiled executable will be native for the target platform:

```text
rasta@Rastas-MBP publish % file TeamServer
TeamServer: ELF 64-bit LSB pie executable, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=4774ff99f6c0571b7ad9ba0c004dd50d220d0720, stripped
```

To build the client for macOS:

```text
rasta@Rastas-MBP Client % pwd
/Users/rasta/SharpC2/Client

rasta@Rastas-MBP Client % dotnet publish -c Release -r macos-x64
rasta@Rastas-MBP Client % cd bin/Release/net5.0/osx-x64/publish
rasta@Rastas-MBP publish % file SharpC2
SharpC2: Mach-O 64-bit executable x86_64
```

See [https://docs.microsoft.com/en-us/dotnet/core/rid-catalog](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog) for more information on the dotnet RID catalog.