<properties
    pageTitle="Hallata ja jälgida oma konnektorid ja API rakendused rakendus teenuses | Microsoft Azure'i"
    description="Vaate jõudlus konnektorid ja API rakenduste loogika rakendustes; microservices arhitektuur"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika apps 2014-12-01-eelvaade skeemi versioon.

Olete loonud Siserakenduse API-ga. Mida edasi teha?

Azure, iga API rakendus on majutatud Azure eraldi veebisaidi. Selle tulemusena saate hõlpsalt vaadata, mitu taotlusi ei tehta ja vaadake, kui palju andmeid on kasutusel konnektor. Saate ka rakenduse API varundada, teatiste loomine, lubamine hõbepaber turvalisus ja kasutajate ja rollide lisamine.

Selles teemas kirjeldatakse erinevatele suvanditele, et hallata oma API rakenduse.

Sisseehitatud funktsioonide vaatamiseks avage rakenduse API [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040). Kui teie startboard API rakendus, valige see atribuudid avamiseks. Saate valida ka **Sirvi**, valige **API rakendused**ja valige API rakenduse.

![][browse]

## <a name="see-the-properties-you-entered"></a>Et sisestasite atribuudid

API rakenduse avamisel on mitmeid funktsioone ja tööülesannete saadaval:

![][settings]

Sa saad:

- **Sätted** kuvatakse kindla teabe API rakenduse, sh oma tellimuse üksikasjad ja loendeid kasutajatele, kellel on juurdepääs teie API rakenduse. Saate suurendada või vähendada skaala funktsiooni abil API rakenduse eksemplaride arv muude funktsioonide.
- Juhtida API rakenduse **käivitamine** ja **peatamine** nuppude abil.
- Tootevärskenduste meiliteatiste kasutatavaid API rakenduse aluseks olevate failide saate klõpsata **värskenduse** abil uusimad versioonid. Näiteks kui ei paranda või turvalisus värskenduse Microsoft, klõpsates nuppu **Värskenda** automaatselt värskendatakse rakenduse API lisada see parandus.
- Valige **Muuda leping** versiooniuuendus või allavahetamise API rakenduse kasutamine andmete põhjal. Selle funktsiooni abil saate vaadata oma andmete kasutamine.
- Kui loote konnektor, mis sisaldab tabeleid, nagu SQL-i konnektor, saate sisestada soovi ühenduse tabeli nime. Skeemi põhjal tabeli on automaatselt loodud ja saadaval, kui klõpsate **Skeemid alla laadida**. Seejärel saate seda allalaaditud skeemi teisenduse või kaardi loomiseks.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Konnektor või API sisestatud väärtuste määramine

Pärast konfigureeritud või teie ehitatud konnektor loodud, saate muuta sisestatud väärtused. Näiteks kui SQL-i konnektor konfigureeritud ja te soovite SQL serveri nime või tabeli nime muuta, saate selleks API rakenduse tera oma konnektor.

Järgmised juhised.

1. Avage oma konnektor või API rakenduse. Kui te ei tee, avaneb API rakenduse tera.
2. **Essentials**, klõpsake jaotises Host atribuudi hüperlink. Hüperlingi nimega midagi nagu *slackconnector* või *microsoftsqlconnector123*:

    ![][apiapphost]

3. API rakenduse Host tera, valige **sätted**. Valige sätted labale **Rakenduse sätted**. Klõpsake jaotises **Rakenduse sätted**on loetletud oma konfiguratsioon väärtused:

    ![][hostsettings]

4. Valige seade, mida soovite muuta, sisestage uus väärtus ja **salvestada** muudatused.


## <a name="install-the-hybrid-connection-manager---optional"></a>Hübriidjuurutuse ühenduse Manager - valikuline installimine

![][hcsetup]

Hübriidjuurutuse ühenduse Manager võimaldab ühenduse loomiseks kohapealse süsteemi, nt SQL serveri või SAP-i. Selles hübriid ühenduvuse kasutab Azure'i teenus siini ühenduse ja kontrollida oma Azure ressursse ja teie asutusesisese ressursid turvalisus.

