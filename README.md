# Old Scripts

This is an archive of scripts accrued over the years, all dated to 2023 and prior, most of them being made before 2023. Most scripts were created ad hoc and, as such, have a very narrow design scope. In addition, some scripts have been modified to remove syntax errors or personal information.

When a particular script has positional and/or optional arguments, the relevant subsection about the script will contain a synopsis on its usage.


# Table of Contents

- [`add-res`](#add-res)
- [`archive-dir`](#archive-dir)
- [`check-iommu`](#check-iommu)
- [`cpbak`](#cpbak`)
- [`eclip`](#eclip)
- [`firewall`](#firewall)
- [`fmclk`](#fmclk)
- [`gen-res`](#gen-res)
- [`img-show`](#img-show)
- [`limit-cpu-cores`](#limit-cpu-cores)
- [`mkram`](#mkram)
- [`pdf2mp3`](#pdf2mp3)
- [`rnsong`](#rnsong)
- [`screenshot2text`](#screenshot2text)
- [`set-card0-auto`](#set-card0-auto)
- [`set-card0-high`](#set-card0-high)
- [`sshvm`](#sshvm)
- [`sys-update`](#sys-update)
- [`unfirewall`](#unfirewall)
- [`vpnr`](#vpnr)
- [`watch-cpu`](#watch-cpu)



# Script Names & Functions

## `add-res`

## `vpnr`

Meant to work with the `surfshark-vpn` CLI, `vpnr`, short for *vpn refresh*, temporarily resets part of the [`ufw`](https://manpages.debian.org/bullseye/ufw/ufw-framework.8.en.html) firewall via [`unfirewall`](#`unfirewall`), refreshes a SurfShark VPN connection, and re-enables a system-wide VPN killswitch via [`firewall`](#`firewall`). In other words, if no additional arguments are called, `vpnr` essentially performs the following behavior in bash-like pseudocode:
```bash
surfshark-vpn disable && unfirewall && surfshark-vpn enable && firewall
```

### Synopsis

**vpnr** [-s -e]

### Options

**-s**
    Make a soft refresh of `surfshark-vpn`, never disabling or turning it off completely
**-e**
    Exit the terminal session after a successful run of the program
`add-res` was the chosen output name for [`gen-res`](#`gen-res`). In simple terms, it uses [`xrandr`](https://x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html) to set resolutions from 50% to 100% (inclusive) of 1440p at a target refresh rate of 144 Hz. This makes it especially useful for games which lack a scaling factor or for injecting AMD FSR 1.0 on games run under Steam Proton.


## `archive-dir`

Originally used for multithreaded [*bzip2*](https://sourceware.org/bzip2/) compression of archives, `archive-dir` was eventually modified to use parallelized [*xz*](https://tukaani.org/xz/) compression instead.

### Synopsis

**archive-dir** *source_directory* *destination*


## `check-iommu`

`check-iommu` pretty prints a system's IOMMU groups, making it useful for verifying PCI devices are separated enough, especially when setting up PCI passthrough. The script itself is lifted from [this subsection](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Ensuring_that_the_groups_are_valid) of the Arch Wiki article for PCI passthrough.


## `cpbak`

`cpbak` creates a `.bak` backup file for every source file called. Due to its narrow scope in usage, the script does not account for directories.

### Synopsis

**cpbak** *source*...


## `eclip`

Usable on any environment running an X session, `eclip` uses [`xclip`](https://linux.die.net/man/1/xclip) to copy a file to the clipboard, verbosely terminating to STDOUT in both cases of a success and failure.

### Synopsis

**eclip** *source*


## `firewall`

When evoked, `firewall` will set up [`ufw`](https://manpages.debian.org/bullseye/ufw/ufw-framework.8.en.html), or Uncomplicated Firewall, to only allow incoming and outgoing traffic via a VPN, thereby acting as a killswitch if the VPN gets disconnected.


## `fmclk`

Short for *force memory clock*, this script would force the AMD RX 580 to run at its highest memory clocks possible. This was a workaround for a bug where the RX 580 would have its memory clocks set to its lowest level possible regardless of context.


## `gen-res`

Hard-coded to work on a 1440p monitor running at 144 Hz, `gen-res` calculates the necessary CVT, or *Coordinated Video Timings*, for a given fraction (divisible by 0.05) of between 50% and 100% inclusive of 1440p's vertical and horizontal resolutions. The resulting calculations then get appended to [`add-res`](#add-res).


## `img-show`

Meant as a rough proof of concept for a later potential photo gallery app made in Python, `img-show` presents an interactive shell, asking the user to select pictures from a new or already chosen directory. It then gives the user 3 options by which to present said images: randomly, by alphabetical order, or by creation time (from oldest to newest).


## `limit-cpu-cores`

If the program detects the system battery is discharging, `limit-cpu-cores` will perform as the name implies and will limit the system to utilize only its first 8 logical threads in an effort to conserve battery life. Otherwise, if the system is not discharging (i.e. is charging or lacks the ability to charge/discharge) and is already thread-limited, it will turn back on all CPU threads.


## `mkram`

Depending on the option selected, `mkram` will either mount or unmount a ramdisk provided that one does not or does exist respectively.

### Synopsis

**mkram** mount [size]
**mkram** umount

### Options

**mount**
    mount a tmpfs ramdisk with optional size (default = 16G)
**umount**
    unmount a ramdisk if one exists


## `pdf2mp3`

`pdf2mp3` will extract the text data from a pdf and, using Google text-to-speech, transform that into an mp3.

### Synopsis

**pdf2mp3** *pdf_file* *mp3_file*


## `rnsong`

Useful for audio files which lack any metadata, `rnsong`, short for *rename song*, will use the album and artist directories the song is contained in as well as the user-supplied track number to add metadata to the song as well as rename the song to a standard format. For instance, if `Lorem Ipsum.mp3` is the 7th track in a given album, it will be renamed to `07 Lorem Ipsum.mp3`. Notably, [`id3v2`](https://www.exiftool.org/TagNames/ID3.html), the program `rnsong` uses to change song metadata, does not work with *opus* nor *aac* files.

Ideally, for `rnsong` to work, the audio file needs to stored as follows.
```
─ <artist>/
   └─ <album>/
       └─ <song>
```

### Synopsis

**rnsong** *audio_file* *track_number*


## `screenshot2text`

Only usable on X11 due to its reliance on [`xclip`](https://linux.die.net/man/1/xclip), `screenshot2text` uses [Tesseract Optical Character Recognition](https://github.com/tesseract-ocr/tesseract) to recognize text from a screenshot and then insert said text to the clipboard, notifying the user once the process is complete.


## `set-card0-auto`

Meant for desktops with an AMD GPU (notably an RX 580 when my main desktop had one), this script sets `card0`'s *dpm*, or *dynamic power management*, back to `auto` (via `power_dpm_force_performance_level`). This is useful whenever [`set-card0-high`](#set-card0-high) is called.


## `set-card0-high`

Like with [`set-card0-auto`](#set-card0-auto), this script is meant for desktops with an AMD GPU. Notably, forcing `card0`'s *dpm* or *dynamic power management* (via `power_dpm_force_performance_level`) to `high` fixes a bug present in many AMD GPUs using the GCN architecture where the screen would artefact when multiple screens of differing refresh rates are connected.


## `sshvm`

`sshvm` allows the user to headlessly access local virtual machines already predefined in the script, including GUI programs if X11 forwarding is enabled. Additionally, for convenience, while the virtual machine is on, the program mounts `/home/<vm_username>` to `/mnt/vm/<vm_hostname>` on the host system.


### Synopsis

**sshvm** *vm_hostname*


## `sys-update`

`sys-update` consolidates the updating and upgrading of packages in the `apt`, `pip3`, and `flatpak` package managers, pretty printing with separators and colors where applicable.


## `unfirewall`

As a complement to [`firewall`](#firewall), `unfirewall` uses [`ufw`](https://manpages.debian.org/bullseye/ufw/ufw-framework.8.en.html) to only enable outgoing traffic, making it useful to re-establish a downed VPN connection, rerunning [`firewall`](#`firewall`) afterwards to reset the VPN killswitch.


## `vpnr`

Meant to work with the `surfshark-vpn` CLI, `vpnr`, short for *vpn refresh*, temporarily resets part of the [`ufw`](https://manpages.debian.org/bullseye/ufw/ufw-framework.8.en.html) firewall via [`unfirewall`](#unfirewall), refreshes a SurfShark VPN connection, and re-enables a system-wide VPN killswitch via [`firewall`](#firewall). In other words, if no additional arguments are called, `vpnr` essentially performs the following behavior in bash-like pseudocode:
```bash
surfshark-vpn disable && unfirewall && surfshark-vpn enable && firewall
```

### Synopsis

**vpnr** [-s -e]

### Options

**-s**
    Make a soft refresh of `surfshark-vpn`, never disabling or turning it off completely
**-e**
    Exit the terminal session after a successful run of the program


## `watch-cpu`

Refreshing every half a second, `watch-cpu` displays the frequency of each thread on the CPU from data gathered from `/proc/cpuinfo`.
