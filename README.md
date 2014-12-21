# Gentoo Wercker box [![wercker status](https://app.wercker.com/status/92877b55ac71655b2435346c0218adb0/s "wercker status")](https://app.wercker.com/project/bykey/92877b55ac71655b2435346c0218adb0)

A [Wercker box][0] that contains a Gentoo chroot under `/gentoo`.

## But why‽

Because Wercker boxes currently only support a single platform, Ubuntu 12.04.
Ubuntu Precise is not fully multilib: `build-essential:amd64` and
`build-essential:i386` conflict each other, making it impossible to have more
than one C or C++ compiler toolchain installed at a time.

Installing a (recent enough) Gentoo under a chroot allows us to leverage
Gentoo's multilib support: we can bootstrap toolchains for cross-compiling to
various OSes, architectures and C libraries (GNU libc6, uclibc, etc.) under a
chroot.

## Using the chroot

To run a commnad in the chroot environment, just prefix it with `sudo chroot
/gentoo`. For example, to install `crossdev` inside the chroot:

```sh
sudo chroot /gentoo emerge -q crossdev
```

There is even a convenience step, [`attilaolah/gentoo`][3], that will run a
shell script inside the `chroot` environment without much configuration. Use it
like this:

```yaml
build:
  steps:
    - attilaolah/gentoo:
        command: echo "This is a command."
    - attilaolah/gentoo:
        command: |
          echo "This is a multi-line command."
          echo "It will all be executed by a single shell call."
```

There is another convenience step, [`attilaolah/emerge`][4], that calls
`emerge` under the chroot. Use it like this:

```yaml
build:
  steps:
    - attilaolah/emerge:
        atom: figlet
    - attilaolah/emerge:
        atom: "=dev-lang/python-3.4.2"
        use: "wininst -examples -doc"
```

## Portage updates

The Portage tree is synced when Wercker publishes this box to the registry. If
you need something newer than that, just update the tree:

```sh
sudo chroot /gentoo emerge --sync
```

## More Wercker boxes

This box only contains a bare-bones Gentoo system, with Stage3 and a synced
Portage tree. Other boxes are available that ship pre-installed tools:

* [`gentoo-crossdev`][2]

## Stage3 version

The current Stage3 version is **20141204**. If there is a newer version
available [in autobuilds][1], just send me a pull request and I'll merge it.

[0]: https://app.wercker.com/#applications/54944cfd6b3ba8733da381b9/tab/details
[1]: http://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64/
[2]: //github.com/attilaolah/wercker-box-gentoo-crossdev
[3]: //github.com/attilaolah/wercker-step-gentoo
[3]: //github.com/attilaolah/wercker-step-emerge
