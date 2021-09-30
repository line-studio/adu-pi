# adu-pi

Raspberry Pi 3 base image with Azure Device Update Agent. Compatible with yocto `zeus`.

The [Azure Device Update Agent](https://github.com/Azure/iot-hub-device-update) is included through the the 3rd-party yocto layer [meta-azure-device-update](https://github.com/munit-solutions/meta-azure-device-update).

After cloning the repository run following to get started.

```sh
mkdir -p build/conf
sed "s|###TOPDIR###|$(pwd)|g" meta-adu-pi/conf/local.conf > build/conf/local.conf
sed "s|###TOPDIR###|$(pwd)|g" meta-adu-pi/conf/bblayers.conf > build/conf/bblayers.conf
echo -n password > priv.pass
openssl genrsa -aes256 -passout file:priv.pass -out priv.pem
source poky/oe-init-build-env
bitbake adu-base-image
```

The above will create an image named `adu-base-image-raspberrypi3.wic` which can be written to a SD-card.

To create the update image run `bitbake adu-update-image`. This produces two files `adu-update-image-raspberrypi3.swu` and `adu-update-image-raspberrypi3-manifest.json` (import manifest). These can be uploaded to the Azure Device Update service. The manifest properties can be changed in [local.conf](https://github.com/line-studio/adu-pi/blob/master/meta-adu-pi/conf/local.conf).

## Resources

Additional useful articles and posts.

- [How to build the agent with yocto zeus](https://github.com/Azure/iot-hub-device-update/issues/79)
- [Getting Started with Device Update Agent](https://github.com/Azure/iot-hub-device-update/tree/main/docs/agent-reference)
- [Device Update for Azure IoT Hub tutorial using the Raspberry Pi 3 B+ Reference Image](https://docs.microsoft.com/en-us/azure/iot-hub-device-update/device-update-raspberry-pi)
