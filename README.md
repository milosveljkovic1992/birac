# Glasač

Tekstualna igra u kojoj si birač na izborima u Srbiji. Od provere biračkog spiska dvadeset dana pre izbora do dana posle izbora, rešavaš 20 situacija i biraš kako da zaštitiš svoj glas i svoja prava.

Posle svake odluke igra pokazuje da li je odluka ispravna, delimično ispravna ili pogrešna, zašto je tako i koje pravilo važi.

Igra je deo serije sa igrama Kontrolor (član biračkog odbora), Posmatrač (stalni posmatrač na biračkom mestu) i Mobilni tim (mobilni tim posmatrača).

## Kako se igra

- Svaka situacija ima četiri opcije. Izabrana opcija se „zaokruži“ kao na glasačkom listiću.
- Tri merača prate tvoj glas: **slobodan izbor**, **tajnost glasa** i **informisanost**.
- Zaglavlje pokazuje gde si i kada: dane pre izbora (`D−20`), sat na dan izbora i dan posle (`D+1`).
- Na kraju vidiš sve svoje odluke, broj ispravnih odluka i ocenu.
- Redosled opcija se meša u svakoj partiji.

Situacije obuhvataju, između ostalog: proveru upisa u birački spisak, pritisak poslodavca i „sigurne glasove“, poklone za glas, obaveštenje o glasanju, glasanje van biračkog mesta, dokumente za identifikaciju, „bugarski voz“, neovlašćeno beleženje birača, tuđi potpis u izvodu, obeležavanje prsta, uticaj člana odbora, porodično glasanje, važeći listić, fotografisanje listića, ubacivanje listića, izlazne ankete, prigovore, zatvaranje biračkog mesta, dezinformacije i „mrtve duše“ u spisku.

## Struktura projekta

```
index.html     cela aplikacija (HTML, CSS i JavaScript u jednom fajlu)
og-image.png   slika za deljenje na društvenim mrežama (1200 × 630)
netlify.toml   podešavanja za Netlify (objava i bezbednosna zaglavlja)
CHANGELOG.md   spisak izmena po verzijama
README.md      ovaj fajl
```

Nema zavisnosti ni build koraka. Jedini spoljni resursi su fontovi sa Google Fonts (Bitter, Source Serif 4, IBM Plex Mono). Ikonica u tabu pregledača je SVG ugrađen u `index.html`.

## Lokalno pokretanje

Otvori `index.html` direktno u pregledaču, ili pokreni lokalni server:

```sh
python3 -m http.server 8000
```

pa otvori http://localhost:8000.

## Objava na Netlify

**Preko Git repozitorijuma (preporučeno)**

1. Postavi projekat na GitHub, GitLab ili Bitbucket.
2. Na Netlify izaberi **Add new site → Import an existing project** i poveži repozitorijum.
3. Build command ostavi prazno, a publish directory je `.`. Ova podešavanja se čitaju iz `netlify.toml`.
4. Pokreni **Deploy**. Svaki novi push na glavnu granu automatski objavljuje novu verziju.

Ako je projekat podfolder većeg repozitorijuma, u podešavanjima sajta na Netlify postavi **Base directory** na putanju do ovog foldera.

**Preko Netlify CLI**

```sh
npm install -g netlify-cli
netlify login
netlify deploy          # probna objava
netlify deploy --prod   # produkcija
```

**Bez naloga za Git:** prevuci folder projekta na https://app.netlify.com/drop.

## Izmena sadržaja

Sve situacije su u nizu `S` unutar `<script>` bloka u `index.html`. Svaka situacija ima ovaj oblik:

```js
{ t: "07:40",                       // vreme na satu, ili dan: "D−5", "D+1"
  loc: "Kod kuće",                  // gde je birač, prikazuje se u zaglavlju
  title: "Gde je lična karta?",
  text: "Opis situacije…",
  opts: [
    { x: "Tekst opcije", v: "ok" },                 // ok | part | bad
    { x: "Tekst opcije", v: "bad", f: "Kratko objašnjenje zašto",
      fx: [-8, -10, -8] },                          // opciono: promena merača
    …
  ],
  why: "Pravilo koje se prikazuje posle odgovora." }
```

Ako opcija nema `fx`, koriste se podrazumevane promene merača iz objekta `DEF` (redom: slobodan izbor, tajnost glasa, informisanost). Ocene na kraju igre zavise od broja bodova i definisane su u funkciji `renderEnd`.

## Verzionisanje

Trenutna verzija je upisana na jednom mestu, u `index.html`:

```html
<meta name="version" content="1.0.0">
```

Broj verzije ima oblik `GLAVNI.SPOREDNI.ISPRAVKA`:

| Izmena | Primer | Nova verzija |
|---|---|---|
| Ispravka teksta, pravila ili greške u kodu | pogrešan rok u objašnjenju | 1.0.0 → 1.0.1 |
| Nove ili izmenjene situacije, nova funkcija | dodata situacija o glasanju iz inostranstva | 1.0.1 → 1.1.0 |
| Veća promena igre ili usklađivanje sa novim zakonom | igra za lokalne izbore, novi sistem bodovanja | 1.1.0 → 2.0.0 |

Postupak za novu verziju:

1. Promeni `content` u `<meta name="version">`.
2. U `CHANGELOG.md` premesti stavke iz „Neobjavljeno“ pod novi naslov sa brojem verzije i datumom.
3. Napravi commit i Git tag, pa pošalji oba:

```sh
git commit -am "Verzija 1.0.1"
git tag -a v1.0.1 -m "Verzija 1.0.1"
git push && git push --tags
```

## Napomena o tačnosti

Igra je edukativna simulacija sa izmišljenim mestima i situacijama. Pravila su pojednostavljena prema Zakonu o izboru narodnih poslanika, Zakonu o jedinstvenom biračkom spisku, Krivičnom zakoniku i uputstvima Republičke izborne komisije. Nisu pravni savet. Pre korišćenja igre na obukama treba proveriti u važećim propisima rokove (zaključenje biračkog spiska, rok za prigovor), spisak dokumenata kojima birač dokazuje identitet i pravila za izlazne ankete.
