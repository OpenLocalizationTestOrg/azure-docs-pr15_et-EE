<properties
   pageTitle="Rea-ja Power BI manustatud turvalisuse"
   description="Rea-ja Power BI manustatud turvalisuse üksikasjad"
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

# <a name="row-level-security-with-power-bi-embedded"></a>Rea koos Power BI manustatud turvalisus

Rea turvalisus (RLS) saab kasutaja juurdepääsu piiramiseks teatud andmete aruande või andmekomplekti, mis võimaldab kasutada sama aruande ajal kõik näha erinevaid andmeid mitmest erinevatele kasutajatele. Power BI manustatud toetab nüüd andmekomplektide RLS konfigureeritud.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Selleks, et ära RLS on oluline mõistate kolm peamist mõistet; Kasutajad, rollid ja reegleid. Vaatame lähemalt iga:

**Kasutajate** – need on tegelik lõppkasutajad aruannete. Rakenduses Power BI manustatud, tuvastatakse kasutajad on App luba vara kasutajanimi.

**Rollide** – kasutajad kuuluvad rollid. Rolli ümbris reeglid ja saab nimeks midagi nagu "Müügijuht" või "Müügiesindaja". Rakenduses Power BI manustatud, tuvastatakse kasutajad on App luba vara rollid.

**Reeglid** – rollid on reeglid ja need reeglid on tegelik kavatsete andmetele rakendatud filtrid. See võiks olla võimalikult lihtne "riigi USA-s =" või midagi muud dünaamiline.

### <a name="example"></a>Näide

Selles artiklis ülejäänud pakume näide loome RLS, ja seejärel tarbimine mis manustatud rakenduses. Käesolevas näites kasutatakse [Näidis Retail analüüsi](http://go.microsoft.com/fwlink/?LinkID=780547) PBIX faili.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Meie jaemüügi proovi kuvab kõik poed kindla jaemüügi ahelas. Ilma RLS, olenemata sellest, mis asub juhataja sisse logib ja vaatamist aruande, kuvatakse samad andmed. Juhatuse määranud iga piirkonna halduri peaksite nägema ainult müügiga salvestab, nad hallata ja selle tegemiseks me kasutame RLS.

RLS on loodud Power BI Desktopi. Andmekomplekti ja aruande avamisel me diagrammivaate skeemiga kuvamiseks aktiveerige.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Siin on mõned asjad, mida selles skeemi teade:

-   Kõik mõõdud, nt **Total Sales**, talletatakse tabelis **Sales** asjaolu.
-   Leidub neli täiendavad seotud dimensiooni tabelite: **üksus**, **kellaaja**, **salvestamine**ja **piirkond**.
-   Seose ridadel nooled viitavad sellele, kuidas filtreid saate flow ühest tabelist teise. Näiteks kui filter on **aeg [kuupäev]**, praeguse skeemi see oleks ainult filtreerida väärtused tabelis **Sales** alla. Kuna kõik read seose nooli Osutage soovitud Müügitabeli ja ei kao mõjutaks pole muudes tabelites filtrit.
-   **Piirkonna** tabel näitab, kes ülemus on iga piirkonna jaoks

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Vastavalt selle skeemi, kui me filtri rakendamine **Piirkonna halduri** veerg tabelis piirkond ja kui mis filtreerimiseks vasteid aruande kuvamisel kasutaja filtri ka filtreeritakse **talletamine** ja **müügi** tabelid kuvada ainult teatud piirkonnas andmete halduri alla.

Siin on kuidas:

1.  Klõpsake vahekaardil modelleerimine **Rollide haldamine**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Saate luua uue nimega **halduri**rolli.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  Tabelis **piirkond** sisestage järgmine DAX-i avaldis: **[piirkond halduri] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Veenduge, et vahekaardil **modelleerimine** töötavad reegleid linki **Kuva nimega rollid**ja sisestage järgmine:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Nüüd kuvatakse aruannete andmeid, kui teil on sisse logitud **Andrew Ma**.

Filtri rakendamist me ei siin, kuidas filtreeritakse alla **piirkond**, **salvestamine**ja **müügi** tabelite kõiki kirjeid. Siiski filtri suund seoseid **müügi** - ja **kellaaja**, kuna **Müük** , **üksuse**ja **üksuse** ja **kellaaja** tabelid pole filtreeritud alla.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Mis võib olla ok see tingimus, siiski võib kui me ei soovi haldurid üksused, mille nad ei ole müüdi, sisselülitamine araabiakeelses ristfiltreerimine seose ja meilivoo mõlemast suunast turvalisus filter. Seda saab teha, redigeerides seos **müügi** - ja **üksuse**umbes järgmine:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Nüüd filtrid saate ka tulenevad tabelis Sales **üksuse** tabelisse:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Märkus** Kui kasutate oma andmete jaoks DirectQuery režiimis, peate lubamiseks kahesuunaline rist filtreerimine, valides need kaks võimalust:

1.  **Faili** -> **suvandite ja sätete** -> **Eelvaatefunktsioonide** -> **rist mõlemas jaoks DirectQuery filtreerimise lubamine**.
2.  **Faili** -> **suvandite ja sätete** -> **DirectQuery** -> **Luba piiramatu mõõt DirectQuery režiimis**.


Kahesuunaline ristfiltreerimine kohta lisateabe saamiseks laadige [kahesuunaline ristfiltreerimine SQL Server Analysis Services 2016 ja Power BI Desktopi](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) lühiülevaade.

See murtakse tööd, mis on vaja teha Power BI Desktopi, kuid on üks rohkem tööd mis tuleb teha, et RLS reeglite me määratletud töö Power BI manustatud. Autenditud kasutajad ja rakenduse lubatud ja rakenduse sõned kasutatakse selle kasutaja juurdepääsu teatud Power BI manustatud aruande. Kes on teie kasutaja kindla teabe pole Power BI manustatud. RLS töötada, peate läbima mõned täiendavad kontekstis oma app luba osana.
-   **kasutajanimi** (valikuline) – kasutatud RLS see on string, mis aitab tuvastada kui RLS reeglite rakendamise kasutaja saab kasutada. Jaotisest turvalisus Power BI manustatud rea kasutamine
-   **rollide** – string, mis sisaldab soovitud rea turvalisuse reeglite rakendamise rollid. Kui möödudes rohkem kui üks roll, peaks olema möödunud stringi massiivi.

Kui atribuudi kasutajanimi on olemas, peate läbima vähemalt üks väärtus ka roll.

Täielik app luba näeb välja umbes selline:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Nüüd koos kõik osad koos, kui keegi logib meie rakenduse selle aruande vaatamiseks nad ainult juurde andmed, mida nad on lubatud vaatamiseks meie rea taseme turvalisus määratletud kuvamiseks.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Vt ka
[Rea taseme turvalisus (RLS) koos Power](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
