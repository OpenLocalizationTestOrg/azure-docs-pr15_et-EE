<properties
    pageTitle="Rollipõhise juurdepääsu juhtimine tõrkeotsing | Microsoft Azure'i"
    description="Abi probleeme või küsimusi Rollipõhine juurdepääsu juhtimine ressursside kohta."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Rollipõhine juurdepääsu reguleerimine tõrkeotsing

## <a name="introduction"></a>Sissejuhatus

[Rollipõhine juurdepääsu reguleerimine](role-based-access-control-configure.md) on võimas funktsioon, mis võimaldab volitatud esindaja kohandatud juurdepääs Azure'i ressurssidele. See tähendab, et kindel õiguse teatud isiku õigust kasutada täpselt, mida ta peab ja enam tunda. Siiski aeg-ajalt võib olla keeruline ressursi mudeli Azure ressursid ja võib olla keeruline aru täpselt mis lubate õigused.

Selle dokumendi anname teile teada, mis ootab mõne rollid Azure portaali abil. Need kolm rollid hõlmab kõiki ressurss:

- Omanik  
- Kaasautor  
- Lugeja  

Omanikud ja kaasautorid on täielik juurdepääs teenuse haldamine, kuid kaasautori ei saa anda juurdepääsu muudele kasutajatele või rühmadele. Asjad veidi põnevamaks lugeja rolli, nii et kui me kulutada veidi aega. Vaadake, kuidas juurdepääsu andmiseks [Rollipõhine juurdepääsu reguleerimine alustamise artiklis](role-based-access-control-configure.md) üksikasju.

## <a name="app-service-workloads"></a>Rakenduse teenuse töökoormus

### <a name="write-access-capabilities"></a>Kirjutamisõigused võimalused

Kui annate kasutaja kirjutuskaitstud juurdepääsu ühe veebirakenduse, mõned funktsioonid on keelatud, et teil võib oodata. Järgmised halduse võimaluste kasutamiseks on vaja web app (kaasautor või omanik) juurdepääsu **kirjutamine** ja ei saa kasutada mis tahes kirjutuskaitstud stsenaariumi.

- Käskude (nt algus, Peata jne)
- Nt üldine konfiguratsiooni, skaala sätteid, varundussätted ja jälgimisega seotud sätted sätete muutmine
- Juurdepääs avaldamise identimisteabe ja muude saladused nagu rakenduse sätete ja ühendusstringi
- Streaming logid
- Diagnostikalogide konfigureerimine
- Konsool (Käsuviip)
- Aktiivse ja tehtud juurutuste (juurutamiseks kohaliku git pidev)
- Eeldatav veedavad
- Web testide
- Virtuaalse võrgu (nähtav ainult lugeja, kui kasutaja kirjutamisõigused eelnevalt konfigureeritud virtuaalse võrgu).

Kui te ei pääse mõni järgmised paanid, peate küsima administraatori kaasautor access web appi.

### <a name="dealing-with-related-resources"></a>Seotud ressursid tegelemine

Veebirakenduste on keeruline mõne muu ressursid, millest suhete olemasolu. Siin on paar veebisaitidega tüüpilised ressursirühma:

![Ressursirühm web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Selle tulemusena, kui annate kellegi juurdepääsu lihtsalt web appi, suur osa sellest Azure'i portaalis enne veebisaidi funktsioonid on keelatud.

Nendeks nõuavad **kirjutamine** Accessi **rakenduse teenusleping** , mis vastab teie veebisaidile:  

- Veebirakenduse vaatamine hindade taseme (tasuta või standardne)  
- Skaala konfiguratsiooni (arv eksemplari, virtuaalse masina suurust ja autoscale sätted)  
- Kvootide (salvestusruumi, läbilaskevõime CPU)  

Nendeks nõuavad **kirjutamine** juurdepääsu kogu **ressursirühm** , mis sisaldab teie veebisaidile:  

- SSL-sertide ja sidumiste (see on, kuna SSL-sertide saab jagada ühte ressursirühma saitide ja geograafilise asukoha vahel)  
- Teatiste reeglid  
- Autoscale sätted  
- Rakenduse ülevaateid komponendid  
- Web testide  

## <a name="virtual-machine-workloads"></a>Virtuaalse masina töökoormus

Palju nagu web apps mõni virtuaalse masina enne nõuda virtuaalse masina või muud ressursid ressursside jaotises kirjutamisõigused.

Virtuaalmasinates on seotud domeeni nimed, virtuaalse võrgu, salvestusruumi kontod ja teatiste reeglid.

Nendeks nõuavad **kirjutamine** juurdepääsu **virtuaalse masina**:

- Lõpp-punktid  
- IP-aadressid  
- Ketast  
- Laiendid  

Need nõuavad **kirjutamine** juurdepääsu **virtuaalse masina**ja selle **Ressursirühma** (koos domeeni nimi) on:  

- Kättesaadavus määramine  
- Laadi tasakaalustatud määramine  
- Teatiste reeglid  

Kui te ei pääse mõni järgmised paanid, peate küsima administraatori kaasautor juurdepääsu ressursirühma.

## <a name="see-more"></a>Lisateave
- [Rollipõhine juurdepääsu juhtimine](role-based-access-control-configure.md): alustamine RBAC Azure'i portaalis.
- [Sisseehitatud rolle](role-based-access-built-in-roles.md): rollid, mis tulevad standard RBAC üksikasjade toomine.
- [Azure'i RBAC rollide kohandatud](role-based-access-control-custom-roles.md): saate teada, kuidas luua kohandatud rollid vastavalt vajadusele juurdepääsu.
- [Loo Accessi muuta ajalugu aruande](role-based-access-control-access-change-history-report.md): silma peal muutmisega rollimääranguid RBAC sisse.
