# Rate albume: README de context

Scopul documentului: să-l lipești (împreună cu `rate-albume-v2.html`) într-o conversație nouă cu Claude, ca să continue exact de unde am rămas.

## Ce vrea utilizatorul (decizii luate)

- Un **site** unde oricine dă **note albumelor**, fiecare piesă primește o notă, iar nota albumului e **media pieselor notate**.
- Inspirație: carduri de review de pe TikTok (copertă, cerc cu nota totală, tracklist cu note colorate). Doar inspirație, nu trebuie copiat identic.
- **Doar note, fără recenzii sau comentarii.**
- **Fără conturi și fără bază de date.** Se intră ca guest, cu **nickname** ales de utilizator.
- **Căutare live** de albume reale (ex: „psyk" aduce „psykotic"), cu copertă și tracklist reale, fără completat manual.
- **Link de share** cu rate-ul tău, pe care prietenii îl deschid și văd cardul.
- Mai târziu (nu acum): video/card generat pentru TikTok, media comunității, conturi.
- Limba conversației: română. Preferă soluții simple.

## Fișiere

| Fișier | Stare |
|---|---|
| `rate-albume-v3.html` | **Versiunea curentă.** v2 + Album vs Album + BEST/WORST song. |
| `rate-albume-v2.html` | Versiunea anterioară. Un singur fișier, HTML + CSS + JS, fără build. |
| `rate-albume.html` | v1, doar MusicBrainz, fără coperți reale. Depășit. |
| `albume.html` | Primul prototip (introducere manuală, doar localStorage). Depășit. |

Se pune online (necesar pentru ca linkurile de share să meargă între prieteni) pe netlify.com/drop sau GitHub Pages. Poate fi deschis și direct din folder.

## Arhitectură (v2)

Vanilla JS, fără framework. Se randează prin `app.innerHTML`. Rutare pe `location.hash`:

- `""` → `home()`: nickname, căutare, „Rate-urile mele".
- `#a=<id>` → `openAlbum(id)`: încarcă albumul și arată `page(a)`.
- `#s=<payload>` → `shared(p)`: vedere read-only a rate-ului unui prieten.
- `#v=<idA>,<idB>` → `versus(s)`: Album vs Album (oricare id poate lipsi; slotul gol are căutare proprie).

### Surse de date

1. **MusicBrainz** (`musicbrainz.org/ws/2/`, fetch cu CORS): căutare de release-groups cu wildcard (`psyk*`). Doar Album și EP, cu deduplicare. Limită de ~1 cerere/s: funcția `j()` reîncearcă de 3 ori la 503/429.
2. **Deezer** (`api.deezer.com`) prin **JSONP** (funcția `jsonp()`), pentru că nu are CORS:
   - `dzFind(titlu, artist)` caută albumul și verifică potrivirea numelor (primele 5 litere normalizate);
   - `dzTracks(id)` aduce tracklistul;
   - `dzSearch(words)` e căutarea de rezervă dacă MusicBrainz pică sau nu găsește nimic.
3. **Cover Art Archive** (`coverartarchive.org/release-group/<mbid>/front-250`) e ultima rezervă pentru copertă. Are acoperire slabă la albume noi, de aceea s-a trecut la Deezer.
4. Dacă Deezer nu găsește albumul, tracklistul vine de la MusicBrainz (`mbTracks`).

### Identificatori

- Id MusicBrainz = UUID (conține `-`), verificat cu `isMb(id)`.
- Id Deezer = număr. Albumele găsite doar pe Deezer au id numeric.

### Date salvate (localStorage, cheia `rate2`)

```
{ nick: "nume",
  mine: { "<id>": { id, title, artist, year, cover, u: timestamp,
                    tracks: [ { name, r: nota 0–10 sau null } ] } } }
```

Cheia veche `rate1` (v1) nu e migrată.

### Link de share (fără server)

