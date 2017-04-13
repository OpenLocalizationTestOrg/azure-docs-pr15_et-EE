<properties 
    pageTitle="Mitmikautentimise RADIUS autentimis- ja Azure Server"
    description="See on Azure mitmekordne autentimine leht, mis aitab RADIUS autentimis- ja Azure mitmekordne autentimine serveri juurutamine."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Mitmikautentimise RADIUS autentimis- ja Azure Server

Jaotise RADIUS autentimise võimaldab lubamine ja konfigureerimine RADIUS autentimise Server Azure'i mitme teguri autentimist. On standardne protokoll aktsepteerige autentimise ja nende taotlused. Azure'i mitmekordne autentimise Server toimib RADIUS serveri ja lisatakse teie RADIUS kliendi (nt VPN-seade) ja autentimine target, mis võiks olla Active Directory (AD), LDAP-kataloogi või mõne muu RADIUS serveri, et lisada Azure Mitmikautentimise vahele. Jaoks Azure'i Mitmikautentimise funktsiooni, tuleb konfigureerida nii, et see suhelda nii kliendi serverite ja autentimine target Server Azure'i mitme teguri autentimist. Azure'i mitmekordne autentimise Server aktsepteerib taotlusi RADIUS klientrakenduses, kinnitatakse mandaadi autentimine sihi suhtes, lisab Azure'i Mitmikautentimise ja saadab vastuse RADIUS kliendile. Kogu autentimine õnnestub ainult juhul, kui on esmane autentimis- ja Azure Mitmikautentimise õnnestub.

>[AZURE.NOTE]
>MFA Server toetab ainult PAP (parooli autentimise protokoll) ja MSCHAPv2 autentimine (Microsofti ülesanne-käepigistus Authentication Protocol) RADIUS protokolle kui RADIUS serveri.  Muude protokollid, nt EAP (extensible autentimise protokoll) saab kasutada, kui MFA server toimib RADIUS puhverserveri teise RADIUS server, mis toetab protokolli nagu Microsoft NPS.
></br>
>Selle konfiguratsiooni teiste protokollide kasutamisel ei tööta ühesuunalise SMS ja VANDE sõned MFA Server ei ole võimalus algatada eduka RADIUS ülesanne vastuse protokolli abil.


![RADIUS autentimine](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>RADIUS autentimise konfigureerimine

RADIUS autentimise konfigureerimine installige Windows Server Server Azure'i mitme teguri autentimist. Kui teil on Active Directory keskkonnas, server on ühendatud domeeni sees võrgus. Järgmiste toimingute abil saate konfigureerida Azure mitmekordne autentimise Server:

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni RADIUS autentimist.
2. Märkige märkeruut Luba RADIUS autentimist.
3. Menüü kliendid muuta autentimise Port(s)) ja raamatupidamine Port(s)) kui Azure mitmekordne autentimine RADIUS teenust tuleks siduda mittestandardsed pordid kuulata RADIUS päringutele kliendid, mis konfigureeritakse.
4. Klõpsake nuppu Lisa... nupp.
5. Sisestage dialoogiboksi lisamine RADIUS klient Server Azure'i mitme teguriga autentimine, rakenduse nimi (valikuline) ja ühiskasutuses salajane kuvatakse autentida seadme/serveri IP-aadress. Ühiskasutusega salajane tuleb sama Azure'i mitmekordne autentimine Server ja seadme ja serveri. Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid.
6. Märkige ruut nõua Mitmikautentimise kasutaja match, kui kõik kasutajad on või imporditakse server ja mitmikautentimise. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine mitmikautentimise, jätke ruut märkimata. Vt selle funktsiooni kohta lisateabe saamiseks spikrifaili.
7. Märkige ruut Luba taandepäringud VANDE Turbeloa kui kasutaja hakkab kasutama Azure'i Mitmikautentimise mobiilirakenduse autentimise ja soovite kasutada VANDE Pääsukoodid taandepäringud autentimist riba-, telefoni SMS- või tõuketeatised kõne teate.
8. Klõpsake nuppu OK.
9. Teil võib korrake juhiseid 4 – 8 lisada täiendavad RADIUS kliendid.
10. Klõpsake soovitud vahekaarti.
11. Kui Server Azure'i mitmekordne autentimine on installitud keskkonnas Active Directory domeeni ühendatud serveris, valige Windowsi domeeni.
12. Kui kasutajad peaksid autenditud LDAP-kataloogi, valige LDAP siduda. LDAP siduda kasutamisel peab kataloogi integreerimise ikooni ja redigeerida nii, et Server siduda kataloogi LDAP konfiguratsiooni vahekaardil sätted. LDAP konfigureerimise juhised leiate LDAP puhverserveri konfigureerimine juhend.
13. Kui teine RADIUS serveri peaks autenditud kasutajad, valige RADIUS server.
14. Kas Server on puhverserveri RADIUS taotluste, klõpsates nuppu Lisa konfigureerida... nupp.
15. Sisestage dialoogiboksi lisamine RADIUS serveri RADIUS serveri ja ühiskasutuses salajane IP-aadress. Ühiskasutusega salajane peate olema sama nii Azure'i mitmekordne autentimine Server ja RADIUS serveri. Muuta autentimise pordi ja raamatupidamine portide, kui erinevate pordid kasutavad RADIUS serveri.
16. Klõpsake nuppu OK.
17. Azure'i mitmekordne autentimise Server peate lisama RADIUS kliendi RADIUS serveri nii, et töödelda Juurdepääsutaotluste saadetud serverist Azure'i mitme teguri autentimist. Kasutage sama ühiskasutusega salajane Azure'i mitmekordne autentimine server on konfigureeritud.
18. Teil võib Korrake seda toimingut lisada täiendavad RADIUS serverid ja konfigureerida järjestuses, milles Server tuleb kõne need nupud Nihuta üles ja Nihuta alla. See lõpetab Azure'i mitmekordne autentimine serveri konfiguratsioon. Server on nüüd listening konfigureeritud pordid RADIUS Juurdepääsutaotluste konfigureeritud klientide jaoks.   


## <a name="radius-client-configuration"></a>RADIUS kliendi konfiguratsiooni

Kliendi RADIUS konfigureerimiseks silmas juhiseid.

- Konfigureerige oma seadme/server Azure'i mitmekordne autentimine serveri IP-aadress, mis toimivad RADIUS serveri kaudu RADIUS autentimiseks.
- Kasutage sama ühiskasutusega salajane kohal konfigureeritud.
- Konfigureerida RADIUS ajalõpp 30 – 60 sekundi nii, et on valideerimiseks kasutaja mandaat, tehke selle mitmikautentimise, saata oma vastuse ja seejärel RADIUS juurdepääsutaotluse aega.
