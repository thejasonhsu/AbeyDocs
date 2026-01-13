# Source

## Compiling gabey with tools from chocolatey

The Chocolatey package manager provides an easy way to get
the required build tools installed. If you don't have chocolatey yet,
follow the instructions on https://chocolatey.org to install it first.

Then open an Administrator command prompt and install the build tools
we need:

```text
C:\Windows\system32> choco install git
C:\Windows\system32> choco install golang
C:\Windows\system32> choco install mingw
``` 

Installing these packages will set up the `Path` environment variable.
Open a new command prompt to get the new `Path`. The following steps don't
need Administrator privileges.

Please ensure that the installed Go version is 1.15 (or any later version).

First we'll create and set up a Go workspace directory layout,
then clone the source.

***OBS*** If, during the commands below, you get the following message: 
```
 WARNING: The data being saved is truncated to 1024 characters.
```
Then that means that the `setx` command will fail, and proceeding will truncate the `Path`/`GOPATH`. If this happens, it's better to abort, and try to make some more room in `Path` before trying again. 

```text
1 C:\Users\xxx> set "GOPATH=%USERPROFILE%"
2 C:\Users\xxx> set "Path=%USERPROFILE%\bin;%Path%"
3 C:\Users\xxx> setx GOPATH "%GOPATH%"
4 C:\Users\xxx> setx Path "%Path%"
5 C:\Users\xxx> mkdir src\github.com\abeyfoundation
6 C:\Users\xxx> git clone https://github.com/abeyfoundation/go-abey src\github.com\abeyfoundation\go-abey
7 C:\Users\xxx> cd src\github.com\abeyfoundation\go-abey
8 C:\Users\xxx> go get -u -v golang.org/x/net/context
```
if the step 8 ran error, can execute the next commands:
```
C:\Users\xxx> mkdir -p "%GOPATH%"/src/golang.org/x/
C:\Users\xxx> cd "%GOPATH%"/src/golang.org/x/
C:\Users\xxx> git clone https://github.com/golang/net.git net
C:\Users\xxx> go install net
```

Finally, the command to compile gabey is:

```text
C:\Users\xxx\src\github.com\abeyfoundation\go-abey> go install -v ./cmd/...
```
