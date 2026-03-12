# SANE Magicolor Backend – Ricoh SP 204SF / SP 204SFN Support

Support for **Ricoh SP 204SF** and **Ricoh SP 204SFN** scanners in the **SANE Magicolor backend**.

This repository provides an implementation that enables scanning support for these devices on Linux systems.

It was created to help users quickly build **SANE with working support** for these scanners, especially on distributions where the update has not yet been released.

The changes have been contributed **upstream to the SANE project**.

---

# Supported devices

The following devices are supported:

- Ricoh **SP 204SF**
- Ricoh **SP 204SFN**

Features:

- USB scanning
- Network scanning (SP 204SFN)
- Automatic device discovery via **SNMP**
- Compatible with standard SANE applications

Example applications:

- xsane
- naps2
- scanimage
- gscan2pdf
- simple-scan

---

# Install dependencies

Install required packages first:

```bash
sudo apt update
sudo apt install sane-utils libsane1 xsane
```

Optional GUI tools:

```bash
sudo apt install naps2
```

---

# Build and install

Generate build files:

```bash
autoreconf -ivf
```

Configure:

```bash
./configure --sysconfdir=/etc
```

Compile:

```bash
make -j$(nproc)
```

Install:

```bash
sudo make install
```

Update shared libraries:

```bash
sudo ldconfig
```

Restart SANE service:

```bash
sudo systemctl restart saned.socket
```

---

# Verify scanner detection

Check whether the scanner is detected:

```bash
scanimage -L
```

Expected output example:

```
device `magicolor:net:<IP_ADDRESS>?model=0x43e' is a Konica Minolta Ricoh SP 204SF/204SFN multi-function peripheral
```

You can also test scanning using:

```bash
xsane
```

or

```bash
naps2
```

---

# Network scanning (SP 204SFN)

The **Ricoh SP 204SFN** supports network scanning.

The backend can automatically detect the device using **SNMP discovery** if:

- the scanner is powered on
- the scanner is on the same network
- SNMP is not blocked by the firewall

In most cases **no manual configuration is required**.

## Manual configuration

If the scanner is **not detected automatically**, you can manually add it in the Magicolor backend configuration file:

```
/etc/sane.d/magicolor.conf
```

Add the following line:

```
net <IP_ADDRESS>
```

After editing the configuration file, test scanner detection again:

```bash
scanimage -L
```

---

# Debugging

If the scanner is not detected, run the backend in debug mode:

```bash
SANE_DEBUG_MAGICOLOR=255 scanimage -L
```

This will print detailed debug output showing:

- network discovery attempts
- backend communication
- device detection

---

# Troubleshooting

## Scanner not detected

Check network connectivity:

```bash
ping <IP_ADDRESS>
```

Then test detection again:

```bash
scanimage -L
```

---

## Old distribution packages

Some Linux distributions ship **older SANE versions** without support for these scanners.

In that case building from this repository will enable support.

---

# License

Same license as the upstream **SANE backends project**.
