# Go Documentation

## Installing Go

### Getting Started

If you are upgrading from an older version of Go you must first remove the existing version.

If you use *go build* in the source code directory, an executable file will be built alongside source code.

You can run *go install* to install the binary into your workspace's bin directory or *go clean* to remove it.

The *GOPATH* environment variable specifies the location of your workspace. If no *GOPATH* is set, it is assumed to be *$HOME/go* on Unix systems and *%USERPROFILE%\go* on Windows. If you want to use a custom location as your workspace, you can set the *GOPATH* environment variable.
