# blkid
Rausfinden, welche UUIDs Block Devices haben: `blkid`

# crypttab und fstab
`/etc/crypttab`: Verschlüsselte UUIDs zu dm-crypt Namen
`/etc/fstab`: Welche Partitionen gemountet werden

z.B.: `/etc/crypttab`:
```
crypt-home UUID=c535d014-252a-4a72-809d-ae51f1e5e5e2 none luks,tries=99
```

z.B.: `/etc/fstab`:
```
UUID=34cefc1f-c51e-4d5b-b55a-30235869d43a   /               ext4    noatime         0 0
/dev/mapper/crypt-home                      /home           ext4    noatime         0 0
```
