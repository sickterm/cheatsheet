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

# Festplatte prüfen
Wenn Lesefehler sind, üblicherweise im `dmesg` Log Einträge:

```
# dmesg
[...]
[348233.363982] ata2: EH complete
[348287.391241] ata2.00: exception Emask 0x0 SAct 0x38000000 SErr 0x0 action 0x0
[348287.391252] ata2.00: irq_stat 0x40000008
[348287.391260] ata2.00: failed command: READ FPDMA QUEUED
[348287.391276] ata2.00: cmd 60/00:d8:00:b9:4c/01:00:01:00:00/40 tag 27 ncq dma 131072 in
                         res 41/40:00:60:b9:4c/00:00:01:00:00/40 Emask 0x409 (media error) <F>
[348287.391284] ata2.00: status: { DRDY ERR }
[348287.391290] ata2.00: error: { UNC }
[348287.392501] ata2.00: configured for UDMA/133
[348287.392533] sd 1:0:0:0: [sdb] tag#27 FAILED Result: hostbyte=DID_OK driverbyte=DRIVER_SENSE
[348287.392539] sd 1:0:0:0: [sdb] tag#27 Sense Key : Medium Error [current] 
[348287.392545] sd 1:0:0:0: [sdb] tag#27 Add. Sense: Unrecovered read error - auto reallocate failed
[348287.392552] sd 1:0:0:0: [sdb] tag#27 CDB: Read(16) 88 00 00 00 00 00 01 4c b9 00 00 00 01 00 00 00
[348287.392556] print_req_error: I/O error, dev sdb, sector 21805408
```

SMART Test triggern (als root):

```
# smartctl -t short /dev/sdx
```

SMART Ergebnis lesen (auch als root):
```
# smartctl -a /dev/sdx
[...]
SMART Self-test log structure revision number 1
Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
# 1  Short offline       Completed without error       00%        18         -
```

Bei Fehler steht hier so etwas:

```
# smartctl -a /dev/sdx
[...]
Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
# 1  Short offline       Completed: read failure       90%     11530         7383952
```
