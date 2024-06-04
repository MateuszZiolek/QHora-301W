# My Experiments with QNAP QHora-301W

You can find more information about the QNAP QHora-301W [here](https://www.qnap.com/en/product/qhora-301w).

For several years, I have been using the QHora-301W primarily as a router and access point. The early years were challenging, as the initial software versions were cumbersome. However, after numerous service reports and updates, it eventually started to work smoothly.

At home, I use a GPON 2Gbps connection connected to a 10Gb WAN. From the second 10Gb port, I have a MikroTik CRS310-8G+2S+ connected. More details about this MikroTik device can be found [here](https://mikrotik.com/product/crs310_8g_2s_in#fndtn-specifications).

Everything runs stably, achieving a full 2Gbps download and 600Mbps upload.

## Enabling SSH on QHora

To this day, I have been missing the ability to automatically enable SSH after a restart, but I finally think I have a solution. To do this, we need to enable SSH using the known method:

> How to open Qhora SSH function -> Default SSH is disabled, please hold and keep the WPS button (around 12 seconds) until you hear "bi" twice to enable SSH. Then you can use the PuTTY tool to SSH into the QHora device. Log in to the terminal via SSH using the utility, e.g., PuTTY - IP/port: 192.168.100.1 / 22200 - Login name/password: admin / the MAC Address of QHora.

Information sourced from [this forum thread](https://forum.openwrt.org/t/adding-openwrt-support-for-qnap-qhora-301w/96934/53).

### Permanently Enabling SSH on Startup
Everything is based on SW version 2.4.0.190 build 20240522:

To permanently enable SSH after startup:

1. Log in as admin via SSH.
2. Run the following command to switch to the root user:
    ```bash
    sudo su
    ```
3. Edit the system settings file:
    ```bash
    vi /etc/system/system_setting.json
    ```
4. Change the following line:
    > "enable_ssh": false 
    to 
    > "enable_ssh": true
5. Reboot the device:
    ```bash
    reboot
    ```
After the restart, SSH should be permanently enabled.

> I discovered this method today, so I am not sure how long it will work or if it might get disabled automatically.

Additionally, I executed the following command:
    ```bash
    /etc/init.d/dropbear enable
    ```
## TODO

1. **Running Node Exporter** - I know how to do it, but I need to add automatic deployment from the local host based on Docker. After a restart, the binary disappears. Another option would be to find the autostart and add the node.
2. **Finding where and how statistics are sent** - After joining QWAN, we have the option to create our own organization and get access to [QWAN](https://quwan.qnap.com/). There, we have beautiful statistics and other features that are not available locally.
