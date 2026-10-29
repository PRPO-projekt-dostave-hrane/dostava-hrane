# C4 – nivo 1: kontekst sistema

Kdo uporablja sistem Dostava hrane in s katerimi zunanjimi sistemi se povezuje.

```mermaid
flowchart TB
    stranka["<b>Stranka</b><br/>[uporabnik]<br/>naroča hrano in sledi dostavi"]
    restavracija["<b>Restavracija</b><br/>[uporabnik]<br/>ureja meni, sprejema/zavrača naročila"]

    sistem["<b>Dostava hrane</b><br/>[sistem]<br/>katalog restavracij, naročila,<br/>sledenje dostavi, ocene"]

    placila["<b>Plačilni ponudnik</b><br/>[zunanji sistem, simuliran]<br/>obdela plačilo naročila"]
    eposta["<b>E-poštni strežnik</b><br/>[zunanji sistem]<br/>dostavi obvestila"]

    stranka -- "pregleduje menije, naroča, sledi statusu, ocenjuje" --> sistem
    restavracija -- "ureja meni, sprejema/zavrača naročila, kurir označi dostavo" --> sistem

    sistem -- "zahteva plačilo" --> placila
    placila -. "potrdi plačilo" .-> sistem
    sistem -- "pošlje e-pošto" --> eposta
    eposta -. "obvestilo o statusu" .-> stranka
    eposta -. "novo naročilo" .-> restavracija

    classDef uporabnik fill:#08427b,stroke:#052e56,color:#fff
    classDef sistem fill:#1168bd,stroke:#0b4884,color:#fff
    classDef zunanji fill:#999999,stroke:#6b6b6b,color:#fff
    class stranka,restavracija uporabnik
    class sistem sistem
    class placila,eposta zunanji
```

## Legenda

- **temno modro**: uporabniki sistema
- **modro**: naš sistem
- **sivo**: zunanji sistemi
- **polna črta**: sinhron klic
- **črtkana črta**: odziv ali asinhrona pot


