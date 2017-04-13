<properties 
   pageTitle="SQL Azure'i andmebaasi päringu jõudluse tutvustus" 
   description="Päringu jõudluse jälgimise tuvastab enamik CPU tarbimine päringute SQL Azure'i andmebaasi." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>SQL Azure'i andmebaasi päringu jõudluse tutvustus


Haldamine ja jõudlus relatsiooniandmebaasid häälestamine on keeruline ülesanne, mis nõuab märkimisväärsed ja aega investeering. Päringu täitmise ülevaate võimaldab vähem aega andmebaasi jõudluse tõrkeotsingu esitada järgmist:

- Teie andmebaasi ressursi (DTU) tarbimine süvitsi ülevaate. 
- Põhipäringud alusel CPU/kestus ja täitmise count, mis võib olla häälestatud paranenud jõudlus.
- Päringu üksikasjad süvitsiminek võimalus vaadata selle teksti ja ressursi kasutamine ajalugu. 
- Jõudluse häälestamine marginaalid Kuva [SQL Azure'i andmebaasi Advisor](sql-database-advisor.md) toimingud  



## <a name="prerequisites"></a>Eeltingimused

- Päringu täitmise ülevaate on saadaval ainult selliste Azure SQL-i andmebaasi V12.
- Päringu täitmise ülevaate eeldab, et [Päringu poe](https://msdn.microsoft.com/library/dn817826.aspx) on aktiivne andmebaas. Kui päring pood ei tööta, teilt portaali sisse lülitada.

 
## <a name="permissions"></a>Õigused

Päringu täitmise ülevaate kasutamiseks on vajalik [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md) järgmisi õigusi: 

- **Lugeja**, **omanik**, **kaasautor**, **SQL-i DB kaasautor**või **SQL serveriga kaasautor** õigused on vaja vaadata ülemisel ressursi nõudvate päringud ja diagrammid. 
- Päringu teksti kuvamiseks on vajalik **omanik**, **kaasautor**, **SQL-i DB kaasautor**või **SQL serveriga kaasautor** õigused.



## <a name="using-query-performance-insight"></a>Päringu täitmise ülevaate abil

Päringu täitmise ülevaate on lihtne kasutada.

- Avage [Azure'i portaal](https://portal.azure.com/) ja otsige üles andmebaas, mida soovite analüüsida. 
  - Valige vasakus servas menüüs tugi- ja tõrkeotsing, klõpsake jaotises "Päringu täitmise ülevaate".
- Esimese menüü Läbivaatus ressursside tarbimine põhipäringud loendit.
- Valige päring üksikute üksikasjade kuvamiseks.
- Avage [SQL Azure'i andmebaasi Advisor](sql-database-advisor.md) ja kontrollige, kas soovitused on saadaval.
- Kasutage liugureid või suurendada ikoonid täheldatud intervalli muutmiseks.

    ![jõudluse armatuurlaud](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Paari tunni andmete peab olema jäädvustatud päringu poe SQL-i andmebaasi päringu jõudluse aru saada. Kui andmebaas on ei kasutata või päringu poe ilmnes aktiivne teatud ajavahemiku jooksul, on diagrammid on tühi kuvamisel selle aja jooksul. Päringu poe võib lubada igal ajal, kui see ei tööta.   



## <a name="review-top-cpu-consuming-queries"></a>Vaadake üle ülemise CPU tarbimine päringud

[Portaali](http://portal.azure.com) tehke järgmist.

1. SQL-andmebaasi ja klõpsake käsku **Kõik sätted** > **tugi + tõrkeotsing** > **päringu jõudluse ülevaate**. 

    ![Päringu täitmise ülevaade][1]

    Avaneb vaade põhipäringud ja CPU nõudvate põhipäringud on loetletud.

1. Klõpsake diagrammi üksikasjalikku ümber.<br>Ülemise rea kuvatakse üldine DTU % andmebaasi, samal ajal on lindid Kuva CPU % tarbitud valitud päringud valitud ajavahemiku jooksul (näiteks siis, kui on valitud **möödunud nädalal** iga riba tähistab üks päev).

    ![põhipäringud][2]

    Alumine ruudustiku tähistab kokkuvõtet nähtav päringuid.

  - Päringu ID – päringu sees andmebaasi ainuidentifikaator.
  - CPU päringu jälgitav ajavahemiku jooksul (sõltub koondamine funktsioon) kohta.
  - Päringu kestus (sõltub koondamine funktsioon).
  - Kindla päringu täitmised koguarv.

    Märkige või tühjendage üksikute päringute kaasamiseks või välistamiseks need diagrammilt abil märkeruudud.

1. Kui teie andmed aegunud, klõpsake nuppu **Värskenda** .
1. Saate kasutada liugureid ja nuppude jälgimine intervalli muutmine ja uurida diagrammi suurendada:  ![sätted](./media/sql-database-query-performance/zoom.png)
1. Soovi korral, kui soovite mõne muu vaate, saate valida **kohandatud** menüü ja määramine:
  
  - Meetermõõdustik (CPU, kestus, täitmise loendus)
  - Ajavahemik (viimased 24 tundi, möödunud nädalal, möödunud kuul). 
  - Päringute arv.
  - Funktsioon koondamine.

    ![sätted](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Üksikute päringu üksikasjade kuvamine

Päringu üksikasjade vaatamiseks tehke järgmist.

1. Klõpsake mis tahes päringu põhipäringud loendis.

    ![üksikasjad](./media/sql-database-query-performance/details.png)

1. Avaneb üksikasjade vaade ja päringute CPU tarbimine/kestus ja täitmise arv on jaotatud aja jooksul.
1. Klõpsake diagrammi üksikasjalikku ümber.
  - Ülemisel diagrammil kuvatakse rida, mille üldine andmebaasi DTU % ja ei CPU % tarbitud valitud päringut.
  - Teise diagrammi kuvatakse valitud päring kestab kokku.
  - Diagrammi all kuvatakse valitud päringut täitmised koguarv.
    
    ![päringu üksikasjad][3]

1. Soovi korral võite kasutada liugureid, nupud suurendada või **sätted** päringu andmete kuvamise viisi kohandamiseks või valida muu aja jooksul.

## <a name="review-top-queries-per-duration"></a>Vaadake üle põhipäringud kestuse kohta.

Päringu täitmise ülevaate tehtud värskenduses me kasutusele uue mõõdikud, mis aitavad teil võimalik põhimõtteid: kestus ja täitmise arv.<br>

Päringute on suurim lukustamine rohkem ressursse, blokeerides teisi kasutajaid ja piirata skaleeritavus. Nad saavad ka parima optimeerimine.<br>

Tuvastamiseks pikaajalise päringud:

1. Päringu täitmise ülevaate **kohandatud** vahekaardi valitud andmebaasi avamine
1. Mõõdikute olema **kestuse** muutmine
1. Valige päringud ja jälgimine intervall arv
1. Valige koondamine funktsioon
  - **SUM** liidab kõik päringu täitmise aja jooksul kogu jälgimine.
  - **Max** leiab päringud, mis täitmisaeg on maksimaalne intervalliga kogu jälgimine.
  - **AVG** leiab täitmisaeg, päringu kõik täitmised ja ülaosas välja nende keskmiste kuvamine. 

    ![päringu aeg][4]

## <a name="review-top-queries-per-execution-count"></a>Vaadake üle põhipäringud täitmise arvu kohta.

Suure arvu täitmised võib olla mõjutamata andmebaasi ise ja ressursside kasutus võib olla madal, kuid võite saada üldist rakenduse aeglane.

Mõnel juhul võidakse väga kõrge täitmise count suurendada network edasi-tagasi. Edasi-tagasi oluliselt mõjutada jõudlust. Need on võrgu latentsus ja järgneval serveri latentsus. 

Näiteks palju Andmepõhiste veebilehtede tugevalt andmebaasile juurdepääsu taotlemise iga kasutaja. Kui ühendus ühendamise aitab võrgu liikluse ja töötlemine laadi andmebaasiserveris võivad negatiivselt mõjutada jõudlust.  Avastate, et on edasi-tagasi minimaalselt.

Täidetud tuvastamiseks sageli päringuid ("jutukas") päringud:

1. Päringu täitmise ülevaate **kohandatud** vahekaardi valitud andmebaasi avamine
1. Mõõdikute, et **täitmise arvu** muutmine
1. Valige päringud ja jälgimine intervall arv

    ![päringu täitmise loendamine][5]

## <a name="understanding-performance-tuning-annotations"></a>Jõudluse häälestamine marginaalid mõistmine 

Kuigi uurides oma päringu jõudluse ülevaate töökoormus, võite märgata vertikaalse joone peal diagrammi ikoonid.<br>

Need ikoonid on marginaalid; tegemist jõudlust mõjutavad [SQL Azure'i andmebaasi Advisor](sql-database-advisor.md)toimingud. Järgi Uusehitise marginaali, saate toimingu põhiteavet:

![päringu marginaali][6]

Kui soovite rohkem teada saada, või advisor soovitus rakendada, klõpsake ikooni. Avaneb toimingu üksikasju. Kui see on aktiivne soovitus, mis seda kohe käsu abil saate rakendada.

![päringu marginaali üksikasjad][7]

### <a name="multiple-annotations"></a>Mitme marginaalid. ###

On võimalik, et tõttu suumitase marginaalid, mis on üksteisele lähedal kuvatakse saada ahendatud ühte. See esindab teisiti ikooni, klõpsake seda on avatud uus tera, kus loendi rühmitatud marginaalid kuvatakse.
Päringute ja jõudluse häälestamine toimingud tõestusmeetodid aitab paremini mõista teie töökoormus. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Päringu poe konfiguratsiooni päringu jõudluse ülevaate optimeerimine

Oma päringu jõudluse ülevaate kasutamisel võib ette tulla päringu poe järgmistest teadetest.

- "Päringu poe pole õigesti konfigureeritud selle andmebaasi. Siit leiate lisateavet."
- "Päringu poe pole õigesti konfigureeritud selle andmebaasi. Click here to sätteid muuta." 

Need sõnumid kuvatakse tavaliselt siis, kui päring pood ei saa uute andmete kogumise. 

Esimesel juhul toimub päringu poe on kirjutuskaitstud oleku ja parameetrite optimaalselt. Saate määrata see päring poe mahu suurendamine või tühjendades päringu pood.

![QDS nupp][8]

Teisel juhul toimub päringu poe on välja lülitatud või pole parameetrite optimaalselt. <br>Saate muuta säilitus-ja jäädvustada ja lubada päringu poe täites alltoodud käsud või otse portaalis:

![QDS nupp][9]

### <a name="recommended-retention-and-capture-policy"></a>Soovitatav säilitus- ja jäädvustada poliitika

On kahte tüüpi Säilituspoliitikad.

- Suurus – kui automaatne määramine see puhta andmed automaatselt jõudmisel lähedal maksimaalne maht.
- Vastavalt aega - vaikimisi me määraks 30 päeva, mis tähendab, kui päring poe ruum otsa, kustutab päringu teavet, mis on vanemad kui 30 päeva

Jäädvustada poliitika saab seadistada:

- **Kõik** – jäädvustab kõik päringud.
- **Automaatne** – harv ja päringute ebaoluliste kompileerida ja täitmise kestus ignoreeritakse. Ettevõttesiseselt määratakse lävede täitmise count, kompileerida ja käitusaja aja jooksul. See on vaikimisi.
- **Ükski** – päringu poe lõpetab hõivamine uutele päringutele, kuid käitusaja statistika juba jäädvustatud päringute puhul on endiselt kogutud.
    
Soovitame kõik poliitikate säte automaatne ja selge poliitika 30 päeva:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Päringu poe suurendamine. See võiks täita andmebaasiga ühenduse loomisel ja väljastamise järel päringu:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Nende sätete rakendada lõpuks teha päringu poe kogumiseks uutele päringutele, aga kui te ei soovi oodata kustutada päringu pood. 
> [AZURE.NOTE] Täidesaatva järgmine päring kustutab kõik praegust teavet päringu pood. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Kokkuvõte

Päringu täitmise ülevaate aitab teil mõista oma päringu töökoormus mõju ja kuidas see on seotud andmebaasi ressursside tarbimine. See funktsioon on teave ülaosas tarbimine päringud ja kerge vaevaga tuvastada need enne muutuvad need probleemi lahendamiseks.




## <a name="next-steps"></a>Järgmised sammud

Lisasoovitused SQL-andmebaasi toimimise kohta, klõpsake enne **Päringu jõudluse ülevaate** [soovitusi](sql-database-advisor.md) .

![Jõudluse Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

