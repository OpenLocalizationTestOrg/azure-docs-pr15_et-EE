<properties 
    pageTitle="Azure'i bloobimälu ümbriste, SQL Server, klõpsake näidisandmed ja taru tabelite | Microsoft Azure'i" 
    description="Kuidas uurida erinevate Azure enviromnents talletatud andmed." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Azure'i bloobimälu ümbriste, SQL Server, klõpsake näidisandmed ja taru tabelid

Selle dokumendi teemade lingid, mis hõlmab, kuidas näidisandmed, mis on talletatud kolme eri Azure kohtades:

- **Azure'i bloobimälu container andmete** võetakse allalaadimist programmiliselt ja seejärel proovide selle valimi Python koodiga.
- **SQL serveri andmetega** võetakse nii SQL-i ja Python Programmeerimine keele abil. 
- **Tabeliandmete taru** võetakse taru päringute abil.

**Menüü** allpool kuidas näidisandmed iga nendes keskkondades Azure storage kirjeldavate teemade lingid. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Selle toimingu valimite on samm [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="why-sample-data"></a>Miks näidisandmed?

Kui kavatsete analüüsida andmekomplekti on suur, on tavaliselt on hea mõte kasutada alla valimiga väiksem, kuid esindaja ja paremini hallataval mahu vähendamiseks andmed. See hõlbustab andmete mõistmine, avastamine ja funktsioon matemaatika erifunktsioonid. Selle rolli Cortana Analytics protsess on kiire prototüüpimine töötlemise funktsioonidest ja seadme õppe mudelid lubamine.



