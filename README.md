# Netgear DM200 Bridge ADSL/VDSL transparent sur réseau Orange

Fork du projet : https://github.com/a1comms/openwrt-netgear-dm200-bridge

## Interface de gestion du modem

Le firmware récupère une adresse IP sur le VLAN eth0.200, prévoir donc un service DHCP de ce côté.

Sinon modifier le fichier "files/etc/config/network" en remplaçant la ligne de l'interface "mgmt" :

```
        option proto 'dhcp'
```

par :

```
        option proto 'static'
        option ipaddr '192.168.x.x'
        option netmask '255.255.255.0'
```

## Création de l'image

Récupérer et extraire le projet "image builder" : http://downloads.openwrt.org/releases/19.07.2/targets/lantiq/xrx200/openwrt-imagebuilder-19.07.2-lantiq-xrx200.Linux-x86_64.tar.xz

Récupérer le firmware à jour ("DM200-V1.0.0.61.img") du Netgear DM200 : https://www.netgear.com/support/product/DM200

Extraire le modem du firmware d'origine

```
7z e DM200-V1.0.0.61.img dsl_vr9_firmware_xdsl-05.07.0B.05.00.07_05.07.05.04.00.01.bin
```

Copier le fichier "dsl_vr9_firmware_xdsl-05.07.0B.05.00.07_05.07.05.04.00.01.bin" dans "files/lib/firmware/dsl_vr9_firmware_xdsl-05.07.0B.05.00.07_05.07.05.04.00.01.bin"

Copier le dossier "files" à la racine du projet "image builder"

Lancer la commande

```
make image FILES=files/ PROFILE=netgear_dm200 PACKAGES="-dsl-vrx200-firmware-xdsl-a -dsl-vrx200-firmware-xdsl-b-patch luci-mod-admin-full luci-app-statistics collectd-mod-uptime luci-proto-ipv6 luci-proto-ppp luci-theme-bootstrap uhttpd"
```
