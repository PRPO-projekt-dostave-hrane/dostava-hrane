# ADR-0001: Razrez sistema na storitve katalog, narocila, placila in obvestila 

Status: sprejeto

Datum: 2026-10-29 

## Kontekst 

Sistem za dostavo hrane mora strankam omogočiti pregled restavracij, naročanje, plačilo, sledenje dostavi in ocenjevanje, restavracijam pa upravljanje menijev in sprejemanje naročil. Zahtevani sta vsaj dve mikrostoritvi.

Isti pojmi imajo v različnih delih sistema različen pomen. Jed v meniju ima trenutno ceno in razpoložljivost; jed v naročilu mora hraniti ime in nek ID ter ceno ob času naročila, sicer bi podražitev jedi spremenila že oddana naročila.

Plačilo poteka prek zunanjega ponudnika (simuliranega), ki se lahko odzove počasi, ne odzove ali isto potrditev pošlje večkrat. Podobno velja za obvestila preko e-pošte.

## Odločitev 

Sistem razdelimo na štiri storitve po poslovnih zmožnostih:

- **katalog**: restavracije, njihove povprečne ocene in meniji
- **narocila**: življenjski cikel naročila (od oddano do dostavljeno, poleg tega še zavrnjeno in preklicano) in ocene, ki jo pošlje storitvi "katalog"
- **placila**: plačilo naročila prek simuliranega zunanjega ponudnika; rezultat sporoči storitvi "narocila"
- **obvestila**: pošiljanje e-pošte ob dogodkih iz storitve "narocila"

## Posledice 

+ (+) meje storitev sledijo mejam pomena
+ (+) vsaka storitev ima svojo bazo in jo lahko spreminjam neodvisno
+ (+) izpad ali počasnost plačilnega ponudnika ne vpliva na pregled kataloga ter na sprejem, sledenje in dostavo že plačanih naročil
+ (+) izpad e-pošte ne prepreči naročila
+ (+) razrez se ujema z referenčnim projektom predmeta
- (-) med narocila in placila je dodatna komunikacija za isto naročilo; potrebno je obravnavati izpad in podvojena sporočila tudi med njima
- (-) narocila potrebujejo podatke o jedeh iz kataloga, zato je med njima sinhron klic
- (-) ni skupne transakcije čez storitve
- (-) štiri storitve pomenijo štiri cevovode, image-e in namestitve za enega razvijalca

## Alternative 

- **Monolit**: enostavnejši za enega razvijalca, a ne izpolnjuje zahteve predmeta po mikrostoritvah.
- **Plačilo kot del storitve narocila**: manj dela in brez komunikacije med dvema storitvama za isto plačilo. Zavrnjeno, ker bi težave s plačilnim ponudnikom vplivale na vse funkcionalnosti naročil, tudi na sledenje in dostavo že plačanih naročil.
- **Brez e-poštnih obvestil**: manj dela, ker opis teme e-pošte ne zahteva; restavracija naročila vidi v svojem vmesniku, stranka pa status v aplikaciji. Zavrnjeno, ker stranka brez e-pošte o spremembi statusa izve le, če ima aplikacijo odprto, in ker so obvestila najbolj naraven primer asinhrone integracije med storitvami, ki jo zahteva predmet.