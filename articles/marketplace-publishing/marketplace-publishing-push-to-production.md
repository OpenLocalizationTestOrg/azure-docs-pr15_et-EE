<properties
   pageTitle="Teie pakkumise juurutama Azure'i turuplatsi | Microsoft Azure'i"
   description="Lisateavet ja juhiseid, et teie pakkumise--juurutamine virtuaalse masina pilt, arendaja teenus, andmeteenuse jne--tutvustavad Azure'i turuplatsi."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Teie pakkumise juurutada Azure turuplatsiga
Kui olete rahul teie pakkumise (st olete testinud kliendi stsenaariumid, turundus sisu jne) ja olete valmis alustama, taotleda **Push tootmisele** menüü **Avalda** .  

1. Neli toimingut all olevat KIIRTUTVUSTUS lehel klõpsake jaotises avaldamine portaalis peaksid olema täidetud ja roheline. Virtuaalse masina pakkumised, tagada, et järgida järgmisi juhiseid.

    ![Joonis][img-pubportal-walkthru-checked]

2. Valige loendist vasakus servas menüü **Avalda** .

    ![Joonis][img-pubportal-menu-publish]

3. Klõpsake nuppu **push tootmisele kinnitamise taotlemiseks**. Kui kutse on tehtud, meeskonnatöö kinnitamine aktiveeritakse lõplik läbivaatus ja seejärel teie pakkumise saab Azure'i turuplatsil saadaval.

    ![Joonis][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Korral Virtuaalmasinates, kui klõpsate nuppu taotluse kinnitamine push tekitada tehakse järgmist stseeni taha. On võimalik, iga toimingu menüü AVALDA edenemise kuvamiseks klõpsake jaotises avaldamine portaalis. Peate märkima selle lehe regulaarse intervalliga (kuni olekuna kuvatakse "Loetletud") tõrge teavet, mis on vaja paranduse oma lõppu.

> - Esmalt läheb tootmise kutse sertimine meeskonna valideerimiseks on vhd. Juhul, kui värskendate oma juba loetletud pakkumise ja taotluse sai ainult neid muuta, siis sertimine juhise vahele.
> - Järgmisel samm taotluse jõudnud kontrollida pakkumisega turundusmaterjalide sisu meeskonna sisu kinnitamine.
> - Kui eeltoodud juhiseid ei õnnestunud, siis pakkumine on kinnitatud valmistamisel. Sel ajal, muutuvad "loendis" avaldamise portaalis. See olek "Loetletud" ei tähenda siiski, et toiming on lõpule jõudnud. Enne pakkumine on saadaval Azure'i turuplatsi vaja järgmist.
> - Pakkumine kinnitamise valmistamisel ülal kirjeldatud toimingus dispersioonanalüüs pakkumise alustada üle kõik Azure andmekeskuste. Üldiselt 24 – 48 tundi kopeerimise lõpuleviimiseks kulub aga võib kuluda kuni nädal soovitud vhd suurusest. Juhul, kui värskendate oma juba loetletud pakkumine ja see on saanud ainult neid muuta, siis on dispersioonanalüüs on kiirem.
> - Kui soovitud kopeerimine on lõpule jõudnud, klõpsake pakkumine on saadaval Azure'i turuplatsil.

> Saate kustutada pakkumine alati alles siis **projekti** olek (st kunagi **Push lavastus** või **tõuketeatised tootmisele**). Klõpsake menüü **ajalugu** nuppu **Hülga mustand** mustandi kustutamiseks lehe allosas.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Kõik virtuaalse masina pakkumised tootmise kontroll-loend

- Veenduge, et teil oleks Microsoft Azure'i sertifitseeritud partneri
- Klõpsake menüüs SKU-de jaoks suvandi "Peida selles SKU turuplatsilt kuna see tuleks alati osta kaudu lahenduse malli" märkida Jah ainult juhul, kui see osa lahenduse malli. Kõikidel muudel juhtudel see suvand alati märkida ei.
- Pidage meeles: Te ei tohiks muuta SKU nähtavuse säte, kui kasutatava SKU on loetletud. Me ei toeta seda funktsiooni.
- Veenduge, et logod järgima allpool toodud juhistega Azure'i turuplatsi logo.
- Kirjeldus pakkumine, ja te ei peaks olema sama.
- SKU on pealkiri ja pakkuda pikk Kokkuvõte ei peaks olema sama.
- SKU tiitel ja pakkuda Kokkuvõte ei peaks olema sama.
- SKU tiitlid peavad olema pole identsed pakkumise jaoks mitu SKU-de jaoks.

**Azure'i turuplatsi logo juhised**

- Azure'i kujundus on lihtne värvipaleti. Hoidke arvu esmase ja teisese värvide logo madal.
- Kujunduse värvid Azure portaali on valge ja must. Seega Vältige nende värvide oma logod tausta värvi. Kasutage mõnda värvi, mis muudavad teie logod märgatavaks Azure'i portaalis. Soovitatav on lihtne esmane värve. Kui kasutate läbipaistvaid tausta, siis veenduge, et logo/tekst pole valgeks või mustaks.
- Ärge kasutage taustalaadi logo.
- Vältige teksti, isegi oma ettevõtte või kaubamärgi nimi, logo.
- Ilme ja logo peaks olema 'tasapinnalise' ja tuleks vältida astmikke.
- Logo ei peaks tõmmatud.

**Täiendavad juhised Hero logo:**

- Hero logo pole kohustuslik. Avaldaja saate valida Hero logo üleslaadimiseks. **Aga kui üleslaaditud ikooni pilt ei saa kustutada avalda portaal. Sel ajal, järgima partneri tootmise Azure'i turuplatsi juhised pilt ikoonid veel pakkumine pole on kinnitatud.**
- Publisheri kuvatav nimi, SKU tiitel ja pikk Kokkuvõte pakkumine kuvatakse valge fondi värv. Seega Vältige hoida mis tahes heledad ikooni pilt taustaks. Must, valge ja läbipaistva tausta pole lubatud pilt ikoonid.
- Avaldaja kuvada nime, SKU pealkiri, pikk Kokkuvõte pakkumine ja nuppu Loo on manustatud programmiliselt Hero logo kui pakkumine läheb loetletud. Nii sisestada teksti Hero logo kujundamise ajal. Jäta tühja ruumi paremal Kuna teksti (nt Publisheri kuvatav nimi, SKU pealkiri, pikk Kokkuvõte pakkumine) kaasatakse programmiliselt meile seal. Tühja ruumi teksti tuleks 415 x 100 paremal (ja selle tasakaalustavad 370px vasakult).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Pakub juba loetletud virtuaalse masina täiendava toodangu kontroll-loend

- Kontrollige, kas seal on juba pakkumise pakkumine teie ettevõtte nimi. Kui jah, siis lisage uus versioon, mille kasutatava SKU olemasoleva pakkuda dubleeritud uue pakkumise loomise asemel.
- Andmete kettale ei tohiks muuta vahel kaks versiooni samas SKU-ga.
- Azure'i turuplatsi ei toeta hinnakirjad muutmine loetletud SKU-de jaoks, nagu see mõjutab olemasolevad kliendid arveldamine. Veenduge, et te ei muuda hinnad on loetletud SKU-d regioonid, kus on SKU-ga on saadaval. Siiski saate lisada uute SKU-de või lisada uue piirkondade mõne olemasoleva SKU-ga.


## <a name="next-steps"></a>Järgmised sammud
Kui pakkumine läheb reaalajas, testige kliendi stsenaariumid kinnitada, et kõik lepingud ja funktsioonid töötavad õigesti tootmiskeskkonnas nimega ja kinnitanud lavastus keskkonnas.

## <a name="see-also"></a>Vt ka
- [Alustamine: Azure'i turuplatsi avaldamine pakkumise kohta](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
