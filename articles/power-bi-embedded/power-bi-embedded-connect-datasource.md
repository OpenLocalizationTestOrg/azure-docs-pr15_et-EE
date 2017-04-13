<properties
   pageTitle="Microsoft Power BI manustatud - andmeallikaga ühenduse loomine"
   description="Power BI manustatud, andmeallikatega ühenduse loomine"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Andmeallikaga ühenduse loomine

**Power BI manustatud**, mille saate manustada aruandeid oma rakendusse. Kui Power BI aruande embed rakenduse aruande ühendab aluseks olevate andmete **importimise** andmed või **otse ühenduse** **DirectQuery**andmeallika koopia.

Siin on **impordi** - ja **DirectQuery**kasutamise erinevused.

|Importimine | DirectQuery
|---|---
|Tabelite, veergude *ja andmete* imporditakse või aruande andmekomplekti kopeerida. Aluseks olevad andmed tehtud muudatuste vaatamiseks värskendage või lõpule jõudnud, praegune andmehulga uuesti importimiseks.|Ainult, *tabelite ja veergude* on imporditud või aruande andmekomplekti kopeerida. Saate alati vaadata kõige ajakohasemate andmetega.
Power BI manustatud, te saate kasutada DirectQuery pilveandmeallikad, kuid mitte asutusesisese andmeallikate sel ajal.

## <a name="benefits-of-using-directquery"></a>DirectQuery kasutamise eelised

**DirectQuery**kasutamisel on kaks esmane eelised:

   -    **DirectQuery** abil saate koostada visualiseeringute väga suurte andmekogumite, kus muidu oleks võimatu esimese impordi üle kõik andmed.
   -    Aluseks olevate andmete muutused saate nõuda andmete värskendamise ja mõned aruannete jaoks vajate praeguse andmete kuvamiseks saate nõuda suurte andmeedastuse, muutes andmete importimise uuesti teostamatud. Seevastu **DirectQuery** aruannete alati kasutada praegusi andmeid.

## <a name="limitations-of-directquery"></a>DirectQuery piirangud

   On mõned piirangud **DirectQuery**abil.

   -    Kõigi tabelite peab pärinema ühe andmebaasi.
   -    Kui päring on liiga keeruline, tekib tõrge. Vea parandamiseks saate refaktoorime päringut nii, et see on vähem keerukas. Kui soovitud quuery peab olema keerukas, peate **DirectQuery**kasutamise asemel andmed importida.
   -    Seose filtreerimine on piiratud ühes suunas, mitte mõlemast suunast.
   -    Ei saa muuta veeru andmetüüpi.
   -    Vaikimisi paigutatakse piirangud lubatud mõõdud Dax-i avaldised. Vt [DirectQuery ja mõõdud](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery ja mõõdud

Päringute aluseks oleva andmeallika saadetud on aktsepteeritav jõudluse tagamiseks piirangud on pandud mõõdud. Kui **Power BI Desktopi**, edasijõudnud kasutajatele abil saate valida mööda hiilida seda piirangut, valides **Fail > Suvandid ja sätted > Suvandid**. Dialoogiboksi **Suvandid** , valige **DirectQuery**ja valige suvand **Luba piiramatu mõõdud DirectQuery režiimis**. Selle suvandi valimisel saate kasutada mis tahes Dax-i avaldis, mis kehtib mõõt. Kasutaja peab olema teadlik; siiski, et mõned väljendid mis teha hästi andmete importimisel võib kaasa tuua väga aeglane päringute taustväärtus allikas **DirectQuery** režiimis. 

## <a name="see-also"></a>Vt ka
- [Microsoft Power BI manustatud alustamine](power-bi-embedded-get-started.md)
- [Power BI Desktopi](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
