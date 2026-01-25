_Last updated: January 2026_

# Installation Instructions for macOS

This guide explains how to build and run **Abey (`gabey`)** from source on macOS.

---

## Prerequisites

Before building Abey, ensure the following tools are installed:

- **Git**
- **Homebrew**
- **Go (latest stable version)**

If Go is not installed, you can install it using Homebrew.

---

## Building from Source

### Clone the Repository

Clone the official Abey repository to a directory of your choice:

```bash
git clone https://github.com/abeyfoundation/go-abey.git
```

---

### Install Go

Install the Go compiler using Homebrew:

```bash
brew install go
```

---

### Build `gabey`

Navigate to the repository, check out the release branch, and build the client:

```bash
cd go-abey
git checkout release/2.0
make gabey
```

---

### Install Xcode Command Line Tools (if needed)

If you encounter errors related to macOS system headers, install the Xcode Command Line Tools:

```bash
xcode-select --install
```

Then re-run the build command.

---

## Running the Node

After a successful build, start the Abey node using:

```bash
build/bin/gabey
```

This will launch the Abey command-line client on macOS.

---

If you encounter issues, verify that:
- Go is correctly installed and available in your `PATH`
- Xcode Command Line Tools are installed
