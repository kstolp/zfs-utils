## Basic TPM2 key creation for unlocking ZFS

ZFS does not support multiple key slots (like LUKS2), so we will reuse
the regular passphrase, which we will encrypt with TPM2.

This encrypted file will then be bundled with the initramfs image for
automatic unlocking of the zpool during boot.

    cd /etc/zfs
    echo -n "mypassphrase" > zfs.key
    systemd-creds encrypt --with-key=tpm2 --tpm2-device=auto \
        --name=zfs-key zfs.key zfs-key.creds
    rm zfs.key
    chmod 600 /etc/zfs/zfs-key.creds

Verify that it works:

    systemd-creds decrypt --name=zfs-key /etc/zfs/zfs-key.creds

Sealing the key using various TPM2 PCRS is beyond the scope of this guide.
