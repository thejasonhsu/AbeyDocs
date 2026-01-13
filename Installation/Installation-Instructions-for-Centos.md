## Building from source

### Building gabey (command line client)

Clone the repository to a directory of your choosing:

```shell
git clone https://github.com/abeyfoundation/go-abey/
```
Install latest distribution of [Go](https://golang.org/) if you don't have it already.

Building `gabey` requires Go and C compilers to be installed:

```shell
yum install -y build-essential
```

Finally, build the `gabey` program using the following command.
```shell
cd go-abey
git checkout release3.0
make gabey
```

You can now run `build/bin/gabey` to start your node.
