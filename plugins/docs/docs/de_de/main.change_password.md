# Passwort ändern

## Im Backend

Nutzern können vom Administrator jederzeit ein neues Passwort vergeben werden. 

Es wird jedoch empfohlen, Passwörter vom Nutzer selbst festlegen zu lassen, um Datenpannen aus dem Weg zu gehen.

## Im Frontend

Nutzer können selbständig das Passwort ändern.

1. In der Struktur einen Artikel anlegen, z.B. `Passwort ändern`
2. Im Editiermodus des Artikels auf der rechten Seite die passenden Berechtigungen einstellen (Sichtbar nur für eingeloggte Nutzer)
3. Das Modul `YForm Formbuilder` hinzufügen und folgendes Formular eintragen

```
ycom_auth_password|password|Ihr Passwort:*|{"length":{"min":6},"letter":{"min":1},"lowercase":{"min":0},"uppercase":{"min":0},"digit":{"min":1},"symbol":{"min":0}}|Das Passwort muss mindestens 6 Zeichen lang sein und mindestens eine Ziffer enthalten
password|password_2|Passwort wiederholen:||no_db
validate|empty|password|Bitte geben Sie ein Passwort ein.
validate|compare|password|password_2|!=|Bitte geben Sie zweimal das gleiche Passwort ein
action|showtext|Ihre Daten wurden aktualisiert. Das neue Passwort ist ab sofort aktiv.|||1
action|ycom_auth_db
hidden|new_password_required|0
```

4. Den Artikel verlinken, damit die Nutzer die Seite zum Ändern des Passworts aufrufen können.

Nun können Nutzer ihr Passwort selbständig ändern.

> Tipp: Dieses Formular kann auch dazu genutzt werden, um Nutzer ein neues Passwort festlegen zu lassen, wenn sie ihr Passwort vergessen haben. Weitere Infos in der Doku unter `Passwort vergessen`

## Zusätzliche Prüfung auf das alte Passwort

1. Feld für altes Passwort und Validierung hinzufügen

```
password|old_password|Altes Passwort||no_db
validate|empty|old_password|Bitte altes Passwort angeben.
validate|ycom_auth_password|old_password|Das alte Passwort ist fehlerhaft!
```

2. Neues Passwort darf nicht dem alten Passwort entsprechen

Dafür noch folgende Validierung hinzufügen

```
validate|compare|password|old_password|==|Das neue Passwort darf nicht dem alten Passwort entsprechen.
```

Das ganze Formular sieht dann wie folgt aus:
```
password|old_password|Altes Passwort||no_db
ycom_auth_password|password|Ihr Passwort:*|{"length":{"min":6},"letter":{"min":1},"lowercase":{"min":0},"uppercase":{"min":0},"digit":{"min":1},"symbol":{"min":0}}|Das Passwort muss mindestens 6 Zeichen lang sein und mindestens eine Ziffer enthalten
password|password_2|Passwort wiederholen:||no_db

validate|empty|old_password|Bitte altes Passwort angeben.
validate|ycom_auth_password|old_password|Das alte Passwort ist fehlerhaft!
validate|compare|password|old_password|==|Das neue Passwort darf nicht dem alten Passwort entsprechen.
validate|empty|password|Bitte geben Sie ein Passwort ein.
validate|compare|password|password_2|!=|Bitte geben Sie zweimal das gleiche Passwort ein
action|showtext|Ihre Daten wurden aktualisiert. Das neue Passwort ist ab sofort aktiv.|||1
action|ycom_auth_db
hidden|new_password_required|0
```


