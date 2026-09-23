# Elise wird 9 – gemeinsame Live-Wunschliste

Damit die Eltern auf **verschiedenen Handys** denselben Status sehen, braucht die GitHub-Pages-Seite einen kleinen gemeinsamen Speicher. Diese Version verwendet **Firebase Realtime Database** und die anonyme Anmeldung.

## 1. Firebase-Projekt erstellen

1. Öffne https://console.firebase.google.com/
2. Erstelle ein neues Projekt, z. B. **elise-wird-9**.
3. Öffne **Build → Realtime Database → Create Database**.
4. Wähle eine Region in Europa, z. B. `europe-west1`.
5. Für die Regeln zunächst den geschützten Modus verwenden.

## 2. Anonyme Anmeldung einschalten

Öffne **Build → Authentication → Sign-in method**.
Aktiviere **Anonymous**.

## 3. Web-App registrieren

Im Firebase-Projekt:
**Project settings → Your apps → Web-App hinzufügen (`</>`)**.

Firebase zeigt anschließend eine Konfiguration mit:
`apiKey`, `authDomain`, `databaseURL`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`.

Diese Werte in `index.html` in `firebaseConfig` eintragen.

## 4. Realtime-Database-Regeln

Unter **Realtime Database → Rules** diese Regeln einsetzen:

```json
{
  "rules": {
    "gifts": {
      "$gift": {
        ".read": "auth != null",
        ".write": "auth != null && newData.hasChildren(['name','updatedAt']) && newData.child('name').isString() && newData.child('name').val().length <= 60"
      }
    }
  }
}
```

Damit können nur angemeldete Nutzer die Geschenkstatus lesen oder ändern. Die Anmeldung erfolgt anonym; es werden keine E-Mail-Adressen benötigt.

## 5. GitHub Pages

Die fertige `index.html` in dein Repository **PartyParty** hochladen und die bisherige `index.html` ersetzen.

Danach unter:
**Settings → Pages → Deploy from a branch → main → / (root) → Save**

## Ergebnis

Wenn Mama ein Geschenk übernimmt, wird z. B. angezeigt:

**✓ Übernommen von Mama**

Öffnet Papa die Seite auf seinem Handy, sieht er denselben Status. Änderungen werden live über Firebase synchronisiert.

### Hinweis

Die Seite ist technisch öffentlich erreichbar, aber die Namen der Personen, die Geschenke übernehmen, werden in der Datenbank gespeichert. Verwende deshalb am besten nur Vornamen oder Spitznamen.
