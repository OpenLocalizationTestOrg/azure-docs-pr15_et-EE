<properties
    pageTitle="Azure'i virnas klahvi Vault Sissejuhatus | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i virnas klahvi Vault haldab võtmed ja saladused"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Võtme Vault Azure virnas tutvustus #

## <a name="before-you-start"></a>Enne alustamist

Selles artiklis eeldatakse järgmist:

- Lugeja on tellimus, mis on Azure klahvi Vault lubatud juurdepääs.
- Azure'i PowerShelli SDK on konfigureeritud ja saadaval.
- TP2 versiooni, kõigi rentniku suunatud toiminguid teha ainult PowerShelli kaudu.

## <a name="key-vault-basics"></a>Klahv Vault põhialused

Klahv Vault Azure'i virnas aitab kaitsta cryptographic võtmed ja saladusi, mis cloud rakenduste ja teenuste kasutamine. Klahv Vault abil saate krüptida võtmed ja saladused (nt autentimise klahvid, salvestusruumi konto klahvid, andmete krüptimise võtmed, pfx failide ja paroolid).

Klahv hoidla ühtlustab võtme haldamise protsess ja võimaldab teil hallata tutvustatakse, mida juurdepääsu ja andmete krüptimine. Arendajad saate luua klahvid katsetamine minutiga, ja seejärel sujuvalt üle tootmise võtmed. Turvalisus administraatorid saate anda ja tühistada õigus klahvid, vastavalt vajadusele.

Igaüks Azure'i virnas tellimusega saate luua ja kasutada võtme võlvid. Kuigi klahvi Vault kasu arendajad ja turvalisus administraatorid, saate seda rakendada ja haldab administraator, kes haldab ettevõtte Azure'i virnas muud teenused. Näiteks see administraatori saate sisse logida Azure'i virnas tellimuse luua võlvkelder, kus klahvid talletada ja seejärel nende töötoimingute vastutab organisatsiooni:

- Loomise või importimise klahvi või salajane
- Pääsuõiguste tühistamine või klahvi või salajane kustutamine
- Kasutajate või rakenduste juurde pääseda võtme vault nii, et nad saavad seejärel haldamine või kasutage oma saladusi ja lubada
- Võtme kasutamine konfigureerimine (nt allkirjastamiseks või krüptimiseks)

Selle administraator võib seejärel anda URI-d kõne nende rakenduste arendajatele ja pakkuda turvalisus administraatori võtme kasutamine logiteabe.

Arendajad saate hallata ka võtmed otse API-de abil. Lisateabe saamiseks vt klahvi Vault arendaja juhend.

## <a name="scenarios"></a>Stsenaariumid

Järgmises tabelis on kujutatud mõned stsenaariumid, kus klahvi Vault aitab vajadustele arendajate ja turvalisus administraatorid:


### <a name="developer-for-an-azure-stack-application"></a>Azure'i virnas rakenduse arendaja

**Probleem**: Soovin kirjutada Azure'i virnas, mis kasutab allkirjastamise ja krüptimise võtmed rakenduse, kuid soovin, et neid väljastpoolt minu rakenduse, et lahendus on sobiv rakendus, mis on geograafiliselt jaotatud.

**Lause**: klahvid on talletatud võlvkelder ja millele URI vastavalt vajadusele.


### <a name="developer-for-software-as-a-service-saas"></a>Kui teenus (SaaS) tarkvara arendaja

**Probleem:** Ma ei soovi kohustusi või võimalike vastutuse minu klientide rentniku võtmed ja saladused.

**Lause:** Klientide saate importida oma klahvid Azure'i virnas ja neid hallata. Soovin, et kliendid omanik ja hallata oma võtmed nii, et ma saate keskenduda tehes, mida teha kõige paremini, mis pakub tuum tarkvara funktsioone.


### <a name="chief-security-officer-cso"></a>Turvalisus juht (CSO)

**Probleem:** Soovin veenduge, et minu ettevõte on klahv elutsükli juhtelement ja saate jälgida tootenumbri kasutamine.

**Lause** Klahv Vault on mõeldud nii, et Microsoft vaadata või ekstraktida oma võtmed.  Kui rakendus peab cryptographic toiminguid klientide klahvide abil, võti Vault see rakendus nimel. Rakendus ei kuvata klientide võtmed.  Kuigi me kasutame mitme Azure'i virnas teenuste ja ressursse, soovin haldamine võtmed Azure'i virnas ühest kohast. Vault pakub liidest, olenemata sellest, mitu võlvid teil piirkonnad Azure'i virnas need tugi- ja millised rakendused on neid kasutada.

## <a name="next-steps"></a>Järgmised sammud

[Võtme Vault kasutamise alustamine](azure-stack-kv-getting-started.md)
