<properties
    pageTitle="Azure'i AD-ühenduse seisund Agent installi | Microsoft Azure'i"
    description="See on seisund Azure'i AD-ühenduse leht, mis kirjeldab agenti installi AD FS-i ja sünkroonimine."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure'i AD-ühenduse seisund agenti installi

Selle dokumendi juhatab teid installimist ja konfigureerimist seisund agentide Azure'i AD-ühenduse kaudu. Saate alla laadida agentide [siin](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Nõuded
Järgmises tabelis on seisund Azure'i AD-ühenduse kasutamise nõuete loendit.

| Nõue | Kirjeldus|
| ----------- | ---------- |
|Azure'i AD Premium| Azure'i AD ühenduse seisund on Azure AD Premium funktsioon ja Azure AD Premium nõuab. </br></br>Lisateavet leiate teemast [Azure AD Premium töötamise alustamine](active-directory-get-started-premium.md) </br>30-päevane tasuta prooviversioon käivitamise kohta leiate [käivitamine prooviversiooni.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Teil peab olema oma Azure AD alustada Azure'i AD-ühenduse seisund globaalne administraator|Vaikimisi saate ainult globaalne administraator installima ja konfigureerima seisund agentide alustamine, portaali ja teostavad ülesandeid, mis asuvad Health Azure'i AD-ühenduse. Lisateabe saamiseks lugege teemat [Azure AD kataloogi haldamise](active-directory-administer.md). <br><br> Rollipõhine juurdepääsu juhtimine abil saate lubada juurdepääsu teistele kasutajatele seisund Azure'i AD-ühenduse ettevõtte. Lisateavet leiate teemast [Rollipõhine juurdepääsu juhtimine Azure'i AD-ühenduse seisundi.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Oluline:** Konto, mida kasutatakse agentide installimisel peab olema töökoha või kooli kontoga. Microsofti kontot ei saa see olla. Lisateabe saamiseks lugege teemat [Azure organisatsiooni kasutajaks registreerumine](sign-up-organization.md)
|Azure'i AD-ühenduse seisund Agent iga suunatud serverisse installitud| Azure'i AD-ühendus seisund nõuab agenti installi andmeid, mida on vaadata portaalis suunatud serverites. </br></br>Näiteks andmete toomiseks teie AD FS-i kohapealse taristu agent peab olema installitud AD FS-i, AD FS puhverserver ja veebiteenuse rakenduse puhverserver serverites. Samuti saada andmeid oma kohapealse AD DS taristu agent peab olema installitud domeenikontrolleritesse. </br></br>**Oluline:** Konto, mida kasutatakse agentide installimisel peab olema töökoha või kooli kontoga. Microsofti kontot ei saa see olla. Lisateabe saamiseks lugege teemat [Azure organisatsiooni kasutajaks registreerumine](sign-up-organization.md)|
|Väljamineva meili Ühenduvus Azure teenuse lõpp-punktid|Installimine ja käitusaja, agent nõuab ühenduvust Azure'i AD-ühenduse seisund teenuse lõpp-punktid. Väljamineva meili Ühenduvus blokeerimisel tagada, et lubatud lisatakse järgmised lõpp-punktid: </br></br><li>& #42;. blob.Core.Windows.net </li><li>& #42;. Queue.Core.Windows.net</li><li>adhsprodwus.ServiceBus.Windows.net - Port: 5671 </li><li>https://Management.Azure.com </li><li>https://s1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.DC.AD.MSFT.net/</li><li>https://login.Windows.net</li><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Tulemüüri portide agent serveris.| Agent jaoks on vaja järgmisi tulemüüri portide avatud selleks, et agent suhelda Azure AD seisund teenuse lõpp-punktid.</br></br><li>TCP/UDP pordi 443</li><li>TCP/UDP port 5671</li>
|Luba järgmisi veebisaite, kui IE täiustatud turvalisus on lubatud| Kui IE täiustatud turvalisus on lubatud, siis järgmisi veebisaite lubama serveris, mis läheb installitud agent.</br></br><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://login.Windows.net</li><li>Liiduserveri oma ettevõtte jaoks usaldusväärne Azure Active Directory. Näide: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Tervise Agent AD FS-i installimist Azure AD ühenduse loomine
Agendi installimise alustamiseks topeltklõpsake allalaaditud faili .exe. Esimesel ekraanil, klõpsake nuppu Installi.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health-requirements/install1.png)

