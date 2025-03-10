---
title: Device Authorization Grant
type: docs
prev: /
next: docs/folder/
---

The OAuth2 Device Authorization Grant (also known as Device Flow) is a mechanism that allows users to authorize devices with limited input capabilities, such as smart TVs or IoT devices, to access OAuth2 protected resources. This flow involves the following steps:

1. **Device Requests Authorization**: The device requests authorization from the authorization server and receives a device code and a user code.
2. **User Authorizes Device**: The user visits a verification URL on another device (e.g., a smartphone or computer), logs in, and enters the user code to authorize the device.
3. **Device Polls for Authorization**: The device continuously polls the authorization server to check if the user has authorized it.
4. **Access Token Issued**: Once the user authorizes the device, the authorization server issues an access token to the device.

This flow is particularly useful for devices that do not have a web browser or have limited input capabilities.

## Using Cobra-OAuth2 Library

To simplify the integration of OAuth2 authorization flow into your Cobra CLI applications, you can use the [Cobra-OAuth2](https://github.com/nauthera/cobra-oauth2) Go library. This library provides prebuilt commands for handling login, logout, and token management, making it easy to integrate secure authentication into your CLI applications.

### Features

- **Quick Setup**: Add OAuth2 support to your Cobra CLI with just a few lines of code.
- **Token Management**: Automatically handle token storage and retrieval.
- **Flexible Storage Providers**: Store tokens securely using your preferred storage backend (e.g., keyring, file system).
- **Prebuilt Commands**: Includes `login`, `logout`, and `token` commands to handle authentication flows out of the box.

### Installation

Install the module via `go get`:

```sh
go get github.com/nauthera/cobra-oauth2
```

### Example Usage

#### Main Application Setup

Define your main entry point and execute your Cobra CLI:

```go
package main

import "github.com/nauthera/cobra-oauth2/examples/basic/cmd"

func main() {
	cmd.Execute()
}
```

#### Root Command Setup

Set up the root command and initialize OAuth2 commands:

```go
package cmd

import (
	"net/url"
	"os"

	"github.com/nauthera/cobra-oauth2/pkg/auth"
	"github.com/nauthera/cobra-oauth2/pkg/storage"
	"github.com/spf13/cobra"
)

const CLIENT_ID = "my-client-id"

// rootCmd represents the base command when called without any subcommands
var rootCmd = &cobra.Command{
	Use: "cobra-oauth2",
}

// Execute adds all child commands to the root command and sets flags appropriately.
// This is called by main.main(). It only needs to happen once to the rootCmd.
func Execute() {
	err := rootCmd.Execute()
	if err != nil {
		os.Exit(1)
	}
}

func init() {
	discoveryUrl, err := url.Parse("https://foo-bar.nauthera.io/.well-known/openid-configuration")
	if err != nil {
		rootCmd.PrintErr("error parsing discovery URL: ", err)
		return
	}

	storageProvider := storage.NewKeyringStorage(CLIENT_ID)

	options := []auth.Option{
		auth.WithDiscoveryURL(*discoveryUrl),
		auth.WithClientID(CLIENT_ID),
		auth.WithStorageProvider(storageProvider),
	}

	rootCmd.AddCommand(
		auth.NewLoginCommand(options...),
		auth.NewTokenCommand(options...),
		auth.NewLogoutCommand(options...),
	)
}
```

### Commands

- **`login`**: Initiates the OAuth2 login flow.
- **`token`**: Fetches and displays the current access token.
- **`logout`**: Clears the stored token.

For more details, refer to the [Cobra-OAuth2 GitHub repository](https://github.com/nauthera/cobra-oauth2).

