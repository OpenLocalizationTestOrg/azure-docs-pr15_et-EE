<properties 
   pageTitle="Ülevaade Microsoft Azure'i andmed Lake Analytics | Azure'i" 
   description="Andmeanalüüsi Lake on Azure Big Data arvutus teenus, mis võimaldab kasutada andmete teie äri abil ülevaateid, mis on saadud andmete pilves, kus sõltumata ning selle suurust. Andmeanalüüsi Lake võimaldab see lihtsaim, kõige scalable ja odavaimal viisil. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Microsoft Azure'i andmed Lake Analytics ülevaade

## <a name="what-is-azure-data-lake-analytics"></a>Mis on Azure andmeanalüüsi Lake?

Azure'i andmeanalüüsi Lake on uus teenus, ehitatud suur andmeanalüüsi lihtsustamiseks. Selle teenuse abil saate keskenduda kirjutamine, töötab ja tööde haldamine, mitte opsüsteem jaotatud taristu. Asemel juurutamine, konfigureerimise ja häälestamise riistvara, peaksite kirjutama päringute muuta andmete ja ekstraktida väärtuslik ülevaateid. Kasutusanalüüsi teenuse saab hakkama tööd, mis tahes skaala kohe palju Power peate sissehelistamise lihtsalt seadmisega. Maksate oma töö kui see töötab, muutes selle kulutõhus. Kasutusanalüüsi teenus toetab Azure Active Directory lastes lihtsalt haldamine Accessi ja rollide, integreeritud kohapealse identiteedi programmiga. See hõlmab ka U-SQL-i, keel, mis ühendab SQL-i eelised väljendusvõimalustega võimsusega kasutaja kood. U SQL scalable jaotatud Runtime'i abil saate tõhusalt analüüside poes ja SQL Azure'i, Azure SQL-andmebaas ja SQL Azure'i andmebaas serverites.


## <a name="key-capabilities"></a>Võtme võimalused

- **Dünaamiline skaala** 

    Andmeanalüüsi Lake on architected maa üles pilve skaala ja jõudluse.  Dünaamiliselt sätted ressursid ja võimaldab teil teha analytics TB või isegi exabait andmeid. Töö lõpetamisel see Tuul alla ressursid automaatselt ja maksate ainult kasutatud töötlemise võimsus. Suurendamiseks või vähendamiseks talletatud andmed suurust või kasutada Arvuta hulk, pole teil vaja kirjutada kood. See võimaldab teil keskenduda oma äriloogika ainult ja kuidas te töötlemine ja suurte andmekogumite talletada. 

- **Kiiremini töötada, silumine ja Optimeeri nutikamalt tuttavate tööriistade abil**

    Andmeanalüüsi Lake on sügav integreerimine Visual Studio nii, et tuttavate tööriistade abil saate käivitada, silumine ja häälestada oma kood. Visualiseeringute oma töökoha U-SQL-i abil saate vaadata, kuidas oma kood töötab skaala, nii, et saate kerge vaevaga tuvastada jõudluse kitsaskohad ja optimeerida kulusid. 

- **A-SQL-is: lihtne ja tuttav, võimas ja laiendatav**

    Andmeanalüüsi Lake sisaldab U-SQL-i, mis ulatub tuttav, lihtne, deklaratiivseid laadi SQL-i C# väljendusvõimalustega võimsusega päringukeele. A-SQL-i keele põhineb sama jaotatud käitusaja volitusi suur andmete süsteemide Microsoft sees. Nüüd saate SQL-i ja .net-i arendajad miljoneid töötlemine ja kõik oskused, mida nad on juba oma andmeid analüüsida.

- **Ühendab sujuvalt oma IT-investeeringute**

    Andmeanalüüsi Lake abil saate oma olemasolevad seda isikut, haldus, turvalisus ja võrgupõhised andmebaasid. See lihtsustab andmete juhtimise ja hõlbustab praeguse andmete rakenduste laiendamine. Andmeanalüüsi Lake on integreeritud Active Directory kasutaja halduse ja õiguste ja kaasas sisseehitatud jälgimine ja auditeerimine.

- **Kättesaadav ja tõhus kulu**

    Andmeanalüüsi Lake on kulutõhus lahendus töötab suur andmed töökoormus. Maksate töö kohta alusel, kui andmeid töödeldakse. Pole riistvara, litsentsid või teenuse kohased tugiteenuste lepingud on vaja. Süsteemi skaala automaatselt üles või alla, nagu töö algab ja lõpule jõudnud, tähendab, et maksate kunagi rohkem kui vajalik. 

- **Oma Azure'i andmetega töötab**

    Andmeanalüüsi Lake saate töötada mitmete Azure'i andmeallikad: Azure'i bloobimälu, Azure SQL-andmebaas ja loomulikult Lake andmeanalüüsi on spetsiaalselt optimeeritud töötamiseks Azure'i andmesalve Lake - esitada kõrgeima jõudlus ja läbilaskevõime parallelization teile suur andmed töökoormus.

## <a name="see-also"></a>Vt ka

- Alustamine
    - [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
    - [Alustamine Lake andmeanalüüsi Azure PowerShelli abil](data-lake-analytics-get-started-powershell.md)
    - [Lake andmeanalüüsi Azure'i .NET SDK kasutamise alustamine](data-lake-analytics-get-started-net-sdk.md)
    - [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure'i andmed Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md)
    
- A-SQL- ja arendustegevus
    - [Azure'i andmed Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md)
    - [Azure'i andmeanalüüsi Lake projektide jaoks kasutada U-SQL-i akna funktsioonid](data-lake-analytics-use-window-functions.md)
    - [Arendamise U-SQL-i kasutaja määratletud tehtemärgid Lake andmeanalüüsi projektide jaoks](data-lake-analytics-u-sql-develop-user-defined-operators.md)
    
- Haldus
    - [Azure'i Lake andmeanalüüsi Azure'i portaalis haldamine](data-lake-analytics-manage-use-portal.md)
    - [Azure'i Lake andmeanalüüsi Azure PowerShelli kaudu hallata](data-lake-analytics-manage-use-powershell.md)
    - [Jälgimine ja Azure andmeanalüüsi Lake töö Azure'i portaalis tõrkeotsing](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
    - [Juurdepääs Azure'i andmed Lake Analyticsi diagnostika logid](data-lake-analytics-diagnostic-logs.md)

- Õppeteema – lõpuni
    - [Kasutage Azure'i andmeanalüüsi Lake interaktiivsed õppematerjalid](data-lake-analytics-use-interactive-tutorials.md)
    - [Veebisaidi logid abil Azure'i andmeanalüüsi Lake analüüs](data-lake-analytics-analyze-weblogs.md)

- Andke meile teada, mida te arvate
    - [Kommenteerida meie dokumentatsiooni mahajäämus](data-lake-analytics-documentation-backlog.md)
    - [Funktsioon taotluse esitamine](http://aka.ms/adlafeedback)
    - [Abi saate tugiteenuste Foorumid](http://aka.ms/adlaforums)


