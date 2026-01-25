_Last updated: January 2026_

This guide explains how to build **Abey (`gabey`)** from source on Windows using Chocolatey and Go.

---

## Prerequisites

Before building Abey, ensure the following requirements are met:

- **Windows 10 or later**
- **Administrator access**
- **Chocolatey package manager**
- **Go 1.15 or later**

If Chocolatey is not installed, follow the instructions at:
- https://chocolatey.org

---

## Install Build Tools

Open an **Administrator Command Prompt** and install the required tools:

```text
choco install git
choco install golang
choco install mingw
```

These installations will automatically update your `PATH` environment variable.  
After installation completes, **open a new command prompt** (non-administrator is fine).

---

## Configure Go Environment

Ensure that the installed Go version is **1.15 or newer**.

Set up the Go workspace and environment variables:

```text
set "GOPATH=%USERPROFILE%"
set "Path=%USERPROFILE%\bin;%Path%"
setx GOPATH "%GOPATH%"
setx Path "%Path%"
```

**Important Note**

If you see the warning below, the `setx` command may truncate your `PATH` variable:

```
WARNING: The data being saved is truncated to 1024 characters.
```

If this occurs, **stop immediately**, clean up your `PATH`, and retry the setup to avoid breaking your environment.

---

## Clone the Repository

Create the required directory structure and clone the Abey source code:

```text
mkdir src\github.com\abeyfoundation
git clone https://github.com/abeyfoundation/go-abey src\github.com\abeyfoundation\go-abey
cd src\github.com\abeyfoundation\go-abey
```

---

## Resolve Go Dependencies (if needed)

Attempt to fetch required Go dependencies:

```text
go get -u -v golang.org/x/net/context
```

If the command above fails, manually install the dependency:

```text
mkdir "%GOPATH%\src\golang.org\x"
cd "%GOPATH%\src\golang.org\x"
git clone https://github.com/golang/net.git net
go install net
```

---

## Build `gabey`

Compile the Abey binaries using:

```text
go install -v ./cmd/...
```

This will build `gabey` and related tools into your Go `bin` directory.

---

## Running the Node

After a successful build, you can run the Abey node by executing:

```text
gabey
```

Ensure `%GOPATH%\bin` is included in your `PATH`.

---

## Troubleshooting

- Verify Go is correctly installed by running `go version`
- Ensure `GOPATH` and `PATH` are set correctly
- Reopen the command prompt after installing tools
- Avoid truncating the Windows `PATH` variable