Totul e codat în URL: `#s=` + base64url(JSON) cu câmpurile `{n: nickname, i: id, t: titlu, a: artist, y: an, c: copertă, k: [piese], r: [note]}`. Coperta din link e acceptată doar dacă e pe `*.dzcdn.net` (`imgOk`), ca să nu poată fi pus un URL arbitrar. Butonul „Dă și tu note" copiază albumul în lista celui care deschide linkul, cu note goale.

### Funcții principale

`home`, `search` (`mbSearch` → `dzSearch`), `openAlbum`, `page(a, ro, by)`, `rate(i)` (fereastra cu slider, butoane 1–10 și „Următoarea →"), `shareLink`, `shared`, `route`, `ring(m)`, `cover(a,id)` / `setCover`, `avg(a)`, `col(nota)` (culoare pe notă, paleta `COL`).

## Probleme întâlnite și rezolvate

- **Coperți lipsă** (gradient în loc de poză): Cover Art Archive nu le are → Deezer.
- **„Căutarea a eșuat"**: cauză probabilă MusicBrainz ocupat/rate limit → reîncercări + căutare de rezervă pe Deezer + motivul erorii afișat în mesaj.
- **Legenda 10–0** copiată din cardul TikTok nu făcea nimic în interfață → scoasă.

## Limitări cunoscute

- Netestat de Claude pe date reale (mediul lui nu are acces la MusicBrainz/Deezer); testarea o face utilizatorul și raportează.
- Deezer nu caută pe prefix: „psyk" găsește „psykotic" doar prin MusicBrainz.
- Notele sunt doar în browserul utilizatorului (se pierd la ștergerea datelor); linkurile de share trimise rămân valabile.
- Un link de share are câteva sute de caractere, deci e lung dar funcțional.

## UI (v2.1, redesign)

Temă închisă ca implicit (cu variantă deschisă) și **buton rotund sus-dreapta pentru comutare noapte/zi** (alegerea se salvează în localStorage, cheia `theme`; fără alegere se folosește tema sistemului), glow și gradient pe fundal, font Space Grotesk (cu fallback), titlu „hero" pe pagina de start, insignă cu nota pe copertele din „Rate-urile mele", fundal estompat din copertă pe pagina albumului, cerc cu strălucire, **piese afișate ca butoane cu contur** (conturul și o bară de fundal proporțională cu nota iau culoarea notei; piesele nenotate au contur discret), fereastra de rating tintată în culoarea notei curente.

## v3: funcții noi

- **BEST / WORST song**: în fiecare album, piesa cu nota cea mai mare e verde, cea mai mică roșie, restul albe (nenotatele rămân discrete). Apare doar dacă ai cel puțin 2 piese notate cu note diferite; la egalitate sunt evidențiate toate piesele cu nota maximă/minimă. Logica e în `roles(a)` și `trk(a,i,R,sym)`.
- **Album vs Album**: butonul „⚔ Album vs Album" pe pagina de start și pe pagina unui album. Două coloane, fiecare cu copertă, cerc cu media, tracklist notabil și buton „Notează piesele"; cel cu media mai mare primește „CÂȘTIGĂ". Notele se salvează la fel ca în rest (`rate2`).
- Refactor: `rate(a,i,re)` primește albumul și funcția de redesenare; `loadAlbum(id)` încarcă un album fără să-l afișeze (cu cache `alb`); `search(v,box,pick)` e reutilizabilă.

## Idei pentru mai târziu

1. Card/video pentru TikTok din rate-ul unui album (imagine descărcabilă).
2. Media comunității: necesită backend (Supabase cu autentificare anonimă; invitatul poate deveni cont fără să piardă notele) și protecții anti-abuz.
3. Link de profil cu toate albumele notate.
4. Tier list, comparație între prieteni, clasament pe discografie.

## Cum mă pui la curent data viitoare

Spune-mi: „continuăm proiectul din README", încarcă acest fișier și `rate-albume-v3.html`, apoi scrie ce vrei schimbat.
