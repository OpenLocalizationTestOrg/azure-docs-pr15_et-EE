<properties
    pageTitle="Hübriidjuurutuse andmeühenduste ülevaade | Microsoft Azure'i"
    description="Lisateavet hübriid ühendused, Turve, TCP-pordid ja toetatud konfiguratsioone. MABS, WABS."
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
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hübriidjuurutuse andmeühenduste ülevaade
Sissejuhatus hübriid ühendused, toetatud konfiguratsioone loendid ja nõutav TCP-pordid loendid.


## <a name="what-is-a-hybrid-connection"></a>Mis on ühenduse hübriid

Hübriidjuurutuse ühendused on funktsioon Azure'i BizTalki teenuseid. Hübriidjuurutuse ühendused pakuvad lihtne ja mugav võimalus ühendada funktsiooni veebirakenduste Azure'i rakendust Service (varem veebisaitide) ja Azure'i rakendust Service (varem Mobile teenused) funktsiooni mobiilirakenduste kohta kohapealse ressursid tulemüüriga taha.

![Hübriidjuurutuse ühendused][HCImage]

Hübriidjuurutuse ühendused eelised on järgmised.

- Veebirakenduste ja mobiilirakenduste juurdepääsu turvaliselt olemasoleva kohapealsed andmed ja teenused.
- Mitme veebirakenduste või mobiilirakenduste jagada ühenduse hübriid juurdepääsuks on kohapealse ressurss.
- Minimaalne TCP-pordid on vaja juurdepääsu oma võrgule.
- Rakenduste kasutamine hü ühendused juurde ainult teatud asutusesisese ressurss, mis on avaldatud hübriid-ühenduse kaudu.
- Saate luua ühenduse mis tahes kohapealse ressurss, mis kasutab staatiline TCP pordi, nt SQL Server, MySQL-i, HTTP Web API-d ja enamik kohandatud veebiteenused.

    > [AZURE.NOTE] TCP-põhiste teenuste (nt FTP Passiivne režiim või laiendatud passiivset) dünaamiline Porte pole praegu toetatud. LDAP ei toetata. LDAP kasutab staatilised TCP-pordid, kuid see võib kasutada ka UDP. Seetõttu ei toetata.

- Saate kasutada kõigi raamistiku veebirakenduste (.NET, PHP, Java, Python, Node.js) ja mobiilirakenduste (Node.js, .NET) ei toeta.
- Veebirakenduste ja mobiilirakenduste juurdepääsu kohapealse ressursid täpselt samamoodi nagu siis, kui veebis või Mobile'i rakendus asub teie kohalikus võrgus. Näiteks saate sama ühenduse kasutatav string asutusesisese kasutada Azure.


Hübriidjuurutuse ühendused pakuvad ka ettevõtte administraatorid juhtelement ja nähtavus ettevõtte ressursside tutvuda hübriid rakendusi, sh:

- Kaudu rühmapoliitika sätted, administraatorid saate luba võrgus hübriid ühendused ja määrata ka ressursse, mis pääseb hübriid rakendused.
- Sündmuste ja auditilogi logid ettevõtte võrgus pakuvad nähtavus ressursside korrad hübriid ühendused.


## <a name="example-scenarios"></a>Näidisstsenaariumid

Hübriidjuurutuse ühendused toetavad järgmised framework ja rakenduse kombinatsioonid:

- .NET Frameworki juurdepääsu SQL Server
- .NET Frameworki juurdepääsu HTTP-või HTTPS teenustega WebClient
- SQL serveri MySQL-i PHP juurdepääs
- Java juurdepääsu SQL Server, MySQL-i ja Oracle'i
- Java juurdepääsu HTTP-või HTTPS-teenused

Hübriidjuurutuse ühendused kasutamisel asutusesisese SQL serveri juurdepääsu arvestage järgmisega.

