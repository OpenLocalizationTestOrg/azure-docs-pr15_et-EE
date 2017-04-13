<properties 
    pageTitle="Mis on meeskonnatöö andmete teadus protsessi?  | Microsoft Azure'i" 
    description="Andmete meeskonna teadus protsess on nutikad rakenduste, mida kasutada täiustatud analüüsi süstemaatilised meetod." 
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
    ms.author="bradsev" /> 


# <a name="what-is-the-team-data-science-process-tdsp"></a>Mis on meeskonnatöö andmete teadus protsess (TDSP)?

[Meeskonnatöö andmete teadus protsess (TDSP)](data-science-process-overview.md) pakub süsteemne lähenemine nutikad rakenduste loomine, mis võimaldab meeskondadel andmeteadlaste üle täielik elutsükli vajalikke toiminguid, et lülitada nende toodete rakendusi tõhus koostöö tegemiseks. Funktsiooni TDSP kirjeldab toiminguid, mis pakuvad **juhiseid** määratlemine probleem, häälestada tööriistu ja keskkond, mis on vajalik, asjakohaste andmete analüüsimine, koostamine ja hindamine prognoosmudelite ja seejärel juurutada enterprise rakendustes mudelite jada. 

Siin on juhised **meeskonnatöö teadus**käigus.  

