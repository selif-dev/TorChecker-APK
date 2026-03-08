# 🧅 Tor Checker

> **IT** · App Android educativa per testare la connessione Tor tramite Orbot. Scopri cosa funziona davvero e cosa può ancora esporre il tuo IP.
> **EN** · Educational Android app to test your Tor connection via Orbot. Discover what actually works and what can still expose your IP.

---

## 📱 Screenshots

| Dark | Light |
|------|-------|
| *(coming soon)* | *(coming soon)* |

---

## ⚠️ Scopo / Purpose

> **IT** · Questo è uno strumento **didattico**. I risultati mostrano lo stato della tua connessione Tor in quel momento, ma non garantiscono anonimato completo. Anche con Orbot attivo alcune perdite (es. WebRTC/UDP) sono possibili.
>
> **EN** · This is an **educational** tool. Results show the state of your Tor connection at that moment, but do not guarantee complete anonymity. Even with Orbot active, some leaks (e.g. WebRTC/UDP) are still possible.

---

## ✅ Controlli / Checks

| Check | IT | EN |
|-------|----|-----|
| 🔐 **Orbot Status** | Verifica se il proxy SOCKS5 è attivo sulla porta 9050 | Checks if the SOCKS5 proxy is active on port 9050 |
| 🌐 **IP via Proxy Tor** | Confronta IP Tor vs IP reale, con geolocalizzazione e latenza | Compares Tor exit IP vs real IP, with geolocation and latency |
| 🔍 **DNS Leak Test** | Verifica se il DNS esce fuori da Tor | Checks if DNS resolves outside Tor |
| ✅ **TorProject Check** | Verifica ufficiale via `check.torproject.org/api/ip` | Official check via `check.torproject.org/api/ip` |
| 🧅 **Indirizzo .onion** | Raggiungibilità di un indirizzo .onion personalizzabile | Reachability of a custom .onion address |
| 📡 **WebRTC Leak Test** | Rileva perdite IP via STUN/UDP che bypassano Orbot | Detects IP leaks via STUN/UDP that bypass Orbot |

---

## 🔀 Modalità di connessione / Connection modes

L'app supporta due modalità selezionabili, con banner informativo che riflette lo stato combinato con la VPN:

| Modalità | VPN Orbot | Comportamento |
|----------|-----------|---------------|
| Proxy Tor | ✗ | HTTP via SOCKS5 (9050), UDP esposto |
| Proxy Tor | ✓ | Doppia copertura Tor, UDP bloccato dalla VPN |
| Diretto | ✓ | TCP via Tor a livello OS, UDP bloccato dalla VPN |
| Diretto | ✗ | Nessuna protezione — baseline per vedere l'IP reale |

The app supports two selectable modes, with an informative banner that reflects the combined state with VPN:

| Mode | Orbot VPN | Behavior |
|------|-----------|----------|
| Tor Proxy | ✗ | HTTP via SOCKS5 (9050), UDP exposed |
| Tor Proxy | ✓ | Dual Tor coverage, UDP blocked by VPN |
| Direct | ✓ | TCP through Tor at OS level, UDP blocked by VPN |
| Direct | ✗ | No protection — baseline to see real IP |

> Il chip **VPN** distingue tre stati: VPN Orbot attiva (verde), VPN non-Orbot (arancione), nessuna VPN (rosso).
>
> The **VPN** chip distinguishes three states: Orbot VPN active (green), non-Orbot VPN (orange), no VPN (red).

---

## 🌍 Geolocalizzazione e latenza / Geolocation and latency

La card IP mostra per entrambi gli IP (reale e Tor):
- **Flag + Città, Paese** tramite `ipapi.co` (fallback: `ip-api.com`)
- **Latenza** della richiesta in ms
- **Refresh manuale** con icona `↻` nell'header della card — aggiorna IP reale e Tor IP senza rieseguire tutti i check
- **Cache geo**: se l'IP non cambia, la geolocalizzazione non viene ri-fetcha

Se la VPN Orbot è attiva al momento del fetch dell'IP reale, l'app mostra un avviso ⚠️ poiché l'IP rilevato potrebbe essere quello del nodo di uscita Tor anziché il tuo IP reale.