- SQL-i kiire nimega eksemplari peab olema konfigureeritud kasutama staatilised pordid. Vaikimisi SQL Expressi nimega eksemplari kasutamine dünaamiline pordid.
- SQL-i kiire vaikimisi eksemplari kasutada staatilised pordid, kuid TCP peab olema lubatud. Vaikimisi TCP pole lubatud.
- Rühmitamist või kättesaadavus rühmad, kasutades funktsiooni `MultiSubnetFailover=true` režiimis pole praegu toetatud.
- Funktsiooni `ApplicationIntent=ReadOnly` pole praegu toetatud.
- SQL-i autentimise võib olla nõutav Azure rakendus ja asutusesisese SQL server ei toeta lõpuni – autoriseerimine meetod.


## <a name="security-and-ports"></a>Turvalisus ja pordid

Hübriidjuurutuse ühendused abil ühiskasutusse Accessi allkirja (SAS) Luba ühendused kohapealse hübriid ühenduse ülemus ja Azure rakenduste kaudu ühenduse hübriid secure. Rakendus ja kohapealse hübriid ühenduse Manager on loodud eraldi võtmed. Ühenduse klahve saate kerida ja sõltumatult tühistada.

Hübriidjuurutuse ühendused ette tõrgeteta ja turvaline jaotuse klahve rakendusi ja kohapealse hübriid ühenduse ülemus.

Vaadake [hübriidjuurutuse ühenduste loomine ja haldamine](integration-hybrid-connection-create-manage.md).

*Rakenduse autoriseerimine on eraldi ühenduse hübriid*. Saab kasutada mis tahes õige autoriseerimine meetod. Luba sõltub lõpuni – autoriseerimine meetodite toetatud Azure pilveteenuse ja kohapealse komponendid. Näiteks Azure rakenduse pääseb juurde ka asutusesisese SQL serveri. Selle stsenaariumi korral SQL-i luba võib autoriseerimine meetod, mis on toetatud lõpuni.

#### <a name="tcp-ports"></a>TCP-pordid
Ainult väljaminev TCP või HTTP ühenduvust teie privaatvõrgu vajavad hü ühendused. Teil pole vaja avamiseks mis tahes tulemüüri portide või muuta oma võrgu perimeetri konfiguratsiooni lubama mis tahes sissetuleva meili Ühenduvus teie võrku.

Hübriidjuurutuse ühendused kasutavad järgmiste TCP-portidega:

Port | Miks seda vaja
--- | ---
9350 - 9354 | Järgmised pordid kasutatakse andmete edastamiseks. Teenuse siini relay halduri sondid pordi 9350 määratlemiseks, kui TCP-ühendus on olemas. Kui see on saadaval, siis eeldatakse, et pordi 9352 on saadaval ka. Andmete liiklus läheb Port 9352. <br/><br/>Lubage Väljaminevad ühendused järgmised pordid.
5671 | Pordi 9352 kasutamisel andmeside port 5671 kasutatakse juhtimise kanal. <br/><br/>Lubage Väljaminevad ühendused selles pordi.
80, 443 | Mõned andmed taotlused Azure kasutatakse järgmised pordid. Samuti kui pordid 9352 ja 5671 ei tööta, *siis* pordid 80 ja 443 on taandepäringud pordid, andmeedastuse ja juhtelemendi kanali jaoks.<br/><br/>Lubage Väljaminevad ühendused järgmised pordid. <br/><br/>**Märkus** See on soovitatav kasutada neid taandepäringud pordid asemel TCP pordid. HTTP/WebSocket kasutatakse andmekanalite asemel kohalikke TCP protokolli. See võib põhjustada alumise jõudlust.



## <a name="next-steps"></a>Järgmised sammud

[Hübriidjuurutuse ühenduste loomine ja haldamine](integration-hybrid-connection-create-manage.md)<br/>
[Azure'i veebirakenduste ühenduse on kohapealse ressurss](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Ühenduse loomiseks kohapealse SQL Server Azure'i web app](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Vt ka

[REST API haldamise BizTalki teenust Microsoft Azure'i](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalki teenuste: väljaanded diagrammi](biztalk-editions-feature-chart.md)<br/>
[Azure'i portaalis BizTalki teenuse loomine](biztalk-provision-services.md)<br/>
[BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
