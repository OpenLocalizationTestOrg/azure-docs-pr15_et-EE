<properties
    title="required"
    pageTitle="Meie tehnika artiklites kasutatavad kohandatud allahindlusest laiendid"
    description="Kohandatud allahindlusest laiendid, mis võimaldavad manustatud videoid, märkmete ja näpunäiteid, korduvkasutatava sisu ja muu üksuse azure.microsoft.com tehnika artiklites on loetletud."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Allahindlusest Azure.microsoft.com jaoks

Üldine allahindlusest leiate näpunäiteid teemast [Allahindlusest põhitõed](https://help.github.com/articles/markdown-basics/) ja meie [allahindlusest cheatsheet](./media/documents/markdown-cheatsheet.pdf?raw=true). Kui teil on vaja luua artiklis crosslinks allahindlusest, vt [linkimine] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com toetab [eraldatud koodi plokid](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) ja [süntaksi esiletõstmine](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). Siiski ACOM toetab ainult üks süntaks esiletõstmine värviskeemi sõltumata teie määratud koodi blokeerimise keel.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Meie tehnika artiklites kasutatavad kohandatud allahindlusest laiendid

Kasutage meie artikleid GitHub maitsestatud allahindlusest enamik artiklis vormingu - punktid, lingid, loendeid, jne. Kuid me kasutame kohandatud allahindlusest laiendid, kus läheb vaja azure.microsoft.com sulatatud lehtede rikkalikumat vormingu. Siin on praegu kasutavad laiendid.

+ [Märkmete ja näpunäited]
+ [Sisaldab]
+ [Manustatud videoid]
+ [Tehnoloogia ja platvorm lülitid]

## <a name="notes-and-tips"></a>Märkmete ja näpunäited

Saate valida 4 tüüpi märkmeid ja näpunäiteid.

- AZURE'I. MÄRKUS
- AZURE'I. HOIATUS
- AZURE'I. TIPss
- AZURE'I. OLULISTE

###<a name="usage"></a>Kasutus
Märkmete ja näpunäited pigem tagasihoidlikult kogu teie artikleid üldiselt kasutada. Kui te neid kasutada, valige märkus või näpunäite vastav tüüp:

- Kasutage AZURE. Pange tähele esiletõstmiseks neutraalne või positiivne teave, mis rõhutab või täiendab põhiteksti põhilistest punktidest. Märkme annab teavet, mis kehtib ainult teatud juhtudel.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Kasutage AZURE. Hoiatus Teavita kasutajal tingimus, mis võib põhjustada probleem tulevikus. Näiteks on teatud suvandi valimine või valiku abil teatud võib jäädavalt lukustamine saate kindla stsenaariumi.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Kasutage AZURE. Näpunäide aidata kasutajatel rakendada meetodite abil ja nende vajadustest tekstis kirjeldatud toiminguid. Näpunäide võib ka soovitamine alternatiivsete, mis ei pruugi olla selge. Näpunäiteid, aga ei ole olulised põhiteadmisi teksti.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Kasutage AZURE. OLULINE teave, mis on tööülesande lõpuleviimiseks.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Kuigi need märkmed ja näpunäited toetavad koodi plokid, pildid, loendid ja lingid, proovige hoida oma märkmed ja näpunäited lihtne ja arusaadav. Kui te ei leia ise keerukate märkmete loomine paljude vormingu, mis võib olla peate mõne muu põhiteksti artikli jaotist märk. Ja artiklis liiga palju märkmeid saab häiriva ja raske lugeda või.

###<a name="sample-markdown"></a>Valimi allahindlusest

Kõik näidised kuvada ka AZURE. MÄRKUS. Näpunäide, hoiatust või tähtis kasutamiseks asendada selle allahindlusest "Märkus":

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Ühe lõigu:

    > [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Microsoft Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit.

Multiparagraph:

    > [AZURE.NOTE] Selle õpetuse lõpuleviimiseks peab teil olema aktiivne Microsoft Azure'i konto.
    >
    > Kui teil pole kontot, saate [luua tasuta prooviversiooni konto](http://www.windowsazure.com/pricing/free-trial/) vaid paar minutit.

## <a name="includes"></a>Sisaldab

Korduvkasutatava teksti meie GitHub hoidla asub failide kutsume "sisaldab". Kui teil on tekst, mida tuleb kasutada mitut artikleid, saate lisada faili korduvkasutatava teabe viide. Kaasa ise on lihtne allahindlusest (.md) faili. See võib olla mis tahes kehtiv allahindlusest, sh tekst, pildid ja. Kõik failid tuleb allahindlusest sisaldavad [soovitud / sisaldab kataloogi](https://github.com/Azure/azure-content/tree/master/includes) hoidla juurkaustas. Kui see artikkel on avaldatud, sujuvalt integreeritud avaldatud teema kaasamine teksti.

- Teatud süntaksi abil viidata ka kaasamine.

- Saate luua ka kaasa meediumifailide tuleb luua media kausta teatud kaasamine. Meediumi kaustade jaoks sisaldab kuuluvad [azure-sisu/sisaldab/media](https://github.com/Azure/azure-content/tree/master/includes/media)kausta. Meediumi kataloogi ei tohi sisaldada mis tahes oma pildid. Kui kaasa ei saa pilte, siis vastava meediumi kataloogi ei ole vaja.

###<a name="usage"></a>Kasutus

- Kasutage sisaldab kõikjal, kus teil on vaja sama teksti kuvada mitu artikleid.

- Sisaldab on mõeldud kasutamiseks märgatavat hulgal sisu - lõigu või kaks, ühiskasutusega protseduuri või ühiskasutusega jaotis. Ärge kasutage neid midagi väiksem lause; **need on pole toodete nimed**.

- Tagada kõigi on kaasamine teksti on kirjutatud terveid lauseid või fraase, mis ei sõltu eelnev tekst või artikli, mis viitab kaasa järgmine tekst. Juhised ignoreerimine loob mõne tõlkimatud stringi artikkel, mis on lokaliseeritud komplekti piirid. 

- Ärge manustamine kuuluvad muu sisaldab. Ta ei toeta DPS, avaldamise süsteem.

- Ärge jagage media failide vahel. Eraldi faili kasutamine iga kaasamine ja artiklis kordumatu nimi. Talletada meediumifail seostatud kaasa media kausta.

- Ärge kasutage mõne kaasa artikli ainult sisu.  Sisaldab täiendavatele sisu ülejäänud artikkel on mõeldud.

- Kuna kõik sisaldab peab olema selle / sisaldab kataloogi tee on kaasamine artiklist on alati

    .. / sisaldab

- Korrake link või pilt, failinimi viide nii artiklit ja kaasamine. Lisage "-kaasata" soovite lingi viide või meediumi failinime vältimiseks korduv viide:

 **Lingi viide**

 Muutmine: et odata.org: odata.org kaasata

 **Pildi viide**

 Muutmine: et table.png: tabeli-include.png

###<a name="sample-markdown"></a>Valimi allahindlusest
Mõne kaasa lisamine dokumentatsiooni artiklis süntaks on:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Näide

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Kaasa esimene osa on kaasamine nimi ilma tee ja .md laiendamine. Teine osa on suhteline tee lisa olevat / sisaldab kataloogi laiendiga .md.

###<a name="rendering"></a>Renderdamise

Kaasa muudab sulatatud GitHub lehel järgmiselt:

 [AZURE'I. KAASA juhendid bloobimälu]

Sulatatud HTML-vormingus azure.microsoft.com, HTML-koodi, klõpsake soovitud sisaldab on ühendatud ülejäänud dokumendi HTML-vormingus. Siiski HTML-i sisaldab mõnda HTML-i kommentaari algse sisaldavad allahindlusest failinimi ja GitHub Kinnita räsi. See kommentaar on kaasatud tõrkeotsinguks nii, et allika sisu saate hõlpsalt tuvastatud ja leitud GitHub:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Manustatud videoid

Meie tehnilised artiklid toetamiseks embeddeded videod tehnilised artiklid kui videod on Microsofti [kanali 9](http://channel9.msdn.com/) veebisaidil. Kanali 9 videod peab integreeritud [azure.microsoft.com Video keskele](http://azure.microsoft.com/documentation/videos/home/). Me praegu ei toeta manustatud YouTube'i videod; kui te pole ühenduse kaasautor, olete Tere tulemast link YouTube'i kui video, mida soovite esile tõsta sisestatakse seal. Microsoft osaliste tuleks kasutada kanali 9 ja Video keskele.

### <a name="usage"></a>Kasutus

- Veenduge, et video on Video keskel.

- Kopeerige video ID sõbraliku URL-i kanali 9 oleva video või Video Azure'i keskele. Näiteks [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) videot video ID on **azure-ajasti-ebatavalised-ajakava**.

### <a name="syntax"></a>Süntaks

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Renderdamise

Sees GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Avaldatud artiklit: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Tehnoloogia ja platvorm lülitid

Kasutage tehnoloogia ja platvorm lülititest tehnika artiklites, kui te Autor mitme maitsed sama aadress erinevusi rakendamiseks artikli üle tehnoloogiad või platvormid. See on tavaliselt kõige sobivam meie mobiilne platvorm sisu arendajatele. Praegu kaht erinevat tüüpi lülitid, [lihtsa lülitid](#simple-selectors) ja [kahesuunalise lülitid](#two-way-selectors).

Kuna sama Vaateselektori allahindlusest läheb valikusse iga teema, soovitame pannes Vaateselektori oma teema on kaasamine ja seejärel selle kaasa kõik teie teemad, mis kasutavad sama Vaateselektori viitamine.

###<a id="simple-selectors"></a>Lihtne lülitid

Lihtne (ühesuunalise) lülitid renderdada kogumina raadionupud pealkirja all. Võimalik, et valida ühe platvormi või tehnoloogia kogum, nt .net-i, Node.js ja Java Teemad peavad need nupud kasutamine  Kohandatud allahindlusest laiend kasutada mis tahes lülitid – kasutada HTML-i jaoks lülitid.  

Teemast [Alustamine teatis jaoturi](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) näha, kuidas autori loodud 8 versioonide sama artikli, kuid kasutatud lülitid lubada kõigil üle navigeerimine.

![Lihtne Vaateselektori näide](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Süntaks

    > [AZURE.SELECTOR]
    - [Link: #1 sildi](link #1 url)
    - [Link #2 silt](link #2 url)

Näide:

    > [AZURE.SELECTOR]
    - [Universaalne Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS-i](../articles/notification-hubs-ios-get-started/)
    - [Androidi](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Renderdamise

Pildil kuvatakse renderdamise azure.microsoft.com. Sulatatud GitHub lehtedel olevat lülitid renderdada nimega täpploendi lingid.

###<a id="two-way-selectors"></a>Kahesuunalise lülitid

Kahesuunalise lülitid võimaldab kasutajatel, valige soovitud teemade maatriksi kahel viisil. See on oluline, kui Azure tehnoloogia, näiteks Mobile toetab mitme taustväärtus kaudu kui ka mitmele kliendile. Pidage meeles järgmist:

- Kui see on mõeldud `(Platform | Backend)`, dropwdown teksti saab nüüd kohandada.
- Teil pole vaja loendiüksuse oma maatriksis iga punkt, kuid on ainult üksuse, kui teema URL on olemas ja pole duplikaat.
- Link võib olla mis tahes URL-i, kuigi see on tavaliselt teise GitHub teema.

Vaata [Alustamine Mobile teenuste](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) näha, kuidas autori loodud 15 versioonid sama artikli (9 mobiilikliendi platvormid ja 2 kirjutamata platvormid), kuid kasutatud lülitid lubada kõigil üle navigeerimine. Pange tähele, et 3 artiklid ei taustväärtus mõlemad versioonid.

![Kahesuunalise lülitid näide](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Süntaks

    > [AZURE'I. VAATESELEKTORI-loendi (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Näide:

    > [AZURE'I. VAATESELEKTORI-loendi (platvormi | Kirjutamata)]     -  [(iOS-i | .net-i)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS-i | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows universaalne C# | .net-i)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows universaalne C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .net-i)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .net-i)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS-i | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Renderdamise

Pildil kuvatakse renderdamise azure.microsoft.com. Sulatatud GitHub lehtedel olevat lülitid renderdada nimega täpploendi lingid.

<!--Anchors-->
[Märkmete ja näpunäited]: #notes-and-tips
[Sisaldab]: #includes
[Manustatud videoid]: #embedded-videos
[Tehnoloogia ja platvorm lülitid]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)
