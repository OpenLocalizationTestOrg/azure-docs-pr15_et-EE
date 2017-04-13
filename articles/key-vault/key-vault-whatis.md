<properties
    pageTitle="Mis on Azure klahvi Vault? | Microsoft Azure'i"
    description="Azure'i klahvi Vault aitab kaitse cryptographic võtmed ja saladusi kasutavad cloud rakenduste ja teenuste. Azure'i klahvi Vault abil saate klientide krüptida võtmed ja saladused (nt autentimise klahvid, salvestusruumi konto klahvid, andmete krüptimise võtmed. PFX-failid ja paroolid) kasutades klahve, mis on kaitstud riistvara turvalisus moodulid (HSMs)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Mis on Azure klahvi Vault?

Azure'i klahvi Vault on saadaval enamikes piirkondades. Lisateabe saamiseks lugege teemat [klahvi Vault hinnad lehe](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Sissejuhatus

Azure'i klahvi Vault aitab kaitse cryptographic võtmed ja saladusi kasutavad cloud rakenduste ja teenuste. Klahv Vault abil saate krüptida võtmed ja saladused (nt autentimise klahvid, salvestusruumi konto klahvid, andmete krüptimise võtmed. PFX-failid ja paroolid) kasutades klahve, mis on kaitstud riistvara turvalisus moodulid (HSMs). Lisatud assurance, saate importida või võtmed HSMs sisse. Kui te ei soovi seda teha, Microsoft protsessid oma võtmed FIPS 140-2 tase 2 valideeritud HSMs (riistvara ja püsivara).  

Klahv hoidla muudab võtme haldamise protsessi ja võimaldab teil hallata tutvustatakse, mida juurdepääsu ja andmete krüptimine. Arendajad saate luua klahvid katsetamine minutiga, ja seejärel sujuvalt üle tootmise võtmed. Turvalisus administraatorid saate anda ja tühistada õigus klahvid, vastavalt vajadusele.

Kasutage järgmise tabeli abil saate paremini mõista, kuidas klahvi Vault aitab vajadustele arendajate ja turvalisus administraatorid.





| Roll        | Probleemi kirjeldus           | Azure'i võtme Vault lahendada  |
| ------------- |-------------|-----|
| Azure'i rakenduse arendaja      | "Soovin kirjutada Azure, mis kasutab allkirjastamise ja krüptimise võtmed rakenduse, kuid soovin, et need võtmed olema väljastpoolt minu rakenduse, et lahendus on sobiv rakendus, mis on geograafiliselt jaotatud. <br/><br/>Soovin neid võtmed ja saladusi kaitsta, ilma vajaduseta kirjutada koodi ise. Samuti soovin nende võtmed ja saladusi lihtne minu jaoks kasutada oma rakendustest optimaalse jõudluse tagamiseks." | √ Klahvid võlvkelder talletatakse ja millele URI vastavalt vajadusele.<br/><br/> √ Klahvid kaitsevad Azure, kasutades tööstusharu standardile algoritmide kohta, võtme pikkusega ja riistvara turvalisus moodulid (HSMs).<br/><br/> √ Klahvid töödeldakse HSMs, mis asuvad samas Azure andmekeskuste nimega rakendusi. See annab parema töökindluse ja kui kui võtmed asuvad eraldi asukohta, nt kohapealse lühendatud latentsusaeg.|
| Kui teenus (SaaS) tarkvara arendaja      |"Ma ei soovi kohustusi või võimalike vastutuse minu klientide rentniku võtmed ja saladusi. <br/><br/>Soovin omanik ja hallata oma võtmed nii, et ma saate keskenduda tehes, mida teha, mis pakub tuum tarkvara funktsioonid kliendid." | √ Kliendid saate importida oma klahvid Azure ja neid hallata. Kui SaaS rakendus peab klientide klahvide abil cryptographic toiminguid, ei klahvi Vault rakenduse asemel neid toiminguid. Rakendus ei kuvata klientide võtmed.|
| Peamine turvalisuse ametniku | "Soovin teada, et meie rakenduste vastavad FIPS 140-2 tase 2 HSMs turvaline võtme haldamiseks. <br/><br/>Soovin veenduge, et minu ettevõte on klahv elutsükli juhtelement ja saate jälgida tootenumbri kasutamine. <br/><br/>Ja kuigi me kasutame mitme Azure'i teenuste ja ressursse, soovin haldamine võtmed Azure ühest kohast."     |√ HSMs on FIPS 140-2 tase 2 kinnitatud.<br/><br/>√ Klahvi Vault on mõeldud nii, et Microsoft vaadata või ekstraktida oma võtmed.<br/><br/>√ Lähedal reaalajas logimist võtme kasutamine.<br/><br/>√ vault pakub liidest, olenemata sellest, mitu võlvid on Azure piirkonnad need tugi- ja millised rakendused on neid kasutada. |


Igaüks Azure tellimusega saate luua ja kasutada võtme võlvid. Kuigi klahvi Vault kasu arendajad ja turvalisus administraatorid, võib see rakendada ja hallata ettevõtte administraator, kes haldab muud Azure teenused ettevõtte. Näiteks see administraatori oleks logige sisse Azure tellimuse luua võlvkelder organisatsiooni, kus klahvid talletada ja seejärel vastutab tööülesanded, näiteks:

+ Loomise või importimise klahvi või salajane
+ Pääsuõiguste tühistamine või klahvi või salajane kustutamine
+ Kasutajate või rakenduste juurde pääseda võtme vault nii, et nad saavad seejärel haldamine või kasutage oma saladusi ja lubada
+ Võtme kasutamine konfigureerimine (nt allkirjastamiseks või krüptimiseks)
+ Kuvari võtme kasutamine

See administraatori seejärel anda URI-d kõne nende rakenduste arendajatele ja anda oma turvalisus administraatori võtme logimine teavet. 

   ![Azure'i võtme Vault ülevaade][1]

Arendajad saate hallata ka võtmed otse API-de abil. Lisateabe saamiseks vt [klahvi Vault arendaja juhend](key-vault-developers-guide.md).

## <a name="next-steps"></a>Järgmised sammud

Saamisega alustamise õpetus administraator, leiate [Azure'i klahvi Vault alustamine](key-vault-get-started.md).

Logimise klahvi Vault kasutuse kohta leiate lisateavet teemast [Azure klahvi Vault logimine](key-vault-logging.md).

Azure'i klahvi Vault võtmed ja saladused kasutamise kohta leiate lisateavet teemast [kohta klahvid, saladusi, ja serdid](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