The IP card shows for both IPs (real and Tor):
- **Flag + City, Country** via `ipapi.co` (fallback: `ip-api.com`)
- **Request latency** in ms
- **Manual refresh** via `↻` icon in the card header — updates real and Tor IP without re-running all checks
- **Geo cache**: if the IP hasn't changed, geolocation is not re-fetched

If Orbot VPN is active when the real IP is fetched, the app shows a ⚠️ warning since the detected IP may be the Tor exit node rather than your real IP.

---

## 📊 Punteggio privacy / Privacy score

Dopo aver eseguito i check, l'app mostra un riepilogo compatto:
- **`4/6 check OK`** con colore verde / arancione / rosso in base ai risultati
- **Timestamp** "Ultimo controllo: 14:32" sotto il bottone
- **Re-run singolo**: ogni card mostra `↻` ed è tappabile per rieseguire solo quel check

After running checks, the app shows a compact summary:
- **`4/6 checks OK`** colored green / orange / red based on results
- **Timestamp** "Last check: 14:32" below the button
- **Single re-run**: each card shows `↻` and can be tapped to re-run that check individually

---

## 🔔 Notifiche / Notifications

L'app invia una notifica push quando rileva che il **proxy SOCKS5** o la **VPN Orbot** vengono disattivati mentre l'app è in esecuzione in background.

The app sends a push notification when it detects that the **SOCKS5 proxy** or **Orbot VPN** has been deactivated while the app is running in the background.

---

## 🎨 Temi / Themes

| Tema | Descrizione / Description |
|------|--------------------------|
| ☀️ **Chiaro / Light** | Tema chiaro con accenti viola |
| 🌙 **Scuro / Dark** | Tema scuro con accenti viola (default) |
| 📱 **Sistema / System** | Segue le impostazioni Android |

---

## ⚙️ Come funziona / How it works

In **modalità Proxy Tor**, le richieste HTTP passano attraverso il proxy **SOCKS5 di Orbot** su `127.0.0.1:9050`, implementato tramite il package `socks5_proxy`.

In **Tor Proxy mode**, HTTP requests route through **Orbot's SOCKS5 proxy** on `127.0.0.1:9050`, implemented via the `socks5_proxy` package.

In **modalità Diretta**, le richieste vanno senza proxy. Se la VPN Orbot è attiva, il kernel Android instrada comunque tutto il TCP via Tor tramite l'interfaccia `tun0`.

In **Direct mode**, requests go without a proxy. If Orbot VPN is active, the Android kernel still routes all TCP through Tor via the `tun0` interface.

Il **WebRTC Leak Test** invia un pacchetto STUN via **UDP diretto** (bypassando qualsiasi proxy) verso `stun.cloudflare.com` e `stun.stunprotocol.org`. Con VPN Orbot attiva, l'UDP viene bloccato dal tunnel e il test torna verde.

The **WebRTC Leak Test** sends a STUN packet via **direct UDP** (bypassing any proxy) to `stun.cloudflare.com` and `stun.stunprotocol.org`. With Orbot VPN active, UDP is blocked by the tunnel and the test returns green.

Il **rilevamento VPN** distingue Orbot dalle VPN di terze parti verificando che l'interfaccia `tun` sia presente *e* che la porta 9050 risponda.

**VPN detection** distinguishes Orbot from third-party VPNs by verifying that a `tun` interface is present *and* that port 9050 responds.


---

## 🔏 Privacy

L'app non raccoglie né trasmette dati personali. Le richieste di geolocalizzazione IP vengono effettuate verso `ipapi.co` tramite connessione diretta (non proxy). Vedi la [Privacy Policy](./privacy_policy.html).

This app does not collect or transmit personal data. IP geolocation requests are made to `ipapi.co` via direct connection (not proxied). See the [Privacy Policy](./privacy_policy.html).

---

## ⚠️ Note / Notes

- Orbot deve essere avviato prima di eseguire i controlli / Orbot must be running before checks
- I siti `.onion` possono impiegare 30+ secondi per rispondere / `.onion` sites may take 30+ seconds
- Con VPN Orbot attiva, l'UDP è bloccato — il WebRTC leak test torna sempre verde / With Orbot VPN active, UDP is blocked — the WebRTC leak test always returns green
- Se la VPN è già attiva all'avvio, l'IP reale mostrato potrebbe essere l'exit node Tor / If VPN is already active at startup, the displayed real IP may be the Tor exit node

---

## 📄 Licenza / License

MIT
