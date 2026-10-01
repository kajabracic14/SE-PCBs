# Superior Engineering PCBs

Dokumentacija vseh vezij bolida: opis delovanja, pinout, CAN, testiranje in zgodovina revizij.

- **Stran:** `index.html` (gostovana na GitHub Pages)
- **Podatki, slike in prijava:** Supabase (brezplačen paket)

---

## Prva postavitev (enkrat, približno 30 minut)

### 1. Supabase projekt

1. Pojdi na [supabase.com](https://supabase.com) in se prijavi z GitHub računom.
2. Klikni **New project**. Ime npr. `se-pcbs`, regija **Central EU (Frankfurt)**, geslo za bazo shrani na varno (aplikacija ga ne potrebuje).
3. Počakaj, da se projekt postavi (1–2 minuti).

### 2. Baza

1. Odpri **SQL Editor** → **New query**.
2. Odpri `supabase.sql` in na dnu (korak 6) vpiši e-maile članov z malimi črkami. `lahko_ureja = true` pomeni urejanje, `false` samo ogled.
3. Prilepi celotno datoteko v urejevalnik in klikni **Run**. Na koncu mora pisati *Success*.

### 3. Povezava strani z bazo

1. V Supabase odpri **Project Settings** → **API** (ali **Data API / API Keys**).
2. Kopiraj **Project URL** in ključ **anon / publishable**.
3. V `index.html` na vrhu skripte vpiši oba podatka:

   ```js
   const SUPABASE_URL = "https://xxxxxxxx.supabase.co";
   const SUPABASE_KEY = "eyJ...";
   ```

   Ta ključ je po namenu javen in sme biti v javnem repozitoriju. Podatke ščitijo pravila v bazi: brez prijave z e-mailom s seznama članov se ne da prebrati ničesar. **Ključa `service_role` nikoli ne vpiši v stran.**

### 4. GitHub Pages

1. V GitHub organizaciji naredi nov **javen** repozitorij, npr. `pcbs`.
2. Naloži `index.html` (in to datoteko `README.md`).
3. **Settings** → **Pages** → *Source*: **Deploy from a branch**, branch `main`, mapa `/ (root)` → **Save**.
4. Čez minuto ali dve je stran na `https://<organizacija>.github.io/pcbs/`.

### 5. Nastavitev prijave

1. V Supabase odpri **Authentication** → **URL Configuration**.
2. **Site URL:** `https://<organizacija>.github.io/pcbs/`
3. Pod **Redirect URLs** dodaj isti naslov.

Brez tega povezava iz e-maila za prijavo ne bo vodila nazaj na stran.

### 6. Prenos podatkov iz prototipa na claude.ai

1. V prototipu klikni **Izvozi vse**. Datoteka vsebuje vsa vezja, revizije in slike.
2. Na novi strani se prijavi in klikni **Uvozi**, nato izberi to datoteko.

---

## Vsakdanje vzdrževanje

**Nov član:** Supabase → **Table Editor** → `clani` → **Insert row**. E-mail z malimi črkami.

**Član odide:** v tabeli `clani` izbriši njegovo vrstico. Takoj nima več dostopa.

**Varnostna kopija:** v aplikaciji vsaj ob koncu sezone klikni **Izvozi vse** in datoteko shrani na ekipni disk.

**Sprememba aplikacije:** uredi `index.html` in ga potisni na `main`. Pages se posodobi samo.

---

## Dobro je vedeti

- **Projekt se uspava.** Brezplačen Supabase projekt se po tednu dni brez uporabe ustavi. Podatki ostanejo. V nadzorni plošči klikni **Restore project** in čez nekaj minut spet deluje. Med sezono se to ne zgodi, poleti pa lahko.
- **Omejitev e-mailov za prijavo.** Vgrajeni pošiljatelj Supabase pošlje le nekaj e-mailov za prijavo na uro. Prijava na napravi ostane veljavna, dokler se ne odjaviš, zato to za ekipo običajno zadošča. Če vas moti, pod **Authentication** → **SMTP Settings** nastavite svoj e-mail strežnik.
- **Slike so dostopne prek povezave.** Kdor pozna točno povezavo do slike, jo lahko odpre tudi brez prijave. Povezave so naključne in jih ni mogoče uganiti, vseeno pa ne nalagaj ničesar tajnega.
- **Zgradba kode.** Ves dostop do baze je v objektu `store` na vrhu skripte. Preostanek aplikacije ne ve, kje so podatki, zato je zamenjava baze omejena na ta del.

## Zgradba podatkov

| Tabela | Vsebina |
| --- | --- |
| `clani` | e-mail, ime, ali lahko ureja |
| `vezja` | eno vezje: kratica, ime |
| `revizije` | ena revizija vezja: oznaka, status, spremembe, errata in vsi tehnični podatki (JSON v stolpcu `data`) |
| storage `slike` | fotografije konektorjev |
