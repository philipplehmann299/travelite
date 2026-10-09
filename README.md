# travelite – umzug auf supabase + github + hosting

dateien: `index.html` (die app), `schema.sql` (datenbank + sicherheitsregeln)

## 1. supabase einrichten (ca. 10 min)
1. **sql editor > new query**: kompletten inhalt von `schema.sql` einfügen und **run**. fehlerfrei = tabellen, regeln und echtzeit sind da.
2. **authentication > providers**: *email* aktiv lassen. zum ersten testen kannst du unter *sign in / providers > email* „confirm email“ ausschalten. später wieder an und eigenen smtp-versand einrichten (der eingebaute ist stark begrenzt).
3. **project settings > api**: kopiere die **project url** und den **anon public key**.
   - den **service_role key niemals** in die app oder auf github legen. der anon key darf öffentlich sein, weil die regeln aus `schema.sql` den zugriff schützen.

## 2. werte in die app eintragen
in `index.html` ganz unten, zwei zeilen suchen und ersetzen:
```
const SB_URL='HIER_PROJECT_URL_EINTRAGEN',SB_KEY='HIER_ANON_PUBLIC_KEY_EINTRAGEN';
```

## 3. auf github legen
neues repository > **add file > upload files** > `index.html` hochladen (README und schema.sql dürfen mit) > commit.

## 4. veröffentlichen – du brauchst nur *eine* der beiden varianten
- **github pages (keine neue registrierung):** repository > settings > pages > branch `main`, ordner `/ (root)`. die adresse lautet `https://DEINNAME.github.io/REPONAME/`. kostenlos für öffentliche repositories (privat nur mit bezahltem plan).
- **cloudflare pages:** erst hier registrieren, wenn du das repository privat halten willst oder eine eigene domain möchtest. workers & pages > create > pages > connect to git > repository wählen, build command leer lassen, output directory `/`.

## 5. adresse in supabase hinterlegen
**authentication > url configuration**: *site url* = deine veröffentlichte adresse (und unter *redirect urls* ebenfalls eintragen), sonst führen bestätigungs-mails ins leere.

## 6. testen
adresse öffnen > registrieren > profil anlegen > reise erstellen > code oder einladungslink an eine zweite person geben.

## wichtig
- daten aus der claude-version werden **nicht automatisch** übernommen. testdaten einfach neu anlegen.
- passnummern: lieber nicht eintragen. profile sind für alle mitreisenden lesbar, die oberfläche zeigt sie nur dem travellead.
- rechte (travellead/traveller) werden jetzt von der datenbank erzwungen. nicht abgesichert sind noch einzelne eintragstypen: mitglieder können eintragen anderer mitglieder derselben reise ändern.
- impressum/datenschutz: im impressum fehlen noch deine angaben; bei öffentlichem betrieb ist eine datenschutzerklärung nötig.
- bisher fehlen: abmelden-knopf, passwort-zurücksetzen.
