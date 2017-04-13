<properties 
   pageTitle="Kasutus- ja Azure otsingu teenuse statistika jälgimine | Microsoft Azure'i | Majutatud pilveteenuses otsing" 
   description="Jälgida majutatud cloud otsinguteenuse Microsoft Azure Azure'i otsinguks ressursside tarbimine ja registri suurus." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Jälgida kasutust ja Azure otsingu teenuse statistika

Registrid ja dokumendimaht jälgimine aitab teil aktiivselt reguleerida võimsus enne pihta olete loonud teie teenuse jaoks ülempiiri. 

Ressursikasutus jälgimiseks loendab ja statistika teie teenuse jaoks on hõlpsalt vaadata [Azure portaali](https://portal.azure.com), kuid saate hankida teavet programmiliselt kui on kohandatud teenuse halduse tööriista loomine. Selles artiklis käsitletakse nii tehnika juhised.

Saate uue otsingu liikluse analytics funktsiooni index tasemel teadmisi jaoks. [Otsingu liikluse Analytics Azure'i otsingu](search-traffic-analytics.md) alustamiseks külastage.

##<a name="view-counts-and-metrics-in-the-portal"></a>Loendab ja mõõdikute kuvamine portaalis 

1. [Azure'i portaali](https://portal.azure.com)sisse logima. 

2. Avage oma Azure'i otsinguteenuse teenuse armatuurlaud. Paanide teenuse kohta leiate avalehel või sirvimiseks teenusega on JumpBar Sirvi. Üksikasjalikud juhised leiate teemast [loomine teenuse](search-create-service-portal.md) .

Kasutus sisaldab arvesti, mis ütleb teile, millist osa saadaval ressursid on praegu kasutusel.

  ![][1]

Võta tagasi, et ühisteenuste on kuni ühe koopia ja partition. Peale selle kokku või 50 MB andmeid toetab ainult 10 000 dokumendi, kumb on esimene.

##<a name="get-index-statistics-using-the-rest-api"></a>Saada register statistika REST API abil

Azure'i Search REST API- ja .NET SDK pakuvad teenuse mõõdikute Programmeerimisjuurdepääs.  Kui kasutate [indexers](https://msdn.microsoft.com/library/azure/dn946891.aspx) registri laadida Azure'i SQL-andmebaasi või DocumentDB, täiendavad API on saadaval vajate arvude. 

  + [Saada register statistika](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Dokumentide arv](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Saada registraatori olekut](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Järgmised sammud

Vaadake üle [piirangud ja võimsus](search-limits-quotas-capacity.md) määratlemiseks sektsioonid ja peate, kui olemasolev ei piisa koopiad. 

Teenuse haldus kohta lisateabe saamiseks külastage [haldamine oma otsingu teenuse Microsoft Azure](search-manage.md) .

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