![OTS-töövoo](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Protsess on **iteratiivne**: mõistmist uute ja olemasolevate või täiustati mudeli areneb ja nõuab ümber varem lõpule järjekord juhiseid. Olemasoleva ettevõtte arendamine ja projekti kavandamise protsessid on töötamiseks etappide jada TDSP määratletud **hõlpsalt kohandada** . 

Protsessi etappide diagramm ja lingitud [TDSP õppeteema](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja allpool kirjeldatud.  

## <a name="preparation-steps"></a>Ettevalmistavaid toiminguid 

## <a name="p1-plan-the-analytics-project"></a>P1. Kasutusanalüüsi projekti plaani 

Käivitage mõnda analytics projekti määratledes oma ettevõtte eesmärkide ja probleemide korral. Need on määratud nii **ettevõtte nõuetele**. Selle etapi keskne eesmärk on saada oluliste muutujate (eelarve või tõenäosuse, mis järjestuses pettusega, nt), mis analüüsi tuleb prognoosida, nendele nõuetele. Täiendavad siis tavaliselt oluline on **andmeallikate** tegelemiseks projekti eesmärgid analytical perspektiivi vaja mõista. See pole aeg-ajalt, näiteks olemasolevate tuleb koguda ja logige erinevat tüüpi andmete probleemi lahendamiseks ja projekti eesmärkide leidmiseks. Juhised leiate teemast [andmete meeskonna teadus protsessi keskkonna kavandamine](machine-learning-data-science-plan-your-environment.md) ja [täiustatud analüüsi Azure seadme õppe stsenaariumid](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Häälestamise analytics keskkonnas 

Keskkonnas analytics andmete meeskonna teadus protsessi hõlmab mitu osa: 

- **andmete tööruumid** , kui andmed on etapiviisilise analüüsimise ja modelleerimise, jaoks 
- mõne **töötlemine taristu** eeltöötlus, uurida ja andmete modelleerimine
- **käitusaja taristu** kiireks analytical mudelite ja käivitada kliendi nutikad rakendusi, mis kasutavad mudelid.  

Kasutusanalüüsi taristu, tuleb häälestus on sageli keskkond, mis on eraldatud core süsteemide osa. Kuid see mõjutab tavaliselt mitme süsteemid ettevõttes kui ka ettevõtte välistest allikatest andmeid. Kasutusanalüüsi taristu võib olla puhtalt pilvepõhist, või mõne kohapealse häälestamine või kahe hübriid. Suvandid, leiate teemast [andmete teadus keskkonnas kasutada andmete meeskonna teadus protsessi häälestamine](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Kasutusanalüüsi järgmiselt:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. neelata andmete analytical keskkonda 

Esimese asjana erinevatest allikatest, kas seest kui väljastpoolt ettevõtet, asjakohaste andmete toomiseks on analüütiline keskkonnas, kus saab töödelda andmeid. Andmete allika **Vorming** võib erineda nõutud sihtkoht vorming. Nii mõned andmed teisendus olla ka manustamisest instrumentaarium teha. Suvandid, vaadake [salvestusruumi keskkonnas Analyticsi andmete laadimine](machine-learning-data-science-ingest-data.md)

Lisaks andmete algse manustamisest, paljud nutikad rakendused on vaja regulaarselt olevate andmete värskendamiseks on poolelioleva õ käigus. Seda saab teha **andmete kohaletoimetamisel** või töövoo häälestamiseks. See osa iteratiivne protsess, mis sisaldab taastamine ja uuesti analytical mudelid lahenduse juurutamine nutikad rakenduse kasutatavad hindamise osa. Vt näiteks [andmete SQL Azure'i koos Azure'i andmed Factory kohapealne SQL Serverist](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2 uurimine ja eelnevalt andmete töötlemiseks 

Järgmiseks on saada süvitsi mõistmine andmete uurides **Kokkuvõte, statistika** , seoste ning meetodite abil selliste **visualiseeringu**. See on ka, kus **andmete kvaliteedi** ja terviklus, nt puuduvate väärtuste, andmete tüüpi vastuolude ja vastuolulised kogemused andmete seoseid, probleeme käsitletakse. Eelnevalt töötlemiseks teisendusteta kasutatakse enne täiendavate analytics toorandmetega puhastada ja modelleerimine võib toimuda. Kirjelduse leiate teemast [ülesannete ettevalmistamiseks andmete täiustatud seadme Õppekeskuse](machine-learning-data-science-prepare-data.md).


## <a name="3-develop-features"></a>3. töötada funktsioonid 

Andmeteadlaste koostöös domeeni asjatundjate, tuleb määratleda funktsioonid, mis jäädvustada olulisemaid atribuutide andmehulga ja mida saab kasutada kõige paremini prognoosida oluliste muutujate kavandamise käigus. Järgmisi funktsioone saab tuletada olemasolevate andmete põhjal või võib olla vaja andmete kogumine. Selle protsessi nimetatakse **funktsiooni matemaatika** ja on üks klahv juhiseid koostamise ennustav tegelik süsteem. Selle toimingu jaoks on vaja domeeni teadmisi loominguline kombinatsiooni ja ülevaateid, mis on saadud andmete uurimine etappi. Juhised leiate teemast [funktsioon andmete meeskonna teadus protsessi matemaatika erifunktsioonid](machine-learning-data-science-create-features.md).


## <a name="4-create-predictive-models"></a>4. prognoosmudelite loomine 

Andmeteadlaste koostada analytical mudelite prognoosida kavandamise toimingut kasutades puhastada andmeid ja featurized määratletud äri nõuetele tuvastatud olulisi muutujaid. Seadme õppe süsteemide toetavad mitme **modelleerimine algoritmide** kohaldatavaid erinevaid kaasuste. Juhised leiate teemast [meeskonnatöö Azure seadme õ algoritmid nuppu](machine-learning-algorithm-choice.md).

Andmeteadlaste peate valima ennustamine tegevuse jaoks sobivaim mudeli ja see ei ole aeg-ajalt, et mitme mudelite tulemusi tuleb parimate tulemuste saamiseks. Sisendandmete modelleerimiseks tavaliselt on kolm osa CEIP jagatud:

- koolitus andmehulga 
- valideerimine andmehulgas. 
- testimise andmehulgas. 

Mudelid on loodud, kasutades **koolitus andmehulgas**. Optimaalne kombinatsioon mudelite (parameetritega häälestatud) on märgitud töötab mudeleid ja mõõte ennustamine tõrked **valideerimine andmehulgas**. Lõpuks **andmehulga testimiseks** kasutatakse hindab valitud mudeli sõltumatu andmete pole kasutatud koolitada või kinnitage mudel.  Toimingute, vaadake, [Kuidas hinnata mudeli jõudluse Azure seadme õ](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-models"></a>5. juurutada ja tarbimine mudelite 

Kui oleme hästi mudeleid kogumi, võivad need **operationalized** muude rakenduste kasutamine. Sõltuvalt ettevõtte nõuetele, tehakse prognoose **reaalajas** või **paketi** alusel. Operationalized, et mudelitel koos mõne **avamine API kasutajaliidese** hõlpsalt tarbitud erinevate rakendustest kokku puutuda selline veebisaidile, arvutustabelite, armatuurlaudu ja rea äri-ja taustväärtus. Lugege teemat [Deploy teenuse Azure seadme õ web](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Kokkuvõte ja järgmised toimingud

[Meeskonnatöö andmete teadus protsess](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) on modelleerida nagu näidata etappide jada selle **pakuvad juhiseid** tööülesanded vaja täiustatud analüüsi abil saate luua ka nutikad rakendusi. Iga toimingu näeb üksikasjade kohta, kuidas kasutada Microsoft tehnoloogiaid kirjeldatud toimingu lõpuleviimiseks. 

TDSP ei näe ette kindlat tüüpi **dokumentide** artefakte, on parim dokumendi andmete uurimine, modelleerimise ja hindamise tulemuste ja asjakohased koodi salvestada, et analüüsi saab näidata, kui see on nõutav. See võimaldab taaskasutuse analytics töö kui muude rakenduste sarnaseid andmeid ja ennustamine ülesannete kallal.

Täielik end-to-end juhendavad tutvustused, mis illustreerivad kõigi vajalike toimingute tegemiseks **teatud stsenaariumide** protsess on olemas. Vaadake, näiteks:

- [Meeskonnatöö andmete teadus protsessi tegelikkuses: SQL serveri kasutamine](machine-learning-data-science-process-sql-walkthrough.md)
- [Meeskonnatöö andmete teadus protsessi tegelikkuses: Hdinsightiga Hadoopi kogumite abil](machine-learning-data-science-process-hive-walkthrough.md).
- [Andmete teadus Azure'i HD.mdnsight säde kasutamine](machine-learning-data-science-spark-overview.md)
- [Skaleeritav andmete teadus Azure'i andmed Lake: on-lõpuni kiirtutvustus](machine-learning-data-science-process-data-lake-walkthrough.md)

 
