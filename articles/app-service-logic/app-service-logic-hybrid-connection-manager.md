<properties 
    pageTitle="Ühenduse hübriid halduris | Microsoft Azure'i" 
    description="Installida ja konfigureerida ühenduse hübriid ülemus ja ühenduse loomiseks kohapealse konnektorid loogika rakendustes" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Ühenduse loomiseks kohapealse konnektorid ühenduse hübriid halduri abil

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon. Loogika rakendused üldiselt kättesaadav (GA) kasutab lüüsi asutusesisese Ühenduvus. Lisateavet uue [lüüsi](app-service-logic-gateway-connection.md) ning [Loogika Apps-GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Kohapealse süsteem kasutamiseks loogika rakenduste kasutab ühenduse hübriid ülemus. Mõned saate luua ühenduse kohapealse süsteemi, nt SQL serveri, SAP-i, SharePointi ja jne. 

Hübriidjuurutuse ühenduse Manager (HCM) on ühe hiireklõpsu-üks kord on installitud IIS-i serveris oma võrgustikus taga tulemüüri installer. Mõne Azure'i teenus siini relay kasutamisel HCM autendib kohapealse süsteemi Azure konnektor. 

> [AZURE.NOTE] Hübriidjuurutuse haldur on vaja ainult siis, kui loote asutusesisese ressursside tulemüüri taha. Kui loote pole kohapealse süsteemi, ei pea ühenduse hübriid ülemus.

Enne alustamist peate.

- Azure'i teenus siini relay nimeruumi SAS ühendusstring. Vt [Teenuse siini hinnad](https://azure.microsoft.com/pricing/details/service-bus/) , milliste määratlemiseks sisaldab releed.
- Kohapealse sisselogimise süsteemiteabe, sh kasutajanime ja parooli. Näiteks kui loote asutusesisese SQL serveriga, peate SQL serveri login konto ja parool.
- Kohapealse serveri teave, sh pordi number ja serveri nimi. Näiteks kui loote asutusesisese SQL serveriga, peate SQL serveri nimi ja TCP pordi number.

## <a name="get-the-service-bus-connection-string"></a>Teenuse siini ühendusstringi hankimine

Azure'i portaalis kopeerimine teenuse siini juurkausta SAS ühendusstring. See ühendusstringi loob teie Azure konnektor oma kohapealse süsteemi. 

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885), valige oma teenuse siini nimeruum ja valige **Ühenduseteavet**.

    ![][SB_ConnectInfo]

2. Kopeerige ühendusstring SAS.

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Hübriidjuurutuse ühenduse halduri installimine

