_Last updated: January 2026_

This guide explains how to build and run **Abey (`gabey`)** from source on CentOS-based systems.

---

## Prerequisites

Before building Abey, ensure the following dependencies are installed:

- **Git**
- **Go (latest stable version)**
- **C compiler and build tools**

If Go is not installed, download and install it from:
- https://golang.org/

---

## Building from Source

### Clone the Repository

Clone the official Abey repository to a directory of your choice:

```bash
git clone https://github.com/abeyfoundation/go-abey.git
```

---

### Install Build Dependencies

Install the required build tools using `yum`:

```bash
yum install -y build-essential
```

> Note: On some CentOS versions, you may need:
> ```bash
> yum groupinstall -y "Development Tools"
> ```

---

### Build `gabey`

Navigate to the repository, check out the release branch, and build the client:

```bash
cd go-abey
git checkout release3.0
make gabey
```

---

## Running the Node

Once the build completes successfully, you can start the node using:

```bash
build/bin/gabey
```

This will launch the Abey command-line client.

---

If you encounter build errors, verify that:
- Go is correctly installed and available in your `PATH`
- All required system packages are installed
