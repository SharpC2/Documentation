# Hosting Files

You can host arbitrary files on the default HTTP handler (useful as a quick payload delivery method).

```{admonition} Note
:class: information

There is no automatic link between payload generation and hosting files, so you must generate and download a payload first and then upload it to the handler.
```

## Adding

You may host any file from your own filesystem by providing its path and the filename you want to host it as.

```text
[drones] > hosted-files
[hosted-files] > add a.txt c:\users\daniel\desktop\test.txt
[hosted-files] > list

Filename  Size
--------  ----
a.txt     14
```

```
curl http://localhost/a.txt
this is a test
```

```
[hosted-files] > remove a.txt
[hosted-files] > list

No hosted files
```

```{admonition} Note
:class: information

Adding a hosted file will automatically start the handler if it's not already.
```