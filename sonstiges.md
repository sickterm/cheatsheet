#Editoren benutzen
##VI 

Beenden
```
$:q
```		

Beenden ohne Speichern
```
$:q!	
```

Speichern, dann beenden
```
$:x 
```

Speichern 
```
$:w
``` 


##Nano
Nano zeigt einem die Kommandos unten an. Das ^ Zeichen bedeutet STRG. Also ^O heißt STRG+O.


#Arbeiten mit der Konsole
Prozess abbrechen: CTRL + C

Prozess beenden (wie exit): CTRL + D

##$which 
zeigt einem an, wo ein commando ist. Also z.B.
```
$which rename
$/usr/bin/rename
```

ins eigene Home verzeichnis wechseln: cd ~

##ls
Datein mit page thru auflisten:
``` 
$ls | less
```
die Option -l macht es auführlich? 

##cp
Datei kopieren mit CP

##mv
Dateien verschieben

##rm
remove

##makedir


##Leere Datei erstellen
Dies erstellt eine neue Datei "dateiname.txt":
```
$:> dateiname.txt
```


##Rename
Umbennen von Dateien nach dem Muster:
rename 's/old-name/new-name/' files 

Dieser Befehl benennt "CR2.xmp" in "xmp" um, und zwar bei allen Dateien der Endung "Cr2.xmp"

```rename -n -v ’s/CR2.xmp/xmp/’ *.CR2.xmp```
n heißt, er testet nur und macht es nicht wirklich
v heißt verbose, also er zeigt mir, was er 
f force overwriting existing file










