<properties
    pageTitle="Mida korral on Azure teenuse häirete, mis mõjutab Azure'i klahvi Vault | Microsoft Azure'i"
    description="Vaadake, mida teha Azure teenuse häirete, mis mõjutab Azure'i klahvi Vault korral."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure'i klahvi Vault-saadavus ja koondamine

Azure'i klahvi Vault funktsioonid mitut tüüpi koondamise veenduge, et teie võtmed ja saladused jäävad kättesaadavaks rakenduse isegi siis, kui üksikute osade teenuse nurjuda.

Teie olulisi vault sisu on kopeeritud piirkonnas ja teisene alale vähemalt 150 miili eemal, kuid samas geograafia. See säilitab teie võtmed ja saladused kõrge kestvus.

Kui üksikute osade võtme vault teenuses ei õnnestu, alternatiivse komponendid piirkonnas samm olla teie taotlus veenduge, et pole degradeerumine funktsionaalsust. Teil pole vaja vastu võtta selle käivitamiseks. See juhtub automaatselt ja läbipaistev.

Sel juhul, et kogu Azure piirkonna pole saadaval, on tehtud Azure'i klahvi Vault selle piirkonna taotlused, automaatselt suunatud (*üle nurjus*) teisene piirkond. Kui esmane regioon on uuesti, taotlused marsruuditakse tagasi esmane alale (*nurjus tagasi*). Uuesti, pole vaja vastu võtta, kuna see toimub automaatselt.

On mõned piirangud, millega peaks arvestama.

* Piirkonna Tõrkesiirde korral võib kuluda paar minutit teenust kasutada. Selle aja jooksul tehtud taotlusi ei pruugi kuni selle Tõrkesiirde on lõpule viidud.
* Kui Tõrkesiirde on lõpule jõudnud, on teie olulisi vault kirjutuskaitstud režiimis. Taotlusi, mis on toetatud selles režiimis on:
 * Loendi võtme võlvid
 * Saada võtme võlvid atribuudid
 * Loendi saladusi
 * Saada saladusi
 * Loendi võtmed
 * Saada (Atribuudid) võtmed
 * Krüptimine
 * Dekrüptimine
 * Mähkimine
 * Lahtipakkimiseks
 * Kontrollige
 * Logi
 * Varundus
* Pärast Tõrkesiirde uuesti installimine nurjus, taotluse kõigi (sh lugeda *ja* kirjutada taotlusi) on saadaval.
