<properties 
    pageTitle="Azure RemoteApp tõrkeotsing – rakenduse käivitamine ja ühenduse tõrkeid | Microsoft Azure'i" 
    description="Saate teada, kuidas käivitamiseks ja Azure RemoteApp rakenduste ühendamiseks seotud probleemide tõrkeotsing." 
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



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Tõrkeotsing: Azure RemoteApp - rakenduse käivitamine ja ühenduse tõrkeid. 

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp rakendused ei pruugi mõne põhjustel käivitada. Selles artiklis kirjeldatakse erinevad põhjused ja tõrketeated, mis võivad kasutajad saavad, kui proovite käivitada rakendusi. Samuti räägib ühenduse tõrkeid. (Kuid selles artiklis kirjeldatud probleemid, rakendusse Azure RemoteApp kliendi).  

Levinud tõrketeated tõttu rakenduse käivitamine ja ühenduse tõrgete kohta teabe saamiseks lugege edasi.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Otsustasime häälestamisel... Proovige uuesti.

See tõrge tähendab Azure RemoteApp on ülespoole võimsus vajadust kasutajaid. Taustal Azure RemoteApp VM mitu eksemplari luuakse käsitlema oma kasutajate vajadustega võimsus. Tavaliselt see võtab aega umbes viis minutit, kuid võib võtta aega kuni 10 minutit. Mõnikord seda ei juhtu piisavalt kiiresti ja ressursid kohe. Näide 9: 00 stsenaarium kus paljud kasutajad peavad rakenduse kasutamine Azure RemoteAppn samal ajal. Kui see juhtub me lubada **võimsus režiimi** tagasi lõpuks. Selle tegemiseks ja avage mõni Azure tugiteenuste Piletite või saada meile aadressil [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Olla kindel, et kaasata oma tellimuse ID taotluse.  

![Saate häälestada saame](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Kas automaatne uuesti rakenduste, lugege uuesti oma rakenduse käivitamine  

See tõrketeade kuvatakse on tihti kui kasutasite Azure RemoteApp ja seejärel panete arvuti unerežiimi kauem kui neli tundi ja seejärel ärkasin PC-arvutis ja Azure RemoteApp kliendi proovivad automaatselt uuesti ja ajalõpp ületati.  Paluge kasutajatel tagasiliikumiseks rakendus ja selle avamine klientrakenduses Azure RemoteApp.

![Võiks automaatne-taastada rakenduste](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Temp profiili probleemid 

See tõrge ilmneb juhul, kui ühendamine nurjus kasutajaprofiili (kasutaja profiili ketas) ja kasutajale saanud ajutist profiili.  Administraatorid peaksid liikuge saidikogumi Azure portaali ja seejärel avage vahekaart **seansid** ja proovige **Logi välja** kasutaja. See jõustada täielik Logi välja kasutaja seansi - siis on kasutaja, käivitage rakendus uuesti proovida. Mis nurjumisel Azure tugiteenuste ja või saada meile aadressil [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp on lõpetanud töötamise

See tõrketeade kuvatakse tähendab, et Azure RemoteApp klient, millel on probleem ja tuleb taaskäivitada. Paluge kasutajatel sulgeda: valige **sulgege programm** ja seejärel käivitage uuesti Azure RemoteApp kliendi.  Kui probleem ei lahene, Ava ja Azure tugiteenuste Piletite ja või saada meile aadressil [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp on lõpetanud töötamise](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Kaugtöölaua ühendus on juurdepääs selle ressursi ilmnes tõrge. Klõpsake ühenduse uuesti või pöörduge oma süsteemiadministraatori poole.

See on üldise tõrketeate – Azure tugiteenuste ja või saada meile aadressil [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) nii, et saate uurime. 

![Üldine Azure RemoteApp sõnum](./media/remoteapp-apptrouble/ra-apptrouble4.png) 