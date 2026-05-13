# Container Go

A minimal Linux container runtime built in Go. Uses kernel namespaces (UTS, PID, mount) and `pivot_root` to run commands in an isolated environment.

## Usage

```
# prepare a root filesystem in ./rootfs
go build -o container .
sudo ./container run <command> [args...]
```

The `run` command spawns a child process in new namespaces with `rootfs` as the root filesystem.

## How it works

- **parent()** — forks itself into new UTS, PID, and mount namespaces
- **child()** — bind-mounts `rootfs`, pivots the root, then executes the target command

Requires Linux with root privileges (for namespace creation and `pivot_root`).