Kui installimine on lõpule jõudnud, klõpsake nuppu Konfigureeri kohe.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health-requirements/install2.png)

Käsuviip käivitatakse, mõned PowerShelli, mis käivitab Register-AzureADConnectHealthADFSAgent, millele järgneb. Kui teil palutakse sisse logida Azure, minna ja logige sisse.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health-requirements/install3.png)


Pärast sisselogimist, PowerShelli kasutab ka edaspidi. Kui see on lõpule jõudnud, saate sulgeda PowerShelli ja konfiguratsiooni on lõpule viidud.

Selles etapis teenuste alustada automaatselt, mis võimaldab jälgida ja andmete kogumine agent. Kui teil on täidetud kõigi liigendatud eelmiste jaotiste eeltingimused, hoiatused kuvatakse PowerShelli aknas. Kindlasti enne installimist agent [nõuded](active-directory-aadconnect-health-agent-install.md#requirements) lõpuleviimiseks. Järgmine pilt on näide nende vigade.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health-requirements/install4.png)

Veenduge, et agent on installitud, otsige järgmisi teenuseid serveris. Kui konfiguratsiooni, nad peaksid juba töötama. Muul juhul nad on lõpetanud, kuni konfiguratsiooni on lõpule viidud.

- Azure'i AD-ühenduse teenuse seisund AD FS-i diagnostika
- Azure'i AD-ühenduse teenuse seisund AD FS-i ülevaateid
- Azure'i AD-ühenduse jälgimise teenuse seisund AD FS-i

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Windows Server 2008 R2 serverid agendi installimine

Juhised Windows Server 2008 R2 serverid:

1. Veenduge, et server töötab Service Pack 1 või uuem versioon.
1. Lülitage välja IE paoklahvi (ESC) agent installi.
1. Enne installimist AD seisund agendi serverid, installida Windows PowerShelli 4.0. Windows PowerShelli 4.0 installimiseks tehke järgmist.
 - Installige [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) allalaadimine ühenduseta Installeri järgmise lingi kaudu.
 - Installige PowerShell ISE (alates Windows funktsioonide)
 - Installimine on [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Installige Internet Exploreri versiooni 10 või uuem serveris. (Teenuse seisund vajalik autentida Azure'i administraator mandaadi abil.)
1. Windows PowerShelli 4.0 Windows Server 2008 R2 installimise kohta leiate lisateavet artiklist viki [siin](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Luba auditi AD FS-i

> [AZURE.NOTE] See jaotis kehtib ainult AD FS-i liiduserverite.

Funktsiooni Kasutusanalüüsi andmete kogumine ja selleks, peab Azure'i AD-ühenduse seisund agent auditilogide AD FS-i teave. Need logid on vaikimisi lubatud. Luba auditi AD FS-i ja AD FS-i auditilogide, otsige oma AD FS-i serverid, tehke järgmist.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Auditi AD FS 2.0 lubamiseks

1. Klõpsake nuppu **Start**, valige käsk **programmid**, käsk **Haldusriistad**, ja klõpsake **Kohaliku turbepoliitika**.
2. Liikuge kausta, **Turve sätted\Kohalikud Policies\User teabeõiguste haldus** ja seejärel topeltklõpsake Genereeri turvalisus auditite.
3. Menüü **Kohaliku turbesätte** veenduge, et AD FS 2.0 teenusekonto on loetletud. Kui seda pole olemas, klõpsake nuppu **Lisa kasutaja või rühma** lisamine loendisse ja seejärel klõpsake nuppu **OK**.
4. Auditi lubamiseks avage administraatoriõigustega Käsuviip ja käivitage järgmine käsk:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Sulgege kohaliku turbepoliitika ja seejärel avage halduse lisandmoodul. Halduse lisandmoodul avamiseks klõpsake nuppu **Start**, valige käsk **programmid**, käsk **Haldusriistad**ja klõpsake AD FS 2.0 haldus.
6. Klõpsake paanil toimingud Federation atribuutide redigeerimine.
7. **Federation atribuutide** dialoogiboksis vahekaarti **sündmused** .
8. Märkige ruudud **edu auditid** ja **auditeerimine tõrge** .
9. Klõpsake nuppu **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Auditi Windows Server 2012 R2 AD FS-i lubamiseks

1. Avage **Kohaliku turbepoliitika** avades **Server Manager** avakuval või Server Manager töölaua tegumiribal ja seejärel käsku **Tööriistad või kohalikud turbepoliitika**.
2. Liikuge kausta, **Turve sätted\Kohalikud Policies\User õiguste määramine** , ja seejärel topeltklõpsake **auditid Genereeri turvalisus**.
3. Menüü **Kohaliku turbesätte** veenduge, et AD FS-i teenuse konto on loetletud. Kui seda pole olemas, klõpsake nuppu **Lisa kasutaja või rühma** lisamine loendisse ja seejärel klõpsake nuppu **OK**.
4. Auditi lubamiseks avage administraatoriõigustega Käsuviip ja käivitage järgmine käsk:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Sulgege **Kohaliku turbepoliitika**ja seejärel avage **AD FS -i halduse** lisandmoodul (Server Manager, klõpsake nuppu Tööriistad ja seejärel valige AD FS-i haldus).
6. Klõpsake paanil toimingud **Federation atribuutide redigeerimine**.
7. Federation atribuutide dialoogiboksis vahekaarti **sündmused** .
8. Märkige ruudud **edu auditid ja tõrge auditid** ja seejärel klõpsake nuppu **OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Otsige üles AD FS-i auditilogi logid


1. Avage **Sündmusevaatur**.
2. Avage Windowsi logid ja valige **Turvalisus**.
3. Klõpsake parempoolsel paanil **Filter praeguse logid**.
4. Valige jaotises Sündmuse allikas, **AD FS -i audit**.

![Auditilogide AD FS-i](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Kui rühmapoliitika, mis on keelamine AD FS-i auditeerimine on olemas, ei saa teabe kogumiseks Azure'i AD-ühenduse seisund Agent. Veenduge, et teil pole rühmapoliitika, mis võivad keelamine auditeerimine.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Azure'i AD-ühenduse seisund agent sünkroonimine installimine
Azure'i AD-ühenduse seisund agent sünkroonimine installitakse automaatselt uusima koostamine, Azure'i AD-ühenduse. Azure'i AD-ühenduse kasutamiseks sünkroonimine, peate alla laadida Office'i uusima versiooni Azure'i AD-ühenduse ja installige see. Uusima versiooni saate alla laadida [siin](http://www.microsoft.com/download/details.aspx?id=47594).

Veenduge, et agent on installitud, otsige järgmisi teenuseid serveris. Kui konfiguratsiooni, nad peaksid juba töötama. Muul juhul nad on lõpetanud, kuni konfiguratsiooni on lõpule viidud.

- Azure'i AD-ühenduse seisund sünkroonimine ülevaateid teenus
- Azure'i AD-ühenduse jälgimise teenuse seisund sünkroonimine

![Azure'i AD-ühenduse sünkroonimise seisundi kontrollimine](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Pidage meeles, et seisund Azure'i AD-ühenduse kasutamine eeldab, Azure AD Premium. Kui teil pole Azure AD Premium, te ei saa Azure'i portaalis konfigureerimise lõpuleviimiseks. Lisateabe saamiseks vaadake [nõuded leht](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Käsitsi Azure'i AD-ühenduse seisund sünkroonimine registreerimine
Azure'i AD-ühenduse seisundi sünkroonimine agent registreerimise nurjumisel installimise Azure'i AD-ühenduse saate PowerShelli käsk agent käsitsi registreerimiseks.

>[AZURE.IMPORTANT] Ainult selle PowerShelli käsu on nõutav, kui agent registreerimine nurjub pärast installimist Azure'i AD-ühenduse.

PowerShelli käsk nõutakse ainult siis, kui seisund agendi registreerimine nurjub, isegi pärast edukat installimist ja Azure'i AD-ühenduse konfiguratsiooni. Teenuste seisundi Azure'i AD-ühenduse hakkavad pärast agent on edukalt registreeritud.

Saate käsitsi registreerida järgmist PowerShelli käsu sünkroonimine agent Azure'i AD-ühenduse seisund:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Käsu võtab jälgima parameetrid:

- AttributeFiltering: $true (vaikeväärtus) – kui Azure'i AD-ühenduse ei saa sünkroonida vaikimisi atribuudi seadmine ja on kohandatud kasutamiseks filtreeritud atribuudi. $false muul viisil.
- StagingMode: $false (vaikeväärtus) – kui Azure'i AD-ühenduse server pole lavastus režiimi, kui server on konfigureeritud olema lavastus režiimi $true.

Kui küsitakse autentimiseks kasutage sama üldadministraatori kontot (näiteks admin@domain.onmicrosoft.com) mida kasutati konfigureerida Azure'i AD-ühenduse.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Installimise Azure AD ühenduse seisund Agent AD DS
Agendi installimise alustamiseks topeltklõpsake allalaaditud faili .exe. Esimesel ekraanil, klõpsake nuppu Installi.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Kui installimine on lõpule jõudnud, klõpsake nuppu Konfigureeri kohe.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Käsuviip käivitatakse, mõned PowerShelli, mis käivitab Register-AzureADConnectHealthADDSAgent, millele järgneb. Kui teil palutakse sisse logida Azure, minna ja logige sisse.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Pärast sisselogimist, PowerShelli kasutab ka edaspidi. Kui see on lõpule jõudnud, saate sulgeda PowerShelli ja konfiguratsiooni on lõpule viidud.

Selles etapis teenuste alustada automaatselt, mis võimaldab jälgida ja andmete kogumine agent. Kui teil on täidetud kõigi liigendatud eelmiste jaotiste eeltingimused, hoiatused kuvatakse PowerShelli aknas. Kindlasti enne installimist agent [nõuded](active-directory-aadconnect-health-agent-install.md#requirements) lõpuleviimiseks. Järgmine pilt on näide nende vigade.

![Azure'i AD-ühenduse AD DS seisundi kontrollimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Veenduge, et agent on installitud, otsige domeenikontrolleri järgmisi teenuseid.

- Azure'i AD-ühenduse teenuse seisund AD DS ülevaateid
- Azure'i AD-ühenduse jälgimise teenuse seisund AD DS

Kui konfiguratsiooni, nende teenuste peaks juba töötab. Muul juhul nad on lõpetanud, kuni konfiguratsiooni on lõpule viidud.

![Azure'i AD-ühenduse seisundi kontrollimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Installimise Azure AD ühenduse seisund Agent AD DS veebisaidil.
Pärast installimist .exe faili, saavad teostada registreerimise käigus järgmist PowerShelli käsu abil:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure'i AD-ühenduse seisund agentide kasutada HTTP-puhverserver konfigureerimine
Saate konfigureerida Azure'i AD-ühenduse seisund agentide töötamiseks HTTP-puhverserver.

>[AZURE.NOTE]
- Abil "Lähtestada seatud ProxyServerAddress" pole toetatud, nagu agent kasutab System.Net web nõuab Microsoft Windows HTTP Services asemel teha.
- Konfigureeritud Http-puhverserveri aadressi kasutatakse läbiv Https krüptitakse.
- Autenditud puhverserverite (kasutades HTTPBasic) pole toetatud.

### <a name="change-health-agent-proxy-configuration"></a>Muuda seisund Agent puhverserveri konfigureerimine
Teil konfigureerida kasutama HTTP-puhverserver Azure'i AD-ühenduse seisund Agent järgmistest suvanditest.

>[AZURE.NOTE]Azure'i AD-ühenduse seisund Agent kõigi teenuste tuleb taaskäivitada, selleks, et puhverserveri sätteid värskendada. Käivitage järgmine käsk:<br>
Uuesti-teenus AdHealth *

#### <a name="import-existing-proxy-settings"></a>Olemasoleva puhverserveri sätete importimine

##### <a name="import-from-internet-explorer"></a>Internet Exploreri importimine
Internet Explorer HTTP-puhverserveri sätteid saab importida, kasutatakse seisund Azure'i AD-ühenduse kaudu. Iga serverites, kus töötab seisund agent, käivitada PowerShelli järgmine käsk:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>WinHTTP importimine
WinHTTP puhverserveri sätete saab importida kasutatava seisund Azure'i AD-ühenduse kaudu. Iga serverites, kus töötab seisund agent, käivitada PowerShelli järgmine käsk:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Määrake puhverserveriaadressid käsitsi
Saate käsitsi määrata puhverserveri iga serverites, kus töötab seisund Agent, kasutades järgmist PowerShelli käsku:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Näide: *Set-AzureAdConnectHealthProxySettings - HttpsProxyAddress myproxyserver: 443*

- "aadress" saab DNS-i lahutatavat serveri nime või aadressi IPv4
- "port" saate välja jäetud. Kui välja jäetud, siis 443 vaikeport on valitud.

#### <a name="clear-existing-proxy-configuration"></a>Eemalda olemasoleva puhverserveri konfigureerimine
Olemasoleva puhverserveri konfiguratsiooni saate tühjendada, käivitage järgmine käsk:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Lugege praeguse puhverserveri sätted
Praegu konfigureeritud puhverserveri sätted saate lugeda, käivitage järgmine käsk:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Test Ühenduvus Azure AD ühenduse teenuse seisund
On võimalik, et võib tekkida agendi Azure'i AD-ühenduse seisund teenuse seisund Azure'i AD-ühenduse katkemisel põhjustavad. Nendeks võrguprobleemide, luba küsimusi või erinevad muudel põhjustel.

Kui agent ei saa andmeid saata üle kahe tunni teenuse seisund Azure'i AD-ühenduse, on tähistatud portaalis järgmine teatis: "seisund teenuse andmed ei ole ajakohane." Saate kinnitada, kui probleemse Azure'i AD-ühenduse seisund agent saab andmete üleslaadimiseks teenuse seisund Azure'i AD-ühenduse järgmist PowerShelli käsu:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Roll parameeter on praegu järgmised väärtused:

- ADFS-I
- Sünkroonimine
- LISAB

Käsk - ShowResults lipu abil saate vaadata üksikasjalikku logid. Kasutage järgmises näites:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Ühenduvus tööriista kasutamiseks peate teostama esmalt agent registreerimise. Kui te ei saa registreerimise agent lõpetama, veenduge, et olete täitnud kõiki [nõuded](active-directory-aadconnect-health-agent-install.md#requirements) seisundi Azure'i AD-ühenduse. Vaikimisi sooritatakse test ühenduvuse agendi registreerimise käigus.



## <a name="related-links"></a>Seotud lingid

* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund toimingud](active-directory-aadconnect-health-operations.md)
* [Kasutades Azure AD AD FS-i seisundit kasutajaga](active-directory-aadconnect-health-adfs.md)
* [Azure'i AD-ühenduse seisundi kasutamise sünkroonimine](active-directory-aadconnect-health-sync.md)
* [Kasutades Azure AD kasutajaga AD DS seisund](active-directory-aadconnect-health-adds.md)
* [Azure'i AD-ühenduse seisund KKK](active-directory-aadconnect-health-faq.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
