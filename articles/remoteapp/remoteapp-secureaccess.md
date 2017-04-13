
<properties 
    pageTitle="Juurdepääs Azure RemoteApp ja lisaks turvaliseks | Microsoft Azure'i"
    description="Siit saate teada, kuidas turvaline juurdepääs Azure RemoteApp juurdepääsu Azure Active Directory 's"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Juurdepääs Azure RemoteApp ja lisaks turvamine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Selles artiklis anname ülevaade sellest, kuidas administraator saab häälestada turvaline juurdepääs kanali lõppkasutaja kaudu Azure RemoteApp algab ja lõpeb nagu SQL-andmebaasi või mõne muu rakenduse tagaandmebaas turvaline ressursiga. Eesmärk on veenduge, et ainult autoriseeritud kasutajad soovitud tingimustele juurde serveri rakendused ja et turvaline tagaandmebaas pääseb juurde ainult kontrollitud Azure RemoteApp keskkonnas, mitte muudesse asukohtadesse.

On 3 peamist admin peab vaadata.

![Azure RemoteApp juurdepääsu kaalutlused](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Lugeda teavet ja nende vastused järgmistele küsimustele.

## <a name="who-can-access-the-collection"></a>Kes pääseb kogumine?
Administraator valib kasutajatele, kes pääsevad serveri rakendused kogumist. Saate kasutada Azure Active Directory (Azure AD) töö- või koolikontod (varem nimetati, "ettevõtte kontod") või Microsofti kontoga (nt @outlook.com). Enamik enterprise stsenaariumid kasutada Azure AD kontod; need võimaldavad teil Accessiga tingimusvormingu funktsioonid arutada hiljem ja on ka ainult domeeni ühendatud saidikogumid valik. Ülejäänud artikkel eeldab, et kasutate Azure AD kontod Azure RemoteApp.

**Mida me täita:**

Azure AD kontode kaudu juurdepääsu Azure RemoteApp annab meile kaks asja:

1.  Me aru saada, kes pääsevad rakendused Meil on avaldatud ja kasutada mis tahes tagasi-lõpetatakse nende rakenduste ühendamine.
2.  Me kontrollida aluseks Azure AD, et saame luua ja kustutada kasutajakonto, Määra parool poliitikad, kasutada mitmikautentimise jne. 

## <a name="how-is-the-collection-accessed-from-where"></a>Selle saidikogumi taotlemine Kust?
Tavaliselt soovite administraatorid määratlemiseks juurdepääsuks avaliku Interneti-ühendusega keskkonnas, nt Azure RemoteApp. Näiteks soovivad tagada, et kasutajad, kes kasutavad keskkonda väljaspool ettevõtte võrku mitmikautentimise (MFA) abil pääsete; või võib-olla nad peaksid blokeeritud täielikult eemaldada.

Azure RemoteApp administraatorid saavad kasutada funktsiooni, mis on saadaval Azure AD Premium kaudu määrata juurdepääsu poliitikad oma Azure RemoteApp keskkonnas. Ja aruannete teavitamine funktsioone saab kasutada ka jälgida, kuidas keskkonna pääseb.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Kuidas häälestada juurdepääsu Azure RemoteApp jaoks
Me tegemiseks näiteks sel juhul – Azure RemoteApp, administraator soovib blokeerida keskkonna juurdepääsu, kui kasutaja on väljaspool ettevõtte võrku.

>[AZURE.NOTE] Oleme endale versiooniks Azure AD Premium taseme ja, et olete loonud vähemalt üks Azure RemoteApp saidikogumi.

1.  Azure'i portaalis vahekaarti **Active Directory** . Klõpsake kausta, mida soovite konfigureerida.

    Pidage meeles: Juurdepääsu on atribuut kataloogi, mitte Azure RemoteApp, nii, et kõik konfigureerimine on lõpetatud directory tasemel. Selleks peate olema directory administraator neid muudatusi teha.

2.  Klõpsake nuppu **rakendused**ja klõpsake **Microsoft Azure RemoteApp** juurdepääsu häälestamiseks. Pange tähele, et saate häälestada juurdepääsu iga "tarkvara, kui teenus rakenduse kataloogis eraldi.
![Azure RemoteApp juurdepääsu seadistamine](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Klõpsake menüü **konfigureerimine** Määrake **Lubamine juurdepääsureeglid** olekuks sees.
![Azure RemoteApp juurdepääsureeglid lubamine](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Nüüd saate konfigureerida reeglid erinevad ja kes neid rakendamiseks valige:

    1. Valige **Blokeeri juurdepääs pole tööl** täielikult takistada kasutajate juurdepääsu Azure RemoteApp väljaspool võrgu keskkond, mis on määratud.
    2. Klõpsake allpool võimalus määrata IP-aadresside vahemikud, mis moodustavad "usaldusväärsete võrgu". Kõik need väljaspool on tagasi lükatud.

5.  Testige oma konfiguratsioon käivitades Azure RemoteApp kliendi IP-aadress väljaspool vahemikku määratud. Pärast seda, kui logite oma Azure AD mandaadid näha peaks olema järgmine teade:

![Keelatakse juurdepääs Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Tulevaste juurdepääsu funktsioonid 
Azure Active Directory meeskonna tingimusvormingu Accessi uute võimaluste kohta. Administraatorid saab luua uut tüüpi reegleid väljaspool võrgu asukoha reeglid. Uued funktsioonid avaliku eelvaade peaks varsti saadaval.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Kuidas jälgida juurdepääsu Azure RemoteApp
Hea funktsioonide kasutamiseks koos juurdepääsu on Azure Active Directory Premium aruandlusfunktsioonid. Saate kasutada aruannete abil saate jälgida, kes pääseb teie keskkonnas ja avastada kahtlasest tegevusest.

Näiteks saate vaadata kasutajatele, kellel on juurdepääs Azure RemoteApp, mitu korda need kas see nimed ja millal.

1.  Portaalis Azure **Active Directory**nuppu ja klõpsake kataloogi.

2.  Avage vahekaart **aruanded** .

3.  Valige loendist aruanded, **rakenduse kasutamine** jaotises **integreeritud rakendused**.

    Kuvatakse mõned Liidetud statistika Azure RemoteApp. 
![Liidetud Azure RemoteApp Accessi statistika](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Klõpsake rakenduse kasutajad, kes kasutavad Azure RemoteApp kohta teabe.
![Kasutaja juurdepääs statistika Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Kokkuvõte
Azure Active Directory Premium saate häälestada juurdepääsureeglid Azure RemoteApp (ja muu tarkvara kättesaadavaks Azure AD teenuse rakendustena). Reeglite on praegu ainult võrgus asukoha poliitika, kuid tulevikus laiendada muude aspektide ettevõtte haldamine.

Azure'i AD Premium pakub aruandlus ja jälgimise võimalusi, et veelgi laiendada juhtelemendi admin on oma Azure RemoteApp keskkonna üle.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Kuidas kontrollida, kas mu turvaline Ressursi pääseb juurde ainult minu Azure RemoteApp keskkonnas?
Eelmiste jaotiste käesoleva artikli me olulisele juurdepääsu Azure RemoteApp keskkonnas. Me on saavutada, et kasutajatele, kellel on lubatud juurdepääs valides ja häälestamiseks juurdepääsureeglid täpsemaks juhtimiseks, kuidas nad saavad kasutada teenust.

Levinud stsenaariumi Azure RemoteApp juurutuste on, et rakendused tuleb suhelda tagaandmebaas ressurssi, näiteks SQL-andmebaasi. Selle ressursi majutatakse kas kohapeal (nt jaotises ettevõtte võrku) või pilveteenusesse (nt rakenduses Azure'i IaaS). Veenduge, et tagaandmebaas Ressursi pääseb juurde ainult rakenduste kaudu Azure RemoteApp juurutatud ja mitte näiteks rakenduse otse kasutaja arvutis töötab ja avaliku Interneti kaudu juurdepääsu soovitakse sageli administraatorid. Azure RemoteApp on tihti ühes keskses kohas hallata ja turvalise keskkonna ja seega ainult tee kaudu, mis peaks kasutajad suhtlevad tagaandmebaas ressursi.

Lahendus on paigutada Azure RemoteApp keskkonna ja turvalise ressursi rakenduses on sama Azure virtuaalse võrgu (VNET). Kui ressurss on mõnele teisele saidile, saate luua-saidilt VPN-ühendus, näiteks mõne VNet kestvad Azure andmekeskuse ja klientide kohapealse keskkonna loomiseks.

Azure RemoteApp toetab kahte tüüpi saidikogumi juurutuste, kuhu saate sisestada oma VNET.

-   Mitte domeeni-ühendatud: rakendused on "vaateväljas" ja muud ressursid on VNET. Näiteks saate seda kasutada rakendusi SQL-i autentimist kasutava SQL-andmebaasiga ühenduse (rakendused autentimiseks kasutaja otse andmebaasis)

-   Domeeni ühendatud: virtuaalmasinates, mis kasutavad Azure RemoteApp on ühendatud selle VNET domeenikontrolleri. See on kasulik, kui rakenduste vaja selleks, et saada juurdepääsu ressursile tagaandmebaas Windows domeenikontrolleri autentimiseks.
![Domeeni ühendatud saidikogumi Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Kuidas luua turvalist ühendust Azure ja oma kohapealse keskkonna vahel
On mitu konfiguratsiooni võimalust ühenduse oma Azure'i ja kohapealsete keskkondadega. Saadaval on hea ülevaade suvandist.

Azure'i RemoteAppi peate oma VNet esmalt konfigureerimine ja seejärel kasutage seda oma saidikogumi loomine käigus. 

## <a name="the-complete-solution"></a>Täielik lahendus
Alloleval joonisel kuvatakse täielik lahendus, kus meil on turvaline juurdepääs kanali lõppkasutaja kaudu Azure RemoteApp (ARA), sisse ehitatud taustväärtus ressursi.
![Turvaline Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) etapp 1 me valitud kasutajate ja loodud juurdepääsureeglid, mis haldavad, kuidas pääseb ARA. Järgmises näites oleme ainult Luba juurdepääs on ettevõtte võrgus töötavatele kasutajatele. Mitte-ühilduv kasutajad ei saa juurdepääs ARA keskkonnas üldse.
"Etapp 2" me avatud kirjutamata ressursi ainult kaudu VNet/VPN konfiguratsiooni, mida me kontrollida. Azure RemoteApp on paigutatud sama VNet. Seetõttu on ARA keskkonna ainult pääseb ressurss.


