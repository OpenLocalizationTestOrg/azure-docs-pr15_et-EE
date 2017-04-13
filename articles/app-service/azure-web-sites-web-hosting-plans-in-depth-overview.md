<properties
    pageTitle="Azure'i rakendust Service lepingud põhjalik ülevaade | Microsoft Azure'i"
    description="Siit saate teada, rakenduse teenuse lepingute Azure'i rakendust Service töö ja kuidas nad saavad oma teenuse haldamine."
    keywords="rakenduse, azure rakenduse teenus, skaala scalable, rakenduse teenusleping, rakenduse teenuse kulu"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure'i rakendust Service lepingud põhjalik ülevaade#

Mõni rakendus teenusleping tähistab funktsioonid ja et mitme rakendustes saate ühiskasutusse anda. Veebirakenduste, mobiilirakenduste, funktsioon rakendused või API rakendusi, [Azure'i rakendust Service](http://go.microsoft.com/fwlink/?LinkId=529714) kõik töötavad on rakenduse teenusleping. Need lepingud toetavad viis hinnakirjad astme: *tasuta*, *ühiskasutuses*, *Basic*, *Standard*ja *Premium*. Iga taseme on oma võimaluste ja võimsus. Rakenduste sama tellimuse ja geograafiline asukoht, saate lepingu ühiskasutusse anda. Kõik rakendused, leping jagavad saate kasutada kõigi võimaluste ja funktsioone, mis on määratletud plaani taseme. Kõik rakendused, mis on seotud lepingu käivitada ressursse, mis määratleb leping.

Näiteks kui teie lepingut on konfigureeritud kasutama kahel "väike" standard teenuse kiht, kõik rakendused, mis on seotud selle lepingu mõlemal juhul töötab ja standard teenuse taseme funktsioonidele juurde. Lepingu eksemplarid, mis töötavad rakendused on täielikult hallatud ja väga kättesaadav.

See artikkel uurib põhitunnused, nt taseme- ja skaala on rakenduse teenusleping ja kuidas need tulevad mängu ajal oma rakenduste haldamine.

## <a name="apps-and-app-service-plans"></a>Rakenduste ja rakenduse teenuse lepingud

Rakendus App teenuses võib olla seotud ainult ühe rakenduse teenusleping igal ajal.

Nii rakendused ja lepingutes sisalduvad ressursirühma. Ressursirühma toimib elutsükli iga ressursi, mis asub selle äärist. Ressursi rühmade abil saate hallata rakenduse tekstilõigule koos.

Kuna ühe ressursirühm võib olla mitu rakendust Service lepingud, saate eraldada erinevate rakenduste muudele füüsilise ressurssidele. Näiteks saate eraldi arendaja, keskkondade ressursse. Heliprobleemide eraldi keskkonnas tekitamiseks ja arendaja katse võimaldab eristada ressursse. Sel viisil võistlema laadi testimise vastu uue versiooni rakenduste jaoks tootmise rakendusi, mis on serveeritakse real kliendid samadele ressurssidele.

Kui teil on mitu lepingute ühe ressursirühm, saate määratleda ka rakendus, mis hõlmavad piirkondades. Näiteks tugevalt saadaval rakendus, mis töötavad kahes piirkonnas sisaldab vähemalt kaks plaanide, üks iga piirkonna ja iga lepinguga seostatud ühte rakendust. Sellisel juhul, siis sisalduvad rakenduse koopiate ühe ressursirühma. Olles ressursirühma mitme lepingud ja mitme rakendused on lihtne hallata, määrata ja vaadata rakenduse seisundi.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Mõni rakendus teenusleping loomine või olemasoleva kasutamine

Rakenduse loomisel soovitatav on luua ressursirühma. Teisalt, kui rakendus, mida kavatsete luua on suurem rakenduse komponent, tuleks see rakendus luua ressursirühm, mis on eraldatud suuremat rakenduse.

Kas uus rakendus on täiesti uue rakenduse või suurem üks osa, saate kasutada olemasolevaid rakenduse teenusleping majutada ka või looge uus. Seda otsust on veel küsimus ja oodatud laadi.

Kui see uus rakendus saab kasutada paljudes ressursse ja on erinevate skaleerimist tegureid teistest rakendustest majutatud olemasoleva plaani, soovitame selle eristada oma kavandamine.

Kui loote leping, saate oma rakenduse uus komplekt ressursse eraldada ja saada ressursi eraldus üle suuremat kontrolli, kuna iga leping saab oma komplekti eksemplarid.

Kuna erinevates lepingutes saate teisaldada rakendused, saate muuta nii, et ressursid on määratud üle suuremat rakendus.

Lõpuks, kui soovite luua rakenduse teises regioonis ja selle piirkonna pole olemasoleva plaani, luua selle piirkonna saama majutada oma rakenduse seal.

## <a name="create-an-app-service-plan"></a>Mõni rakendus teenusleping loomine

>[AZURE.TIP] Kui teil on rakenduse teenuse keskkond saate vaadata dokumentatsiooni kindla rakenduse teenuse keskkondades siin: [on rakenduse teenuse kava teenuse keskkonnas rakenduse loomine](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Saate luua tühja rakenduse teenusleping kogemusest rakenduse teenuse leping Sirvi või rakenduse loomine osana.

[Azure'i portaal](https://portal.azure.com), klõpsake nuppu **Uus** > **Web + mobile**, ja seejärel valige **Web Appi** või muu rakenduse teenuse rakendus.
![Rakenduse Azure portaali loomine.][createWebApp]

Seejärel saate valida või luua uue rakenduse App teenusleping.

 ![Saate luua ka rakenduse teenusleping.][createASP]

Uus rakendus teenusleping loomiseks nuppu **[+] Loo uus**, tippige **rakenduse teenusleping** nimi ja seejärel valige soovitud **asukohta**. Klõpsake **esimese taseme hinnad**ja seejärel valige sobiv hinnad taseme teenuse kohta. Valige **Kuva kõik** Kuva rohkem suvandeid hinnakirjad, nt **tasuta** ja **ühiskasutuses**. Kui olete valinud hinnakirjad taseme, klõpsake nuppu **Valige** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Rakenduse teisaldamine mõnele muule lepingule rakenduse teenus

Rakenduse teisaldamine rakenduse erinevate teenusleping [Azure portaali](https://portal.azure.com). Saate teisaldada rakendused kui lepingud on sama ressursirühm ja geograafilise asukoha lepingute vahetamist.

Rakenduse teisaldamine mõnda muud lepingut, avage rakendus, mida soovite teisaldada. Klõpsake menüü **sätted** , otsige **Muutmine rakenduse teenuse kavandamine**.

**Muuda rakenduse teenusleping** avab **rakenduse teenusleping** Vaateselektori. Selles etapis saate valida olemasoleva plaani või looge uus. Kuvatakse ainult sobi lepingud (sisse sama ressursirühm ja geograafiline asukoht);

![Rakenduse teenuse leping Vaateselektori.][change]

Iga leping on oma hinnad taseme. Näiteks kui teisaldate tasuta taseme saidil Standard taseme, rakenduse Nüüd võite kasutada funktsioone ja Standard tasandi ressursse.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klooni mõnele muule lepingule rakenduse teenuse rakendus
Kui soovite rakenduse muule alale liikumine, üks võimalus on rakenduse kloonimine. Kloonimine muudab oma rakenduse koopia uude või olemasolevasse rakenduse teenusleping või mis tahes piirkonnas rakenduse teenuse keskkonnas.

 ![Klooni rakendus.][appclone]

Klõpsake menüü **Tööriistad** leiate **Klooni rakendus** .

Kloonimine on mõned piirangud, mida saate lugeda veebisaidil [Azure'i rakenduse teenuse rakenduse kloonimine Azure'i portaalis](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Mõni rakendus teenusleping skaala

On kolm võimalust mastaapimiseks leping.

- **Muuda plaani hinnad taseme**. Näiteks tavaline astme lepingu saab teisendada Standard või Premium taseme ja kõik rakendused, mis on seotud selle lepingu nüüd saate kasutada funktsioone, mis pakub uus teenuse kiht.
- **Plaani eksemplari suuruse muutmine**. Näiteks saab muuta tavaline astme small eksemplarid kasutava lepingu kasutada suur juhtudel. Kõik rakendused, mis on seotud selle lepingu nüüd saate kasutada täiendavaid mälu ja CPU ressursse, mis pakub eksemplari suuremaks.
- **Plaani arvu muutmine**. Näiteks saate 10 eksemplarides mastaabitud Standard leping, mis sobitatakse välja kolm eksemplari. Premium lepingu saate mastaabitud välja 20 eksemplarides (saadavuse). Kõik rakendused, mis on seotud selle lepingu nüüd saate kasutada täiendavaid mälu ja CPU ressursse, mis pakub eksemplari suurem arv.

Saate muuta, klõpsates üksuse **Skaala üles** rakendus või rakenduse teenusleping sätted jaotises hinnakirjad taseme- ja eksemplari suurus. Muudatuste rakendamiseks rakenduse teenusleping ja mõjutavad kõik rakendused, mis on seal.

 ![Rakenduse välja väärtused.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Rakenduse puhastus teenuse kavandamine
Kuna need on endiselt reserveerida Arvuta võimsus konfigureeritud rakenduse teenuse leping skaala atribuutide, kehtib **rakenduse teenuse lepingud** , pole nendega seotud rakendusi kulude endiselt.
Ootamatud kulud, viimase rakendus on rakenduse teenusleping majutatud kustutamisel vältimiseks kustutatakse ka tulemuseks tühja rakenduse teenusleping.


## <a name="summary"></a>Kokkuvõte

Rakenduse teenuse lepingute tähistavad funktsioonid ja võimsus, mida saate jagada oma rakendustes. Rakenduse Service plaanid teile paindlikult eraldada kogumi ressursse teatud rakendused ja edasi ja optimeerida oma Azure ressursi kasutamine. Sellisel viisil, kui soovite oma testimise keskkond säästa saate jagada lepingule üle mitme rakendused. Saate ka maksimeerimine läbilaskevõime jaoks tootmiskeskkonnast skaala selle mitmesse piirkondade ja lepingud.

## <a name="whats-changed"></a>Mis on muutunud

* Juhendi muudatusele veebisaitide rakenduse teenusega artiklist: [Azure'i rakendust Service ja selle mõju olemasoleva Azure'i teenused](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
