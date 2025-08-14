# 📡 Step 2 – UHD Drivers Installation for USRP B210

In this step, we install and configure the **UHD (USRP Hardware Driver)** to enable communication between the **USRP B210** and **srsRAN** (👉 B210 act as a tower or Base stations in our 5g setup).


## Install dependencies

```bash
sudo apt-get install cmake make gcc g++ pkg-config libfftw3-dev libmbedtls-dev libsctp-dev libyaml-cpp-dev libgtest-dev
```

## Install UHD drivers

```bash
sudo add-apt-repository ppa:ettusresearch/uhd
sudo apt-get update
sudo apt-get install libuhd-dev uhd-host
```

## Installing the reqired images for USRP B210.

```bash
sudo uhd_images_downloader
```

## Now Connect the B210 device to PC in which you want to install srsRAN and check if your device detects B210

```bash
uhd_find_devices
```
```bash
uhd_usrp_probe
```

It will print the device if B210 is detected.
