<properties
   pageTitle="Azure'i turbekeskus Lühijuhend | Microsoft Azure'i"
   description="See artikkel aitab teil kiiresti hakata Azure'i turbekeskus juhendada teid läbi turvalisuse poliitika- ja halduse komponendid ja linkimine järgmised toimingud."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Azure'i turbekeskus Lühijuhend

See artikkel aitab teil kiiresti kasutamise alustamiseks Azure'i turbekeskus suunavad teid kaudu turvalisus poliitika- ja haldus turbekeskus.

> [AZURE.NOTE] Selles artiklis tutvustatakse teenus on näide juurutamise abil. See artikkel ei ole samm-sammult juhendi.

## <a name="prerequisites"></a>Eeltingimused

Alustamine turbekeskus, peab teil olema Microsoft Azure'i tellimust. Kui olete tellinud, te saate registreeruda [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).

Tasuta astme turbekeskus tellimus automaatselt lubatud ja pakub oma Azure turvalisus seisundi nähtavus. Pakub turvalisuse poliitika haldus, turvalisus soovitusi ja integreerimise turvalisus toodete ja teenuste Azure partnerite.

Pääsete turbekeskus [Azure portaali](https://azure.microsoft.com/features/azure-portal/). Azure portaali kohta leiate lisateavet teemast [portaali dokumentatsiooni](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Andmete kogumine

Turbekeskus kogub andmete hindamiseks nende turvalisus, soovituste turvalisus ja märku, et ohtude oma virtuaalmasinates (VMs). Kui kasutate esmalt turbekeskus, andmete kogumine on lubatud kõik VMs teie tellimus. Soovitatav on kasutada andmete kogumine, kuid saate loobuda abil andmete kogumine turbekeskus poliitika väljalülitamine.

Järgmised toimingud on kirjeldatud, kuidas vaadata ja kasutada turbekeskus komponendid. Neid juhiseid näitame teile kuidas andmete kogumine välja lülitada, kui valite loobuda.

## <a name="access-security-center"></a>Turvalisus Hõlbustuskeskus

Portaalis järgmiste juhiste juurde pääseda turbekeskus.

1. Valige menüüs **Microsoft Azure'i** **Turbekeskus**.
![Azure'i menüü][1]

2. Kui avate esimest korda turbekeskus, avaneb **Tere tulemast** tera. Valige Jah **! Soovin, et käivitada Azure'i turbekeskus** tera **Turbekeskus** avamiseks ja andmete kogumine lubamiseks.
![Tervituskuva][10]

3. Pärast käivitada turbekeskus Tere tulemast keelest või valige menüüst Microsoft Azure'i turbekeskus, avaneb tera **Turbekeskus** . Hõlpsaks juurdepääsuks tera **Turbekeskus** tulevikus soovitud suvand **PIN-koodi blade armatuurlaud** (paremas ülaservas).
![PIN-koodi blade armatuurlaua suvand][2]

## <a name="use-security-center"></a>Kasutage turbekeskus

Saate konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad. Konfigureerige turbepoliitika, tellimuse:

1. Enne **Turbekeskus** , klõpsake paani **poliitika** .
![Turbepoliitika][3]

2. Enne **turbepoliitika - määratleda poliitika kohta tellimuse või ressursirühma** , valige tellimus.
3. Enne **turbepoliitika** , **andmete kogumine** on lubatud automaatselt koguda logid. Jälgimisega seotud laiend on ette valmistatud kõik tänase ja uue VMs tellimuse kohta. ( **Off** **andmete kogumine** seadmisega saate loobuda andmete kogumine, kuid see takistab turbekeskus annab teile Turbeteatiste ja soovitused).
4. Valige enne **turbepoliitika** , **Valige salvestusruumi konto müüginäitajaid piirkonna kohta**. Iga piirkonna, kus olete VMs töötab, saate valida need kogutud andmete talletuskoht salvestusruumi konto. Kui valite salvestusruumi konto iga piirkonna jaoks, on see teie jaoks loodud. Kogutud andmed on loogiliselt eraldatud teiste klientide andmete turvalisuse põhjustel.

     > [AZURE.NOTE] Soovitame teil lubada andmete kogumine ja esmalt tasemel tellimuse salvestusruumi konto valimine. Turbepoliitikate saate seada ressursi töörühma tasandil ja Azure tellimuse tasandil, kuid andmete kogumine ja salvestusruumi konto konfigureerimine esineb ainult tellimuse tasemel.

5. Enne **turbepoliitika** , valige **vältimise poliitika**. Avatakse **vältimise poliitika** tera.
![Vältimise poliitika][4]

6. Enne **vältimise poliitika** , lülitage soovituste kohta, mille soovite kuvada osana teie turbepoliitika. Näited:

 - Kõigi toetatud virtuaalmasinates puuduvate OS värskendused säte **süsteemi uuenduste** **kohta** skannib.
 - Säte **OS nõrkade kohtade** **kohta** skannib kõigi toetatud virtuaalmasinates mis tahes OS konfiguratsioone, mis võib teha virtuaalse masina haavatavamaks rünnakutele tuvastamiseks.

### <a name="view-recommendations"></a>Kuva soovitused

1. Naaske **Turbekeskus** tera ja valige **soovitused** paan. Turbekeskus analüüsib perioodiliselt teie Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, kuvatakse see **soovitused** enne soovitusi.
![Soovitused Azure'i Security Center][5]

2.  Valige **soovitused** enne soovituse lisateabe saamiseks ja/või toimingu probleemi lahendamiseks ette võtta.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Oma ressursse seisundi ja turve oleku vaatamine

1.  Naaske tera **Turbekeskus** . Paani **ressursid turvalisus seisund** sisaldab näidikud turvalisus riigi virtuaalmasinates, võrgunduse, andmed ja rakendused.
2.  Valige **virtuaalmasinates** lisateabe saamiseks. **Virtuaalmasinates** tera avaneb ja kuvab olek kokkuvõtte ründevaratõrje programmid, süsteemi värskendused, taaskäivitamisel ja OS nõrkade kohtade oma vms.
![Paani ressursid seisund Azure'i Security Center][6]

3.  Valige vajalikud juhtelementide konfigureerimiseks soovituse **VIRTUAALSE masina soovitused** lisateabe saamiseks ja/või toimingu tegemine.
4.  Valige jaotises **virtuaalmasinates** lisateabe VM.

### <a name="view-security-alerts"></a>Turbeteatiste kuvamine

1.  Valige paani **Turbeteatiste** naasmiseks tera **Turbekeskus** . **Turbeteatiste** tera avaneb ja kuvab loendi teatised. Turbekeskus analüüsi turvalisus logid ja võrgu tegevuse loob nende teatised. Teatiste integreeritud partnerilahenduste kaasatakse.
![Turbeteatiste Azure'i Security Center][7]

    > [AZURE.NOTE] Turbeteatiste on saadaval, kui normaliseeritud astme turbekeskus on lubatud ainult. Normaliseeritud tasandi 90 päeva tasuta prooviversioon on saadaval. Standardse taseme saada lisateavet vt [järgmised toimingud](#next-steps) .

2.  Valige teatise lisateabe kuvamiseks. Selles näites vaatame valige **muudetud süsteemi kahendarvu avastanud**. Avatakse labad teatise täiendavat teavet.
![Azure'i turbekeskus turvalisus teatiste üksikasjad][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Partnerite lahenduste seisundi vaatamine

1. Naaske tera **Turbekeskus** . **Partnerite lahenduste** paani võimaldab teil ühe pilguga jälgida oma partnerite lahenduste integreeritud Azure tellimuse seisund olekut.
2. Valige paan **partnerilahenduste** . Tera avaneb ja kuvab teie ühendatud turbekeskus partnerite lahenduste loendit.
![Partnerite lahenduste][9]

3. Valige partneri lahenduse. Selles näites vaatame valige **F5-WAF** lahendus.  Tera avaneb ja kuvab olekut partnerite lahenduste ja lahenduse seotud ressursid. Valige **lahenduse konsooli** avamiseks partnerite halduse kogemus see lahendus.

## <a name="next-steps"></a>Järgmised sammud
Selles artiklis kasutusele turvalisuse poliitika- ja haldus komponentide turbekeskus. Nüüd, kui olete tuttav turbekeskus, proovige järgmisi toiminguid:

- Konfigureerige turbepoliitika, Azure tellimuse. Lisateavet leiate teemast [säte turbepoliitikate Azure'i Security Center](security-center-policies.md).
- Turbekeskus soovituste abil aitab teil kaitsta oma Azure ressursse. Lisateabe saamiseks leiate [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md).
- Läbi vaadata ja hallata oma praeguse Turbeteatiste. Lisateabe saamiseks vt [haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md).
- Lisateavet selle [ohtu tuvastamise funktsioonid](security-center-detection-capabilities.md) [Standard taseme](security-center-pricing.md) turbekeskus kaasas. Normaliseeritud tasandi 90 päeva tasuta prooviversioon on saadaval.
- Kui teil on küsimusi turbekeskus kasutamise kohta, leiate [Azure'i turvalisus Center KKK](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
