# 13. Istioctl

## Introduction
The istioctl tool is a configuration command line utility that allows service operators to debug and diagnose their Istio service mesh deployments.

## Install istioctl
https://istio.io/latest/docs/ops/diagnostic-tools/istioctl/#install-hahahugoshortcode-s2-hbhb
Install the istioctl binary with curl:

1. Download the latest release with the command:

```
$ curl -sL https://istio.io/downloadIstioctl | sh -
```

2. Add the istioctl client to your path, on a macOS or Linux system:

```
$ export PATH=$PATH:$HOME/.istioctl/bin
```

Install specific version:

```
$ curl -L https://istio.io/downloadIstioctl | ISTIO_VERSION=1.7.4 TARGET_ARCH=x86_64 sh -
```

## Istio Profiles
https://istio.io/latest/docs/setup/additional-setup/config-profiles/

```
$ istioctl profile list
Istio configuration profiles:
    empty
    minimal
    preview
    remote
    default
    demo
```

The components marked as X are installed within each profile:

```
$ istioctl install --set profile=demo  # Use a profile from the list
```

**istioctl profile diff**
```
$ istioctl profile diff <file1.yaml> <file2.yaml> [flags]
```

**istioctl profile dump**
```
$ istioctl profile dump [<profile>] [flags]
```

## istioctl proxy-status
https://istio.io/latest/docs/reference/commands/istioctl/#istioctl-proxy-status
Get an overview of your mesh:
```
$ istioctl proxy-status
```

## istioctl proxy-config
https://istio.io/latest/docs/reference/commands/istioctl/#istioctl-proxy-config
For example, to retrieve information about cluster configuration for the Envoy instance in a specific pod:
```
$ istioctl proxy-config cluster <pod-name> [flags]
```
