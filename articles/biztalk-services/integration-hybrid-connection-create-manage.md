<properties 
    pageTitle="Hübriidjuurutuse ühenduste loomine ja haldamine | Microsoft Azure'i" 
    description="Saate teada, kuidas luua ühenduse hübriid, hallata ühenduse ja hübriid ühenduse halduri installimine. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Hübriidjuurutuse ühenduste loomine ja haldamine


## <a name="overview-of-the-steps"></a>Etappide ülevaade
1. Hübriidjuurutuse ühenduse loomisel, sisestades oma privaatvõrgu **hosti nimi** või **FQDN** asutusesisese ressursi.
2. Link oma Azure veebirakenduste või Azure mobiilirakendused ühenduse hübriid.
3. Teie kohapealse ressursi hübriid ühenduse halduri installimine ja ühendage teatud ühenduse hübriid. Azure'i portaalis on ühe klõpsuga kogemus installida ja ühendamine.
4. Hallata hübriid ühendused ja nende ühenduse võtmed.

Selles teemas on loetletud järgmiselt. 

> [AZURE.IMPORTANT] On võimalik määrata ühenduse hübriid endpoint IP-aadressi. Kui kasutate IP-aadress, võib või kohapealse ressurss, ei pruugi sõltuvalt teie klient jõuda. Ühenduse hübriid sõltub tehes DNS-i klient. Enamikul juhtudel __Kliendi__ on rakenduse koodis. Kui kliendi täida DNS-i otsing, (seda proovida IP-aadressi lahendamiseks, nagu oleks domeeni nimi (x.x.x.x)), siis liikluse ei saadeta hübriid-ühenduse kaudu.
>
> Näiteks (pseudocode) määratlete **10.4.5.6** oma kohapealse hosti:
> 
> **Töötab järgmistel juhtudel:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Ei tööta järgmistel juhtudel:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Saate luua ühenduse hübriid

Azure'i portaalis veebirakenduste abil **või** BizTalki teenuste kaudu saab luua ühenduse hübriid. 

**Luua hübriid ühendused veebirakenduste abil**, vt [Ühenduse loomine Azure veebirakenduste asutusesiseste ressursside](../app-service-web/web-sites-hybrid-connection-get-started.md). Saate installida ka hübriid ühenduse Manager (HCM) web appi, mis on eelistatud meetod. 

**Hübriidjuurutuse ühendused BizTalki teenuste loomiseks**:

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Klõpsake vasakpoolsel navigeerimispaanil, valige **BizTalki teenused** ja valige BizTalki teenust. 

    Kui teil pole olemasoleva BizTalki teenuse, saate [BizTalki teenuse loomine](biztalk-provision-services.md).
3. Valige vahekaardil **Ühendused hübriid** .  
![Hübriidjuurutuse vahekaarti ühendused][HybridConnectionTab]

4. Valige **Loo ühendus hübriid** või tegumiribal nuppu **Lisa** . Sisestage järgmine:

    Atribuut | Kirjeldus
--- | ---
Nimi | Hübriidjuurutuse ühenduse nimi peab olema kordumatu ja ei saa BizTalki teenuse sama nimi. Saate sisestada suvalise nime, kuid olge täpne otstarbel. Näited:<br/><br/>Palgad*SQLServer*<br/>SupplyList*SharepointServer*<br/>Klientide*OracleServer*
Hosti nimi | Täielikult kvalifitseeritud hosti nimi, Sisestage hosti nimi või kohapealse ressursi IPv4 aadress. Näited:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domeeni*. Corp*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *yourCompany*.com<br/>10.100.10.10<br/><br/>Kui kasutate IPv4 aadress, Pange tähele, et teie kliendi või rakenduse koodi võib lahendada IP-aadress. Vt tähtis, Pange tähele selle teema ülaosas.
Port | Sisestage pordi number kohapealse ressursi. Näiteks kui kasutate veebirakenduste, sisestage pordi 80 või pordi 443. Kui kasutate SQL Server, sisestage pordi 1433.

5. Märkige ruut Märgi häälestamise lõpuleviimiseks. 

#### <a name="additional"></a>Täiendavad

- Mitme hübriid ühenduse loomist. Vaadake selle [BizTalki teenuste: väljaanded diagrammi](biztalk-editions-feature-chart.md) jaoks lubatud ühenduste arv. 
- Iga hübriid ühendus on loodud ühendus stringide paari: rakenduse nooleklahvid selle saatmise ja kohapealsete tutvustatakse, mida kuulata. Iga paari on esmane ja teisene klahvi. 


## <a name="LinkWebSite"></a>Azure'i rakenduse teenuse Web Appi või Mobile'i rakendus

