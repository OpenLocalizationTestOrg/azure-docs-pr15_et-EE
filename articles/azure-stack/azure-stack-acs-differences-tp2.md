
<properties
    pageTitle="Azure'i ühtsete salvestusruumi: erinevusi ja kaalutlused | Microsoft Azure'i"
    description="Mõista nende erinevusi Azure Storage ja muud Azure'i ühtsete salvestusruumi juurutamise kaalutlused."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure'i ühtsete salvestusruumi: erinevused ja kaalutlused

Azure'i ühtsete salvestusruumi on salvestusruumi pilveteenustega Microsoft Azure'i virnas kogum. Azure'i ühtsete salvestusruumi pakub bloobimälu, tabel, kuhjuda ja konto funktsioonid koos Azure ühtsete semantika. Selles artiklis on kokku võetud Azure Storage teadaolevad Azure'i ühtsete salvestusruumi erinevused. Samuti on toodud muud kaalutlused pidage meeles, kui juurutate Microsoft Azure'i virnas praegu avalikult kättesaadavaks eelvaateversiooni.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Teadaolevad erinevused

Selle tehnilise eelvaate versiooni Azure'i ühtsete salvestusruumi ei saa 100% funktsiooni tarkvarakomplektiga Azure Storage API versioonid, mis on toetatud. Teada funktsioonide erinevused on järgmised:

-   Teatud salvestusruumi konto pole veel saadaval, näiteks Standard\_RAGRS ja Standard\_GRS.

-   Premium\_LRS salvestusruumi kontod saab ette valmistatud. Siiski on praegu ole tulemuslikkuse või garantiid.

-   Azure'i failide funktsioonid pole veel saadaval.

-   Saada lehe vahemikud API ei toeta lehti, pilte lehe plekid erinevad toomine.

-   Saada lehe vahemikud API tagastab lehti, millel on 4 granulaarsus KB.

-   Sektsiooni ja rea võti Azure'i ühtsete salvestusruumi tabeli rakendamisel on iga 400 märki (st 800 baiti) maht piiratud.

-   Azure'i ühtsete salvestusruumi bloobimälu teenuse rakendamisel bloobimälu nimed on piiratud 880 suurus märki (st 1760 baiti).

-   Azure'i ühtsete salvestusruumi rakendamine rentniku talletusmahu kasutamine andmete esitamine pakub salvestusruumi kasutuse meetrit, mis on samad Azure, sama semantikat ja üksused. Praegu siiski salvestusruumi tehingud kasutamine arvesti ei sisalda IaaS tehingud ja andmete edastamiseks kasutamine arvesti sise- või võrguliiklust Azure'i virnas piirkonnale eristada läbilaskevõime kasutuse.

-   Teatud funktsioonide ulatus salvestusruumi hallatavust erinevusi. Näiteks ei saa muuta konto tüüp või on kohandatud domeeni. Lisaks on saadaval ainult API taseme järjepidevuse lisatasu\_LRS salvestusruumi konto tüüp.

## <a name="deployment-considerations"></a>Juurutamise kaalutlused

-   **Ainult testimiseks.** Juurutada Azure ühtsete salvestusruumi kasutada Microsoft Azure'i virnas tehnilise eelvaate väljaanne Tootmiskeskkondades. See versioon on mõeldud ainult hindamiseks lab testimiskeskkonnas.

-   **Jõudluse**. Microsoft Azure'i virnas tehnilise eelvaate versiooni Azure'i ühtsete salvestusruumi pole täielikult optimeeritud jõudlus.

-   **Skaleeritavus.** Microsoft Azure'i virnas tehnilise eelvaate versiooni Azure'i ühtsete salvestusruumi on optimeeritud skaleeritavus.

## <a name="version-support-considerations"></a>Peaksite arvesse võtma versiooni tugi

Selle eelvaate Release Azure'i ühtsete salvestusruumi on toetatud järgmised versioonid:

> Azure'i salvestusruumi kliendi teek: [Microsoft Azure salvestusruumi 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure'i salvestusruumi andmeteenuste: [2015-04-05 REST API versioon](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure'i salvestusruumi halduse teenused: [2015-05-01-eelvaade](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Järgmised sammud

-   [Azure'i ühtsete salvestusruumi tutvustus](azure-stack-storage-overview.md)
