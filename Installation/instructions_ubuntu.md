_Last updated: January 2026_

# Installation Instructions for Ubuntu

This guide explains how to build and run **Abey (`gabey`)** from source on Ubuntu-based systems.

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

Install the required build tools:

```bash
sudo apt-get update
sudo apt-get install -y build-essential
```

---

### Check Out Release Branch

Navigate to the repository and check out the latest release branch:

```bash
cd go-abey
git checkout release3.0
```

---

### Build `gabey`

Build the Abey command-line client:

```bash
make gabey
```

---

## Running the Node

After a successful build, start the node using:

```bash
build/bin/gabey
```

This will launch the Abey command-line client on Ubuntu.

---

If you encounter issues, verify that:
- Go is correctly installed and available in your `PATH`
- All required system packages are installed