Web Appi või mobiilirakenduse teenuses Azure rakenduse linkimiseks mõne olemasoleva ühenduse hübriid, valige **olemasoleva ühenduse hübriid kasutamine** hü ühendused tera. Lugege teemat [Accessi kohapealse hübriid ühendused teenuses Azure rakenduse abil](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Hübriid haldur asutusesisese installimine

Hübriidjuurutuse ühendus on loodud, installida kohapealse ressursi ühenduse hübriid ülemus. Saate alla laadida, oma Azure veebirakenduste või BizTalki teenust. BizTalki teenuste järgmiselt: 

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Klõpsake vasakpoolsel navigeerimispaanil, valige **BizTalki teenused** ja valige BizTalki teenust. 
3. Valige vahekaardil **Ühendused hübriid** .  
![Hübriidjuurutuse vahekaarti ühendused][HybridConnectionTab]
4. Valige tööülesande **Kohapealse häälestus**.  
![Kohapealse häälestamine][HCOnPremSetup]
5. Valige **installimine ja konfigureerimine** käivitamine või allalaadimiseks kohapealse süsteemi ühenduse hübriid ülemus. 
6. Valige installimise alustamiseks märke. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Täiendavad
- Hübriidjuurutuse haldur saab installida järgmised operatsioonisüsteemid:

    - Windows Server 2008 R2 (.NET Framework 4.5 + ja Windows Management Framework 4.0 + nõutav)
    - Windows Server 2012 (Windows Management Framework 4.0 + nõutav)
    - Windows Server 2012 R2


- Pärast installimist ühenduse hübriid ülemus, ilmneb järgmine: 

    - Azure'i majutatud ühenduse hübriid on konfigureeritud automaatselt esmane rakendus ühendusstringi. 
    - Asutusesiseste ressurss automaatselt konfigureeritud kasutama esmane asutusesisese ühendusstring.

- Ühenduse hübriid ülemus peate kasutama kehtiv kohapealse ühendusstringi autoriseerimine. Azure'i veebirakenduste või mobiilirakenduste peate kasutama lubatud rakendus ühendusstringi autoriseerimine.
- Muudate hübriid ühendused, installides muus eksemplaris hübriid ühenduse halduri teise serverisse. Konfigureerida kohapealse kuulajale nimega esimene kohapealse kuulajale sama aadressi kasutamine. Sellisel juhul on liiklus aktiivne kohapealse kuulajatele CEIP jaotatud (round jaan). 


## <a name="ManageHybridConnection"></a>Hübriidjuurutuse ühenduste haldamine
Teie hübriid ühenduste haldamiseks saate teha järgmist.

- Kasutada Azure portaali ja minge BizTalki teenust. 
- Kasutage [REST API-d](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopeerige/taastada hübriid ühendusstringi

1. [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Klõpsake vasakpoolsel navigeerimispaanil, valige **BizTalki teenused** ja valige BizTalki teenust. 
3. Valige vahekaardil **Ühendused hübriid** .  
![Hübriidjuurutuse vahekaarti ühendused][HybridConnectionTab]
4. Valige ühenduse hübriid. Valige tööülesande **Haldamine ühendus**.  
![Suvandite haldamine][HCManageConnection]

    **Ühenduse haldamine** on loetletud rakenduse ja kohapealsete ühendusstringi. Saate kopeerida ühenduse stringe või ühendusstringi kasutatav kiirklahv taastada. 

    **Kui valite taastada**, kasutada ühendusstringi kiirklahv ühiskasutuses on muutunud. Tehke järgmist.
    - Azure'i klassikaline portaalis valige Azure rakenduse **Sünkroonimine võtmed** .
    - Käivitage uuesti **Kohapealse häälestus**. Asutusesiseste häälestamise uuesti käivitamisel kohapealse ressurss automaatselt konfigureeritud kasutama värskendatud esmane ühendusstring.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Rühmapoliitika abil kohapealse ressursse, mis kasutavad ühenduse hübriid reguleerimine

1. [Hübriid haldur haldus malle](http://www.microsoft.com/download/details.aspx?id=42963)alla laadida.
2. Väljavõte failid.
3. Minge arvutis, mis muudab rühmapoliitika, tehke järgmist.  

    - Kopeerige soovitud. ADMX-failide *%WINROOT%\PolicyDefinitions* kausta.
    - Kopeerige soovitud. KESKMISE failide *%WINROOT%\PolicyDefinitions\en-us* kausta.

Kui kopeerinud, saate muuta poliitika rühmapoliitika redaktori.




## <a name="next"></a>Järgmise

[Azure'i veebirakenduste ühenduse on kohapealse ressurss](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Ühenduse loomiseks kohapealse SQL Server Azure'i Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hübriidjuurutuse andmeühenduste ülevaade](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Vt ka

[Haldamise BizTalki teenust Microsoft Azure'i REST API-ga](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalki Services: Diagrammi väljaanded](biztalk-editions-feature-chart.md)  
[Azure'i klassikaline portaalis BizTalki teenuse loomine](biztalk-provision-services.md)  
[BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
