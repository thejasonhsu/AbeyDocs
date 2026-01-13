## Building from source

### Building gabey (command line client)

Clone the repository to a directory of your choosing:

```shell
git clone https://github.com/abeyfoundation/go-abey/
```

Building `gabey` requires the Go compiler:

```shell
brew install go
```

Finally, build the `gabey` program using the following command.
```shell
cd go-abey
git checkout release/2.0
make gabey
```

If you see some errors related to header files of Mac OS system library, install XCode Command Line Tools, and try again.

```shell
xcode-select --install
```

You can now run `build/bin/gabey` to start your node.
