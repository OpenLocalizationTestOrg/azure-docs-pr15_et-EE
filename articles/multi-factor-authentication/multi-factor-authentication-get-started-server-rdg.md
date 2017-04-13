<properties 
    pageTitle="Remote lüüsi ja Azure mitmekordne autentimine serveri RADIUS abil"
    description="See on Azure mitmekordne autentimine leht, mis aitab juurutamine Remote Desktop (RD) lüüsi ja Azure mitmekordne autentimine serveri abil RADIUS."
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

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Remote lüüsi ja Azure mitmekordne autentimine serveri RADIUS abil

Paljudel juhtudel kasutab Remote lüüsi kohalik NPS kasutajate autentimiseks. Selles dokumendis kirjeldatakse, kuidas marsruutimiseks RADIUS taotlus läbi Remote Desktop Gateway (kohalik NPS) kaudu mitme teguriga autentimine server.

Mitme teguri autentimise Server peaks olema installitud eraldi serveris, mis on seejärel puhverserveri RADIUS taotluse NPS kaugserverisse töölaua lüüsi. Pärast NPS kinnitatakse kasutajanimi ja parool, see mitmekordne autentimine Server, mis teeb teise teguri autentimist tagasi tulemuseks lüüsi vastuseks tagasi.





## <a name="configure-the-rd-gateway"></a>RD lüüsi konfigureerimine

Kaugtöölaua lüüs peab olema konfigureeritud RADIUS autentimise saatmiseks Azure'i mitmekordne autentimise serverit. Kui RD lüüsi on installitud, konfigureeritud ja töötab, minge RD lüüsi atribuudid. Minge menüüsse RD ots poe ja seda kasutada keskses serveris, kus töötab NPS asemel kohalik serveris, kus töötab NPS muuta. Lisada ühe või mitme Azure'i mitmekordne autentimine serveri RADIUS serverid ja Määrake ühiskasutusega salajane iga server.





## <a name="configure-nps"></a>NPS konfigureerimine

Kaugtöölaua lüüs kasutab NPS RADIUS kutse saatmiseks Azure'i Mitmikautentimise. RD lüüsi kaudu ajastus enne lõpetamist mitmikautentimise vältimiseks tuleb muuta ajalõpp. Järgmiste toimingute abil saate konfigureerida NPS.

1. NPS, laiendage RADIUS kliendid ja serveri menüü vasakus veerus ja valige Remote RADIUS serveri rühmad. Minema TS LÜÜSI serveri rühma atribuute. Redigeerige RADIUS server, mis on kuvatud ja minge menüüsse Koormusetasakaalustuseks. Muuta "ilma vastuse enne taotluse käsitletakse sekundite arv kukkunud" ja "Vahel taotlusi, kui server on tuvastatud saadaval sekundite arv" 30 – 60 sekundi. Klõpsake vahekaardi autentimine/konto ja veenduge, et RADIUS pordid määratud vastavad mitme teguri autentimise Server on kuulamise pordid.
2. NPS tuleb konfigureerida ka serverilt RADIUS isikutuvastusi tagasi Azure'i mitme teguri autentimist. Klõpsake vasakpoolses menüüs RADIUS kliendid. Lisage Azure mitmekordne autentimise Server RADIUS kliendi. Valige sõbralik nimi ja Määrake ühiskasutusega salajane.
3. Poliitika laiendage vasakpoolsel navigeerimisribal ja klõpsake ühenduse taotlemine poliitika. See peaks sisaldama ühenduse taotlemine poliitika nimega TS LÜÜSI autoriseerimine poliitika, mis on loodud RD lüüsi konfigureerimisel. Selle poliitika ettepoole RADIUS taotlusi mitme teguri autentimise Server.
4. Kopeerige see poliitika uue konto loomiseks. Uue poliitika, lisage tingimus, mis vastab kliendi sõbralik nimi sõbralik nimi seadmine etappi 2 kohal Azure'i mitme teguriga autentimine serveri RADIUS kliendi jaoks. Autentimisteenuse pakkuja vahetada kohaliku arvuti. See poliitika tagab, et RADIUS taotluse saamisel serverist Azure'i mitmekordne autentimine autentimine ilmneb asemel kohalik saatmisega RADIUS tagasi Azure'i mitmekordne autentimine Server, mis tekitaks tsükkel tingimus. Selleks, et tsükkel tingimus, tellida selle uue poliitika kohal algse poliitika, mis edastab selle serveri mitmekordne autentimine.

## <a name="configure-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise konfigureerimine


--------------------------------------------------------------------------------



Azure'i mitmekordne autentimine Server on konfigureeritud RADIUS proxy RD lüüsi ja NPS vahel.  See peaks olema installitud domeeni ühendatud serveris, mis on eraldi RD lüüsi serverist. Järgmiste toimingute abil Server Azure'i mitmekordne autentimise konfigureerimine.

1. Avage Server Azure'i mitme teguri autentimist ja klõpsake ikooni RADIUS autentimist. Märkige märkeruut Luba RADIUS autentimist.
2. Menüü kliendid tagada pordid vastavad, mis on konfigureeritud NPS ja klõpsake nuppu Lisa... nupp. Lisage RD lüüsi serveri IP-aadress, rakenduse nimi (valikuline) ja ühiskasutuses salajane. Ühiskasutusega salajane tuleb sama Azure'i mitmekordne autentimine Server ja RD lüüsi.
3. Klõpsake soovitud vahekaarti ja valige raadionupp RADIUS server.
4. Klõpsake nuppu Lisa... nupp. Sisestage IP address, ühiskasutusega salajane ja NPS serveri pordid. Kui kasutamisel keskne NPS RADIUS klient ja RADIUS target saab sama. Ühiskasutusega salajane peab vastama ühe häälestamise NPS serveri RADIUS kliendi osas.

![RADIUS autentimine](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
