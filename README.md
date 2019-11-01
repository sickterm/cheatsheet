# Partitionieren und Dateisysteme
Festplatte: `/dev/sdx` (Block Device)

Partition: `/dev/sdx1`, `/dev/sdx2` (auch Block Device)

Partitionen erstellen (partitionieren): `fdisk`, am Besten für alle Platten GPT
verwenden, nicht MBR (funktioniert dann auch für Platten größer als 2 TiB)

Dateisystem erstellen (formatieren): z.B. `mkfs.ext4`

Alle Block Devices im System anzeigen: `cat /proc/partitions`

# SATA-hotplug neue Platten scannen
`# scsi_rescan`

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

# LUKS
(Alles hier als root)

LUKS Container initial erstellen:
```
# cryptsetup luksFormat /dev/sdx1

WARNING!
========
This will overwrite data on /dev/sdx1 irrevocably.

Are you sure? (Type uppercase yes): 
Enter passphrase for /dev/sdx1: 
Verify passphrase: 
```

LUKS Header anzeigen:
```
# cryptsetup luksDump /dev/sdx1
LUKS header information
Version:       	2
Epoch:         	6
Metadata area: 	16384 [bytes]
Keyslots area: 	16744448 [bytes]
UUID:          	ffbe6fff-b2ac-4635-b744-80448f2bed10
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)
[...]
Keyslots:
  1: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2i
	Time cost:  5
	Memory:     1048576
	Threads:    4
	AF stripes: 4000
	AF hash:    sha256
	Area offset:290816 [bytes]
	Area length:258048 [bytes]
	Digest ID:  0
```

LUKS Passwort ändern:
```
# cryptsetup luksChangeKey /dev/sdx1
Enter passphrase to be changed: [altes Passwort]
Enter new passphrase: [neues Passwort]
Verify passphrase: [nochmal neues Passwort]
```

In einem USB Stick ein LUKS Volumen öffnen:
```
# cryptsetup luksOpen /dev/sdx1 mein-crypto
Enter passphrase for /dev/sdx1: 
```

Wenn Passwort korrekt war, gibt es jetzt `/dev/mapper/mein-crypto`, das z.B.
normal gemountet werden kann:

```
# mount /dev/mapper/mein-crypto /tmp/mountpoint
```

LUKS device *komplett löschen* (alle Daten sind weg, ACHTUNG!):
```
# cryptsetup erase /dev/sdx1

WARNING!
========
This operation will erase all keyslots on device /dev/sdx1.
Device will become unusable after this operation.

Are you sure? (Type uppercase yes):
```

# Devices komplett löschen
Funktioniert mit allen Block devices, Partitionen, z.B. USB-Sticks oder
SD-Karten. Wichtig, wenn die unverschlüsselt waren:

```
# dd if=/dev/zero of=/dev/sdx bs=1M
```

Überschreibt das *komplette* Device /dev/sdx (also alle Partitionen). Wenn man
nur eine Partition nehmen will, dann eben /dev/sdx1 oder /dev/sdx2 usw.
*ACHTUNG* es gibt keine Bestätigung oder so, bei ENTER sind die Daten futsch.

Wenn man nur den Header löschen will, kann man die Größe auch einschränken,
z.B:

```
# dd if=/dev/zero of=/dev/sdx bs=1M count=1024
```

Löscht die ersten 1024 MiB von /dev/sdx (z.B. also die komplette
Partitionstabelle).

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

# Ubuntu updaten
Erstmal das System so wie es ist aktuell bringen:

```
# apt-get update
# apt-get dist-upgrade
```

Dann Neubooten und Editieren von `/etc/apt/sources.list`:

```
deb http://ftp.halifax.rwth-aachen.de/ubuntu/ disco main restricted
```

Aktuelle Distribution `disco` -- ändern auf das neue Distribution (z.B.
`eoan`). *Alle* Referenzen auf `disco` zum Neuen ändern. Dann:

```
# apt-get update
# apt-get dist-upgrade
```
