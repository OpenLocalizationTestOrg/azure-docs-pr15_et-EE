<properties 
    pageTitle="IIS-i autentimis- ja Azure Mitmikautentimise Server"
    description="See on Azure mitmekordne autentimine leht, mis aitab IIS-i autentimis- ja Azure mitmekordne autentimine serveri juurutamine."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="iis-authentication"></a>IIS-i autentimine

Azure'i mitmekordne autentimise Server IIS-i autentimise osas võimaldab lubamine ja konfigureerimine IIS integratsioon autentimise Microsoft IIS veebirakendustega. Azure'i mitmekordne autentimise Server installib lisandmoodul, mille saate filtreerida taotlusi tehtud IIS veebiserver Azure'i Mitmikautentimise lisamiseks. Lisandmooduli IIS toetab vormi autentimise ja integreeritud Windows HTTP autentimise jaoks. Usaldusväärsed IP-d saab konfigureerida ka välistatud sisemise IP-aadresside kaudu kahekordne autentimine.


![IIS-i autentimine](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Azure'i Mitmikautentimise serveriga vormipõhiseid IIS-i autentimist kasutades

IIS-i vormipõhiseid autentimist kasutava veebirakenduse turvamiseks installida Azure mitmekordne autentimise Server IIS veebiserver ja konfigureerida järgmiste toimingute kohta.

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni IIS-i autentimist.
2. Klõpsake vahekaarti vormipõhiseid.
3. Klõpsake nuppu Lisa... nupp.
4. Tuvastamiseks kasutajanimi, parool ja domeeni muutujate automaatselt, sisestage dialoogiboksis Auto-Configure Form-Based veebisaidi sisselogimise URL (nt https://localhost/contoso/auth/login.aspx) ja klõpsake nuppu OK.
5. Märkige ruut nõua Mitmikautentimise kasutaja match, kui kõik kasutajad on või server ja mitmikautentimise importida. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine mitmikautentimise, jätke ruut märkimata.
6. Kui lehe muutujate ei leita automaatselt, klõpsake selle määrata käsitsi... Veebisaidi Auto-Configure Form-Based dialoogiboksis nuppu.
7. Dialoogiboksis Add Form-Based veebisaidi sisselogimislehe väljale esitage URL sisestage URL ja sisestage rakenduse nimi (valikuline). Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid. Vt lisateavet spikrifaili esitada URL-i.
8. Valige taotluse õiges vormingus. See on seatud "Postituse või saada" Enamik veebirakenduste.
9. Sisestage kasutajanimi muutuja, parooli muutuja ja domeeni muutuja (kui see kuvatakse lehel Logi sisse). Kui peate liikuge sisselogimislehe veebibrauseris, paremklõpsake lehel ja valige "Kuva allikas" väljanimede sisend lahtrid lehel.
10. Märkige ruut nõua Azure'i Mitmikautentimise kasutaja match, kui kõik kasutajad on või imporditakse server ja mitmikautentimise. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine mitmikautentimise, jätke ruut märkimata. Vt selle funktsiooni kohta lisateabe saamiseks spikrifaili.
11.  Klõpsake selle Täpsem... nupp Täpsemad sätted, sh võimalus kohandatud eitamine lehe faili, valige vahemälu eduka isikutuvastusi veebisaidile jooksul kasutanud küpsiseid ja valige, kas autentimiseks Windowsi domeeni, LDAP-kataloogi või RADIUS serveri esmane mandaadi üle vaadata. Kui olete lõpetanud, klõpsake dialoogiboksi Add Form-Based veebisaidi naasmiseks nuppu OK. Vt lisateavet spikrifaili Täpsemad sätted.
12. Klõpsake nuppu OK.
13. Kui URL ja lehe muutujate tuvastatud või sisestatud veebisaidi andmed kuvatakse paanil vormipõhiseid.
14. Vaadake selle lubamine IIS-i lisandmoodulite Azure'i mitmekordne autentimine Server jaotise otse allpool IIS-i autentimise konfigureerimise lõpuleviimiseks.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Azure'i Mitmikautentimise serveriga integreeritud Windowsi autentimist kasutades

IIS-i integreeritud Windows HTTP autentimist kasutava veebirakenduse turvamiseks installida Azure mitmekordne autentimise Server IIS veebiserver ja konfigureerida järgmiste toimingute kohta.

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni IIS-i autentimist.
2. Klõpsake vahekaarti HTTP. Klõpsake vahekaarti vormipõhiseid.
3. Klõpsake nuppu Lisa... nupp.
4. Dialoogiboksis Lisa Base URL sisestage URL-i jaoks veebisaidile, kui HTTP autentimine on tehtud (nt http://localhost/owa) väljale URL-i ja sisestage rakenduse nimi (valikuline). Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid.
5. Reguleerida jõudeaja ajalõpu ja maksimum seansi korda kui vaikimisi ei piisa.
6. Märkige ruut nõua Mitmikautentimise kasutaja match, kui kõik kasutajad on või server ja mitmikautentimise importida. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine mitmikautentimise, jätke ruut märkimata. Vt selle funktsiooni kohta lisateabe saamiseks spikrifaili.
7. Kui soovitud, märkige ruut küpsise vahemälu.
8. Klõpsake nuppu OK.
9. Vt [Azure'i mitme teguriga autentimine serveri lubamine IIS-i lisandmoodulite](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) otse allpool IIS-i autentimise konfigureerimise lõpuleviimiseks.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Azure'i Mitmikautentimise Server IIS-i lisandmoodulite lubamine

Kui olete konfigureerinud vormi põhinev või HTTP autentimise URL-id ja sätted, valige asukohtade kui Azure mitmekordne autentimine IIS-i lisandmoodulite peaks laaditud ja lubatud IIS-i. Kasutage järgmist toimingut:

1. Kui töötab IIS 6, klõpsake vahekaarti ISAPI ja valige veebisait, kus töötab veebirakenduse jaotises (nt Vaikeveebisait) Azure'i mitme teguriga autentimine ISAPI filter lisandmooduli selle saidi jaoks lubada.
2. Kui IIS-i 7 või uuem versioon, klõpsake vahekaarti kohalikke mooduli ja valige server, (ID) või selle IIS-i lisandmooduli juures soovitud üleandmistasandi rakendustes.
3. Märkige ruut Luba IIS-i autentimist ekraani ülaosas. Azure'i Mitmikautentimise on nüüd turvaliseks valitud IIS-i rakendus. Veenduge, et kasutajad imporditud Server. Kui soovite valge sisemise IP-aadresse nii, et selle kahekordne autentimine ei ole vajalik, kui logite sisse veebisaidil nende asukohtadest, siis vaadake jaotist usaldusväärsed IP-d.


## <a name="trusted-ips"></a>Usaldusväärsete IP-d

Usaldusväärsed IP-d võimaldab kasutajatel mööduda Azure'i Mitmikautentimise veebisaidi päringuid, mis on pärit teatud IP-aadresside või alamvõrku. Näiteks võite välistatud kasutajatele: Azure'i Mitmikautentimise ajal Office logimine. Selleks määrate office alamvõrgu kirjena usaldusväärsed IP-d. Usaldusväärsed IP-d konfigureerimiseks kasutage järgmist:

1. IIS-i autentimise jaotise vahekaarti Usaldusväärsed IP-d.
2. Klõpsake nuppu Lisa... nupp.
3. Kui kuvatakse dialoogiboks lisamine usaldusväärsete IP-d, valige ühe IP, IP-vahemiku või alamvõrgu raadionupp.
4. vahemiku IP-aadresside või alamvõrku, mis peaks olema whitelisted hooldushinna IP-aadress. Kui soovitud alamvõrku, valige sobiv Võrgumask ja klõpsake nuppu OK. Funktsiooni nimekiri on nüüd lisatud.
