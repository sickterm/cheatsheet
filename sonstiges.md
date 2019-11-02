# Editoren benutzen
### VI 

Beenden ``` $:q```		
Beenden ohne Speichern ```$:q!```
Speichern, dann beenden ```$:x```
Speichern ```$:w``` 


### Nano
Nano zeigt einem die Kommandos unten an. Das ^ Zeichen bedeutet STRG. Also ^O heißt STRG+O.


# Arbeiten mit der Konsole
### Allgemeine Kommandos
Prozess abbrechen: CTRL + C
Prozess beenden (wie exit): CTRL + D
ins eigene Homeverzeichnis wechseln: ``cd~``

### $which 
zeigt einem an, wo ein commando ist. Also z.B.
```
$which rename
$/usr/bin/rename
```

### ls
Datein mit page thru auflisten:
``` 
$ls | less
```
die Option -l macht es auführlich? 

### cp
Datei kopieren mit CP

### mv
Dateien verschieben

### rm
remove

### makedir


### Leere Datei erstellen
Dies erstellt eine neue Datei "dateiname.txt":
```
$:> dateiname.txt
```


### Rename
Umbennen von Dateien nach dem Muster:
rename 's/old-name/new-name/' files 

Dieser Befehl benennt "CR2.xmp" in "xmp" um, und zwar bei allen Dateien der Endung "Cr2.xmp"

```rename -n -v ’s/CR2.xmp/xmp/’ *.CR2.xmp```

**n** heißt, er testet nur und macht es nicht wirklich
**v** heißt verbose, also er zeigt mir, was er 
**f** force overwriting existing file

# Wie mache ich bestimmte Dinge?

### Papierkorb löschen
Über die grafische Oberfläche lässt er sich nicht löschen, da hängt sich Linux gerne auf. Daher erfolgt das Löschen über die Konsole:
```
$cd ./local/share
$cd Trash
$du -h
```  
**du -h** bedeutet  disk usage; damit sehe ich wieviel Platz das wegnimmt ob somit, ob es das richtige Verzeichnis ist

```$ rm -rf ~/.local/share/Trash/* ```  **ACHTUNG JETZT IST ALLES WEG**

### Benutzerrechte ändern

Das kann man benutzen:

     $sudo chmod -R -v a+rwx ~/Music/

Damit bekommt jeder alle Rechte; dies sollte nicht benutzt werden! 

    $sickterm@einhorn:~/Ablagefach/USER$ sudo chmod 777 -c -R ~/Ablagefach/USER


### Die Konsole auf Englisch umstellen

Es gibt zwei "bashrc". Einmal im persönlichen Homeverzeichnis ~/.bashrc und einmal im Root verzeichnis. Normalerweise sollten Änderungen nur in dem home Verzeichnis vorgenommen werden. Die Bashrc enthält Befehle, die ausgeführt werden, wenn die Konsole geöffnet wird.

Dem ~/.bashrc mit vi öffnen und folgendes hinzufügen:

    $unset LC_ALL
    $export LC_ALL=en.US.UTF-8

danach die Konsole neu starten oder aber:

    $source ~/.bashrc
    
Und schließlich kontrollieren, welche Sprache ausgegeben werden sollte:    

    $ locale
    LANG=pl_PL.utf8
    LANGUAGE=
    LC_CTYPE="pl_PL.utf8"
    LC_NUMERIC="pl_PL.utf8"
    LC_TIME="pl_PL.utf8"
    LC_COLLATE="pl_PL.utf8"
    LC_MONETARY="pl_PL.utf8"
    LC_MESSAGES=C
    LC_PAPER="pl_PL.utf8"
    LC_NAME="pl_PL.utf8"
    LC_ADDRESS="pl_PL.utf8"
    LC_TELEPHONE="pl_PL.utf8"
    LC_MEASUREMENT="pl_PL.utf8"
    LC_IDENTIFICATION="pl_PL.utf8"
    LC_ALL=
    
    
Warum erst den Befehl $unset LC_ALL?
LC_ALL Overrides individual LC_* settings: if LC_ALL is set, none of the below have any effect.



### Mit Caja Verknüpfungen erstellen
Einfach die Datei per Drag & Drop ziehen und dabei CTRL + Umschalt gedrückt halten




