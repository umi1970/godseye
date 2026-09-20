# Setup – God's Eye View

Dieses Repository ist eine lauffähige Kopie von
[bilawalsidhu/gods-eye-view](https://github.com/bilawalsidhu/gods-eye-view) (MIT).
Die App läuft komplett lokal im Browser – **ohne API-Keys**.

## Voraussetzung

**Node.js 24.14+ oder 26.x** (Node 22 reicht nicht, Node 25 ist EOL).

```bash
node -v   # muss v24.14.0 oder neuer sein
```

Falls zu alt, z. B. per nvm:

```bash
nvm install 24
nvm use 24
```

## Starten

```bash
npm ci
npm run doctor   # prüft Node, Abhängigkeiten und Provider-Status
npm run dev
```

Dann **http://localhost:4173** öffnen und im ersten Panel eine Mission wählen:
*Live Contacts*, *Space Missions*, *Environmental* oder *Explore Manually*.

## Windows

```powershell
git clone -b claude/gods-eye-view-install-70dzxa https://github.com/umi1970/godseye.git
cd godseye
node -v            # muss v24.14.0+ sein
npm ci
npm run dev
```

Node 24 installieren, falls nötig: `winget install OpenJS.NodeJS.LTS`
(danach Terminal neu öffnen) oder von https://nodejs.org.

### Fehler: `Cannot find module @rollup/rollup-win32-x64-msvc`

Bekannter npm-Bug (npm/cli#4828): npm überspringt beim Installieren die
optionale Plattform-Binary. Das Lockfile ist korrekt, es fehlt nur lokal.

Reihenfolge zum Beheben, in PowerShell im Projektordner:

```powershell
Remove-Item -Recurse -Force node_modules
npm cache clean --force
npm ci
```

Hilft das nicht, die Binary direkt nachinstallieren (Lockfile bleibt unberührt):

```powershell
npm i @rollup/rollup-win32-x64-msvc@4.62.0 --no-save
npm run dev
```

Letzte Option, falls beides scheitert - löst die Versions-Pins auf:

```powershell
Remove-Item -Recurse -Force node_modules
Remove-Item -Force package-lock.json
npm install
```

### Weitere Windows-Stolpersteine

- **OneDrive:** Liegt das Projekt unter `Dokumente`, synchronisiert OneDrive
  `node_modules` mit und kann Dateien während der Installation verschieben.
  Projekt besser nach `C:\dev\godseye` legen oder den Ordner in OneDrive
  von der Synchronisierung ausschließen.
- **Virenscanner:** Echtzeitschutz kann neu entpackte `.node`-Binaries
  blockieren. Projektordner ggf. ausnehmen.
- **Port 4173 belegt:** `npm run dev -- --port 5173`.

## API-Keys (optional)

Keys sind Upgrades, keine Voraussetzung. Ohne Keys laufen bereits: Esri-Satellitenbilder,
Terrain, Flüge, Militärverkehr, Satelliten, Erdbeben, öffentliche Kameras, Radio und Starts.

Keys werden **in der App** eingetragen: Chip **POWER UP** unten rechts →
*Provider Settings* → einfügen → **SAVE KEYS**. Die App startet sich selbst neu.
Gespeichert wird in der `.env` im Repo-Root (von Git ausgeschlossen, nur für den Besitzer lesbar).

Lohnt sich zuerst:

| Key | Schaltet frei |
| --- | --- |
| Cesium ion Token (kostenlos, privat/nicht-kommerziell) | Photorealistisches 3D + World Terrain |
| Google Maps Key (abrechnungspflichtig) | Google 3D Tiles + Ortssuche |
| OpenAI Key | Sprachsteuerung ("Talk to it") |
| AISStream / FIRMS / TomTom / OpenSky | Schiffe, Brände, Verkehr, mehr Flugdaten |

Details und Kosten: [README.md](README.md) → *Keys & Costs*, Sicherheitshinweise in
[SECURITY.md](SECURITY.md).

## Nützliche Befehle

```bash
npm run build      # Produktions-Build nach dist/
npm run preview    # Build lokal ausliefern
npm test           # Unit-Tests
npm run format     # Prettier
```

## Verifiziert

Auf Node 24.21.0 geprüft: `npm ci`, `npm run doctor` ("Ready"),
`npm run build`, `npm test` (4158 Tests, 0 Fehler) und Start des Dev-Servers
mit gerendertem HUD unter `http://localhost:4173`.