Teemast [Azure rakenduse teenuses ühenduse hübriid halduriga](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hübriidjuurutuse haldur on vaja ainult siis, kui loote kohapealse ressursside tulemüüri taha. Kui loote pole kohapealse süsteemi, ühenduse hübriid ülemus võib pole loendis teie konnektor tera.

## <a name="monitor-the-performance"></a>Kuvari jõudlus
Jõudluse mõõdikute on valmisfunktsioonid ja kaasas iga API rakenduse loomisel. Need mõõdikud on teie API majutatud Azure rakendus. Valimi mõõdikud:

![][monitoring]

Sa saad:

- Valige **taotlusi ja tõrgete** erinevate jõudluse mõõdikute tavaliselt tuntud HTTP kuvatavad tõrkekoodid umbes 200, 400 või 500 HTTP olekukoodi, sh lisada. Saate näha vastuse korda, vaadata, mitu taotlused API rakendus ja palju andmeid on saadaval ja kui palju andmeid läheb läbi vaadata. Jõudluse mõõdikute põhjal, saate luua e-posti teatiste kui mõõdiku ületab läve enda valitud Meilikausta.
- **Kasutus**, näete API rakendus kasutab **CPU** üle praeguse **Kvoodi** MB ja vaadata vastavalt oma maksumus taseme maksimaalne andmekasutuse. **Hinnanguline veedavad** aitavad teil otsustada võimalike kulude töötavad rakenduse API-ga.
- Valige **protsessid** protsess Exploreri avamiseks. See näitab, et teie web eksemplarid ja nende atribuutide, sh jutulõnga count ja mälu kasutus.

Nende tööriistade abil saate määratleda, kui rakenduse teenuse leping tuleks ülespoole või allapoole, mastaabitud alusel teie ettevõtte vajadustele. Need funktsioonid on sisseehitatud portaali koos täiendavate tööriistu, ei ole vaja.

## <a name="control-the-security"></a>Juhtelemendi turvalisus

API rakendusi kasutada Rollipõhine Turve. Administraatorirollid rakendada kogu Azure kogemus, sh API rakendused ja muud Azure ressursid. Järgmised rollid.

Roll | Kirjeldus
--- | ---
Omanik | Täielik juurdepääs teenuse haldamine ja saavad anda juurdepääsu muudele kasutajatele või rühmadele.
Kaasautor | Täielik juurdepääs teenuse haldamine. Ei saa anda juurdepääsu muudele kasutajatele või rühmadele.
Lugeja | Saate kuvada kõik ressursid peale saladusi.
Kasutaja juurdepääs administraator | Saate vaadata kõiki ressursid, luua ja hallata rollid ja luua ja hallata kirjad.

Teemast [Rollipõhine juurdepääsu reguleerimine Microsoft Azure'i portaalis](../active-directory/role-based-access-control-configure.md).

Saate kasutajaid lisada ja neid määrata teatud rollide API rakenduse. Portaali kuvatakse selle kasutajatele, kellel on juurdepääs ja nende määratud roll.

![][access]  

- Valige **Kasutajad** kasutaja lisamine, määrata rolli ja kasutaja eemaldamine.
- **Rollide** rollile, kõigi kasutajate kuvamiseks valige kasutaja lisamine rolli ja rollist Kasutaja eemaldamine.


## <a name="more-good-stuff"></a>Lisateavet hea kraam
- Valige oma teatud API rakenduse automaatselt loodud ärplema faili avamiseks **API määratlus** .
- Valige **sõltuvuste** nõutud API rakenduse failide vaatamiseks. Näiteks kui kasutate SAP-i konnektor, installimist mõned täiendavad failide kohta kohapealse hübriid ühenduse ülemus. Neid järgnevusi kuvatakse teie API rakenduse tera.

>[AZURE.IMPORTANT] Kui avate oma API rakenduse atribuudid ja vaadake jaotist **Essentialsi**, on **Host** ja **lüüsi** linke, mis avada uue labad.
>
> ![][host]
>
>Neid atribuute on veebisait, mis hostib API rakenduse. Sisseehitatud API rakenduse või konnektori kasutamisel atribuutidest enamik tegelikult ei kehti ja soovitame, et te ei värskenda neid atribuute. Kui olete loonud API rakenduse Visual Studio ja juurutatud see Azure tellimuse, saate kasutada Host ja lüüsi labad. <br/><br/>


>[AZURE.NOTE] Minge alustada loogika rakendused enne Azure'i konto kasutajaks, [Proovige loogika rakenduse](https://tryappservice.azure.com/?appservice=logic). Saate luua lühiajaline Starteri loogika rakendus. Nõutav krediitkaardid ja kohustusi.

## <a name="read-more"></a>Lisateave

[Loogika rakenduste jälgimine](app-service-logic-monitor-your-logic-apps.md)<br/>
[Konnektorid ja API rakenduste loendis rakenduse teenus](app-service-logic-connectors-list.md)<br/>
[Rollipõhine juurdepääsu reguleerimine Microsoft Azure'i portaalis](../active-directory/role-based-access-control-configure.md)<br/>
[Azure'i rakendust Service hübriid ühenduse halduri kasutamine](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