1. Valige [Azure portaali](http://go.microsoft.com/fwlink/p/?LinkID=525040)loodud konnektor. Selle avamiseks saate valige **Sirvi**, valige **API rakendused**ja seejärel valige oma konnektor või API rakenduse. 
<br/><br/>
**Hübriid ühenduse**häälestamine on **Lõpetamata**.
<br/>
![][2] 

2. Valige **hübriid-ühendus**. Varem sisestatud teenuse siini ühendusstring on loetletud.
3. Kopeerige **esmane konfiguratsiooni String**.
<br/>
![][PrimaryConfigString]

4. Jaotises **Kohapealse hübriid haldur**saate alla laadida ühenduse hübriid sõim või selle installida otse portaalis. 
<br/><br/>
Otse portaalis installimiseks valige oma kohapealse IIS-i server, liikuge sirvides portaali ja valige **Laadi alla ja konfigureerimine**.
<br/><br/>
Allalaadimiseks ühenduse hübriid ülemus, minge oma kohapealse IIS-i serveri ja minna **ClickOnce'i rakenduses** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Installimine käivitub automaatselt, siis saate seda kasutada.

5. Aknas **Kuulajale seadistamine** Sisestage **Esmane konfiguratsiooni stringi** kleebitud varem (sammus 3), ja valige, **installida**.

Kui häälestamine on lõpule jõudnud, kuvatakse järgmine:
<br/>
![][3] 

Nüüd sirvimisel konnektor uuesti, hübriid ühenduse olek on **ühendatud**. Teil võib olla konnektor sulgege ja avage see uuesti:
<br/>
![][4] 

> [AZURE.NOTE] Teisene ühendusstringi aktiveerimiseks taaskäivitage installiprogramm ühenduse hübriid ja sisestage **Sekundaarne konfiguratsiooni String**.


## <a name="tcp-ports-and-security"></a>TCP-pordid ja turve

Kui loote ühenduse hübriid, luuakse veebisaidi kohaliku kohapealse IIS-i serveris. IIS-i server võib olla mõne DMZ. TCP pordi IIS-i serveris sisaldavad:

TCP Port | Miks
--- | ---
 | Pole sissetuleva TCP-pordid on nõutav.
9350 - 9354 | Järgmised pordid kasutatakse andmete edastamiseks. Teenuse siini relay halduri sondid pordi 9350 määratlemiseks, kui TCP-ühendus on olemas. Kui see on saadaval, siis eeldatakse, et pordi 9352 on saadaval ka. Andmete liiklus läheb Port 9352. <br/><br/>Lubage Väljaminevad ühendused järgmised pordid.
5671 | Pordi 9352 kasutamisel andmete liikluse pordi 5671 kasutatakse selle kanali juhtelement. <br/><br/>Lubage Väljaminevad ühendused selles pordi. 
80, 443 | Kui pordid 9352 ja 5671 ei tööta, *siis* pordid 80 ja 443 on taandepäringud pordid, andmeedastuse ja juhtelemendi kanali jaoks.<br/><br/>Lubage Väljaminevad ühendused järgmised pordid.
Kohapeal süsteemi port | Avage arvutis asutusesisese pordi, kasutab. Näiteks SQL Server kasutab tavaliselt port 1433. Avage TCP-port.

[Teenuse siini koos tulemüüriga majutusteenuse](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Tõrkeotsing

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Kohapealse tõrkeotsing

1. Kinnitage IIS-i server, IIS web roll on installitud ja käivitatud IIS-i teenuseid.
2. IIS-i server, kinnitage hü ühenduse haldur on installitud ja käivitatud:
 - IIS-i halduris (inetmgr), ***MicrosoftAzureBizTalkHybridListener*** veebisaidi peaks olema lisatud ja töötama. 
 - See veebisait kasutab ***HybridListenerAppPool*** , mis töötab *NetworkService'i* kohaliku sisseehitatud kasutajakonto. See AppPool ka alustada.
3. IIS-i server, kontrollige, kas konnektor on installitud ja käivitatud: 
 - Veebisait on loodud oma konnektor. Näiteks kui olete loonud konnektor SQL-i, on ***MicrosoftSqlConnector_nnn*** veebileht. IIS-i halduris (inetmgr), veenduge, et see veebisait on loetletud ja alustada. 
 - See veebisait kasutab oma IIS-rakenduste komplekti nimega ***HybridAppPoolnnn***. See AppPool töötab *NetworkService'i* kohaliku sisseehitatud kasutajakonto. See veebisait ja AppPool mõlemad alustada. 
 - Sirvige kohaliku konnektor. Näiteks kui veebisaidi Konnektori kasutab pordi 6569, liikuge sirvides http://localhost:6569. Vaikimisi dokumendi pole konfigureeritud nii, et mõne `HTTP Error 403.14 - Forbidden error` peaks.
4. Kinnitage oma tulemüüri selles teemas loetletud TCP-pordid on avatud.
5. Vaadake lähte- või sihtkoha süsteem.
 - Mõned kohapealse nõuavad täiendavate sõltuvus failid. Näiteks kui loote ühenduse kohapealse SAP-i, mõned täiendavad SAP-i failid peab olema installitud IIS-i serveris.
 - Kontrollimiseks süsteemile kontoga sisselogimine. Näiteks, kasutab TCP pordi peab olema avatud, nagu port 1433 SQL serveri korral. Azure'i portaalis sisestatud sisselogimist konto peab olema võrgule.
6. IIS-i server, märkige ruut sündmuselogide jaoks vigu. 
7. Puhastus ja hü ühenduse halduri uuesti: 
 - IIS-i, käsitsi kustutada konnektor veebisaidi ja selle rakenduse pool. 
 - Käivitage uuesti ühenduse hübriid ülemus ja veenduge, et sisestate õige **Esmane konfiguratsiooni stringi** oma konnektor.



### <a name="in-the-azure-classic-portal"></a>Azure'i klassikaline portaalis

1. Veenduge, et teenus siini nimeruum on on **aktiivne** olekus.
2. Kui loote konnektor, sisestage teenuse siini SAS ühendusstring. Sisestage ühendusstring ACS.


## <a name="faq"></a>KKK

**Küsimus**: on kaks hübriid ühenduse haldurid. Mille poolest? 

**Answer (vastus)**: on [Hübriid ühendused](../biztalk-services/integration-hybrid-connection-overview.md) tehnoloogia, mida kasutatakse peamiselt veebirakenduste (varem veebisaitide) ja mobiilirakenduste (varem mobiilsideseadmete teenused) ühenduse loomiseks kohapealse. Selles hübriid ühendused juhataja on eraldi [häälestamise](../biztalk-services/integration-hybrid-connection-create-manage.md) ja kasutab teenuse Azure BizTalki (taustal). See toetab ainult TCP ja HTTP protokolle.

Azure'i rakendust Service konnektorid, kus meil hübriid ühenduse haldur.  See hübriid ühenduse juht ei *ei* Kasuta mõni Azure BizTalki teenuse (taustal) ja toetab rohkem kui TCP ja HTTP protokolle. Vaadake teemat [konnektorid ja API rakenduste loendis](app-service-logic-connectors-list.md).

Azure'i teenus siini abil nii kohapealse süsteemiga ühenduse loomiseks.

**Küsimus**: kohandatud API rakenduse loomisel saate kasutada rakenduse Service Manager hübriid ühenduse ühenduse loomiseks kohapealse? 

**Answer (vastus)**: pole selles traditsiooniline mõttes. Saate kasutada sisseehitatud konnektor, rakenduse teenuse hübriid ühenduse haldur ühenduse loomiseks kohapealse süsteemi konfigureerimine. Seejärel kasutage seda konnektor kohandatud API rakendus, võib-olla loogika rakenduse kasutamine. Praegu ei saa arendada või luua oma hübriid API rakendus (nt SQL-i konnektori või faili konnektor).

Kui teie kohandatud API kasutab TCP või HTTP portide, saate kasutada [Hübriid ühendusi](../biztalk-services/integration-hybrid-connection-overview.md) ja selle hübriid haldur. Selle stsenaariumi korral kasutatakse teenuse Azure BizTalki. [Ühenduse loomine asutusesisese SQL serveri web Appist](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) võib aidata.  


## <a name="read-more"></a>Lisateave

[Loogika rakenduste jälgimine](app-service-logic-monitor-your-logic-apps.md)<br/>
[Teenuse siini hinnad](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
