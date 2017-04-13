<properties 
    pageTitle="QuickBooksi Azure RemoteApp juurutamine | Microsoft Azure'i" 
    description="Saate teada, kuidas Azure RemoteApp QuickBooksi ühiskasutusse anda." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Kui juurutate QuickBooksi Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kasutage järgmist teavet jagada QuickBooksi rakenduse Azure RemoteApp.


Saate jagada QuickBooksi 2015 Enterprise Azure RemoteApp hübriid või pilveteenuse kogumise. Ettevõtte fail peab asuma VM, kus töötab QuickBooksi andmebaasi server, mis on Azure RemoteApp serverid eraldi. Ärge hoidke ettevõtte faili Azure RemoteApp pilt - andmekao eeldatakse, kui te seda. Ainult QuickBooksi Enterprise toetab hosting QuickBooksi faili on väline ühiskasutus QuickBooksi andmebaasiserveri standardse Windowsi võrgu kaudu.   

> [AZURE.IMPORTANT] QuickBooksi andmebaasiserveri hostib ettevõtte fail peab asuma eraldi VM sama VNET nimega Azure RemoteApp saidikogumi sees.  

## <a name="steps-to-deploy-quickbooks"></a>Juhised QuickBooksi juurutamine

1. Azure'i VM-i loomine ja QuickBooksi QuickBooksi andmebaasiserver, installige ja asetage ettevõtte faili Azure VM.  Veenduge, et õigesti konfigureerida tulemüüri reeglid.
2. QuickBooksi installimine on [kohandatud pildi](remoteapp-imageoptions.md) ja luua mõni [Azure RemoteApp saidikogumi](remoteapp-collections.md), pilve- või hübriid täpse sama VNET, kus asub majutusteenuse QuickBooksi andmebaasiserveri ettevõtte failidega VM sees. 
3.  [Avaldamine](remoteapp-publish.md) QuickBooksi rakenduse kasutajatele
4.  Käivitada kliendi Azure RemoteApp majutatud QuickBooksi, liikuge standard Windows networking majutusteenuse QuickBooksi andmebaasiserveri VM abil ja ettevõtte faili avada. 

## <a name="documentation-references"></a>Dokumentide viited

- QuickBooksi [konfiguratsioone toetatud](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooksi [Juurutussuvandid](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Saate vaadata ka minu Ignite esitluse, [põhikomponentide Microsoft Azure RemoteApp juhtimis- ja halduskulud](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) – kuni 1:02:45 QuickBooksi osa saada edasi.

## <a name="deployment-architecture"></a>Juurutamise arhitektuur

![QuickBooksi + Azure RemoteApp juurutamine](./media/remoteapp-quickbooks/ra-quickbooks.png)