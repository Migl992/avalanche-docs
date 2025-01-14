---
tags: [Build, Virtual Machines, Rust, Avalanche-RS]
description: Learn how to install a Rust-based VM
sidebar_label: Installing Your VM
pagination_label: Installing Your VM
sidebar_position: 3
---

# Installing a VM

AvalancheGo searches for and registers VM plugins under the `plugins` [directory](/nodes/configure/avalanchego-config-flags.md#--plugin-dir-string).

To install the virtual machine onto your node, you need to move the built virtual machine binary
under this directory. Virtual machine executable names must be either a full virtual machine ID
(encoded in CB58), or a VM alias.

Copy the binary into the plugins directory.

```bash
cp -n <path to your binary> $GOPATH/src/github.com/ava-labs/avalanchego/build/plugins/
```

## Node Is Not Running

If your node isn't running yet, you can install all virtual machines under your `plugin` directory
by starting the node.

## Node Is Already Running

Load the binary with the `loadVMs` API.

```bash
curl -sX POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"admin.loadVMs",
    "params" :{}
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/admin
```

Confirm the response of `loadVMs` contains the newly installed virtual machine
`tGas3T58KzdjcJ32c6GpePhtqo9rrHJ1oR9wFBtCcMgaosthX`. You'll see this virtual machine as well as any
others that weren't already installed previously in the response.

```json
{
  "jsonrpc": "2.0",
  "result": {
    "newVMs": {
      "tGas3T58KzdjcJ32c6GpePhtqo9rrHJ1oR9wFBtCcMgaosthX": [
        "timestampvm-rs",
        "timestamp-rs"
      ],
      "spdxUxVJQbX85MGxMHbKw1sHxMnSqJ3QBzDyDYEP3h6TLuxqQ": []
    }
  },
  "id": 1
}
```

Now, this VM's static API can be accessed at endpoints `/ext/vm/timestampvm-rs` and
`/ext/vm/timestamp-rs`. For more details about VM configs, see
[here](/nodes/configure/avalanchego-config-flags.md#vm-configs).

In this tutorial, we used the VM's ID as the executable name to simplify the process. However,
AvalancheGo would also accept `timestampvm-rs` or `timestamp-rs` since those are registered aliases
in previous step.
