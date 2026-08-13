# Zengo LANcher

LAN parti játékindító. Kiválasztod a listából a játékot, a többit elvégzi:
letölti, telepíti, beállítja a felbontást, elindítja. Tíz klasszikus játék,
ugyanaz a menü Linuxon és Windowson.

Nem kell Steam, nem kell rendszergazda, és semmit nem telepít a rendszer
mappáiba — minden a saját home könyvtáradba megy.

---

## Letöltés — Windows

Töltsd le a `lancher.exe`-t a legfrissebb kiadásból, és indítsd el:

**[⬇ lancher.exe](../../releases/latest)**

Ennyi. **Semmi mást nem kell telepíteni** — se Python, se 7-Zip, se Wine. A
játékokat maga szedi le és bontja ki.

> A fájl nincs kódaláírással ellátva, ezért a Windows SmartScreen figyelmeztethet
> („A Windows megvédte a gépét"). Ilyenkor: **További információ → Futtatás
> mindenképp**.

## Letöltés — Linux

```bash
curl -sSLo lancher https://gist.githubusercontent.com/vomitorius/b67e4f662cce69d4b4f4dcdb0b2243b6/raw/lancher
chmod +x lancher
./lancher
```

Python 3 minden disztróban eleve ott van, más nem kell hozzá. Amit egy adott
játék igényel (`7z`, `wine`, `flatpak`, `zandronum`), azt a script felismeri,
kiírja a pontos telepítő parancsot, és rákérdez, hogy lefuttathatja-e. Ez az egy
lépés kér `sudo` jelszót.

Ha kényelmesebb, tedd elérhetővé mindenhonnan:

```bash
mkdir -p ~/.local/bin && cp lancher ~/.local/bin/
```

---

## Használat

Indítás után a logó alatt megjelenik a játéklista és a saját LAN IP-d (ezt kell
a többieknek beírni).

| Billentyű | Mit tesz |
|---|---|
| `↑` `↓` | választás a listában |
| `Enter` | indítás (ha még nincs telepítve, előtte telepít) |
| `←` `→` | a Doom II alatti Server / Client almenü nyitása és csukása |
| `q` | kilépés |

Parancssorból is megy, mindkét platformon ugyanígy (Windowson `lancher.exe`):

| Parancs | Mit tesz |
|---|---|
| `lancher` | interaktív menü |
| `lancher list` | játéklista és telepítési állapot |
| `lancher play <id>` | indítás (telepít, ha kell) |
| `lancher install <id>` | csak telepítés, indítás nélkül |
| `lancher ip` | a saját LAN IP-d |
| `lancher deps` | függőségek ellenőrzése és telepítése (Linux) |
| `-y` | ne kérdezzen rá a csomagtelepítésre |

## Játékok

Ugyanez a tíz játék, ugyanebben a sorrendben, mindkét platformon:

| id | játék | Linuxon | Windowson | méret |
|---|---|---|---|---|
| `doom` | Doom II (1994) | Zandronum + Doomseeker | chocolate-doom | Doom II IWAD |
| `quake3` | Quake 3 Arena | ioquake3 (flatpak) | portable, natívan | ~210 MB |
| `ut99` | Unreal Tournament '99 | OldUnreal 469e | portable + 469e patch | ~300 MB |
| `cs16` | Counter-Strike 1.6 | Wine | natívan | ~119 MB |
| `dod` | Day of Defeat | Wine | natívan | ~269 MB |
| `tfc` | Team Fortress Classic | Wine | natívan | ~149 MB |
| `halo` | Halo: Combat Evolved | Wine | natívan | ~84 MB |
| `war3` | Warcraft III + Frozen Throne | Wine | natívan | ~261 MB |
| `scbw` | StarCraft + Brood War | Wine + cnc-ddraw | natívan + cnc-ddraw | ~110 MB |
| `aoe2` | Age of Empires II + The Conquerors | Wine | natívan | ~140 MB |

A játékcsomagok Windows játékok, ezért Windowson közvetlenül futnak, Linuxon
pedig Wine alatt, játékonként külön Wine prefixben, hogy ne zavarják egymást.
Ahol van jó natív Linux motor (Doom II, Quake 3, UT99), ott Linuxon azt
használja, mert stabilabb.

**Doom II.** A menüben a Doom II alatt nyíl jobbra kinyitja a **Server** és
**Client** módot. A hoston Server, a többieken Client, ott a script megkérdezi a
szerver IP-jét. Linuxon Zandronum + Doomseeker (port `10666`), Windowson
chocolate-doom (port `2342`), mert Zandronum nincs Windowsra.

**Felbontás.** Ezek a motorok 4:3-ra készültek, és szélesvásznon nem tágítják a
látómezőt, hanem szétnyújtják a képet. Ezért nem a monitor natív felbontása megy
be, hanem a képernyőbe beférő legnagyobb valódi 4:3-as videómód. A StarCraft
fixen 640x480-at tud, azt a [cnc-ddraw](https://github.com/FunkyFr3sh/cnc-ddraw)
wrapper skálázza fel teljes képernyőre, helyes képaránnyal.

## Csatlakozás LAN-on

A host futtassa a `lancher ip`-t (vagy nézze meg a logó alatt), a többiek ezt
írják be:

- **Quake 3** — Multiplayer → Specify → `<IP>`, vagy konzolból (`~`): `/connect <IP>`
- **UT99** — Multiplayer → Open Location → `<IP>`
- **Doom II** — a hoston `doom` → Server, a többieken `doom` → Client, aztán a host IP-je
- **CS 1.6 / Day of Defeat / TFC** — konzolból (`~`): `connect <IP>`
- **Halo** — Multiplayer → Join Game, a LAN-os szerverek maguktól megjelennek
- **Warcraft III / StarCraft / Age of Empires II** — Multiplayer → LAN, a szerver
  magától megjelenik a listában

Quake 3-nál és UT99-nél a „Search local / LAN" fül általában IP nélkül is
megtalálja a szervert, ha egy hálózaton vagytok.

## Hol laknak a fájlok

Linuxon:

```
~/.local/share/zengo-lancher/cache/   letöltött telepítők (törölhető, ha kell a hely)
~/.local/share/zengo-lancher/games/   kicsomagolt játékok
~/.local/share/zengo-lancher/wine/    Wine prefixek játékonként
~/.local/share/zengo-lancher/logs/    a játékok kimenete
~/.config/zandronum/                  Doom II IWAD és Zandronum beállítások
```

Windowson ugyanez a szerkezet a saját profilod alatt:

```
C:\Users\<te>\.local\share\zengo-lancher\
```

Teljes eltávolítás: töröld ezt a mappát (Linuxon a Quake 3 flatpakhoz még
`flatpak uninstall --user org.ioquake3.ioquake3`).

## Hibaelhárítás

**Windows: „A Windows megvédte a gépét".** Aláírás nélküli exe, ez a SmartScreen
alap viselkedése. További információ → Futtatás mindenképp.

**Windows: dupla klikkre felvillan és bezáródik.** Nem szokott: hibánál megvárja,
hogy leüss egy gombot. Ha mégis, indítsd parancssorból (`lancher.exe list`), ott
a hibaüzenet a képernyőn marad.

**A menü szemétnek látszik / nincsenek nyilak.** Windowson használj Windows
Terminalt a régi `cmd.exe` helyett. Ha a színek zavarnak, `NO_COLOR=1` mellett
egyszerű, számozott menüt ad. Pipe-ba vagy fájlba irányítva magától így viselkedik.

**Linux: fekete képernyő (CS 1.6) vagy „DirectX hiányzik" (Warcraft III).** A 32
bites OpenGL hiányzik a gépről. Futtasd: `lancher deps` — felismeri és
feltelepíti (Ubuntu/Debian alatt `libgl1:i386` és társai).

**Megszakadt letöltés.** A félbekész fájl magától törlődik, újrafuttatva onnan
folytatja; a már letöltött csomagokat nem tölti le újra.

**A játék hibakóddal kilépett.** Kiírja a napló végét, a teljes kimenet a `logs/`
mappában van játékonként.

---

Ez a repo csak a letölthető Windows buildet tartja. A Linuxos verzió a fenti
gist-linkről tölthető; a forráskód nem publikus.
