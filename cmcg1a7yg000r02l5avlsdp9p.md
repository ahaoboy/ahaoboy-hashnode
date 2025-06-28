---
title: "Installing fish-shell on Windows MSYS2"
datePublished: Sat Jun 28 2025 09:23:33 GMT+0000 (Coordinated Universal Time)
cuid: cmcg1a7yg000r02l5avlsdp9p
slug: installing-fish-shell-on-windows-msys2
tags: windows, shell, fishshell, msys2, windows-11

---

Thanks to [Berrysoft](https://github.com/Berrysoft) for adding support for fish on Windows [MSYS2](https://www.msys2.org/). The latest repository is available at [https://github.com/Berrysoft/fish-msys2](https://github.com/Berrysoft/fish-msys2).

The latest fish version 4.0.2 and above requires a runtime version of at least 3.6. If your runtime version is 3.5 or below, you can try the method described in this [Reddit post](https://www.reddit.com/r/rust/comments/1jjdsl0/installing_fishshell_40_on_windows_msys2/) to install fish 4.0.1.

To check your MSYS2 runtime version, use the following command:

```bash
pacman -Q msys2-runtime
# Example output: msys2-runtime 3.6.3-3
```

You can complete the installation by executing the following bash script:

```bash
cat << EOF >> /etc/pacman.conf

[fish4]
Server = https://raw.githubusercontent.com/Berrysoft/fish-msys2/publish
SigLevel = Never

EOF

pacman -Syy

pacman -S fish4/fish
```

If everything goes smoothly, you can use the latest version of fish. Verify the installation with:

```bash
fish --version
# Example output: fish, version 4.0.2-Berrysoft-5
```