<properties 
    pageTitle="Windowsi autentimist ja Azure Mitmikautentimise Server"
    description="See on Azure mitmekordne autentimine leht, mis aitab Windowsi autentimist ja Azure mitmekordne autentimine serveri juurutamine."
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

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windowsi autentimist ja Azure Mitmikautentimise Server

Windowsi autentimine jaotis võimaldab administraatoril lubamine ja konfigureerimine Windowsi autentimine üks või mitu rakendust.  Järgmises loendis asja meeles pidada enne häälestamist Windowsi autentimist.

-  taaskäivitage arvuti on vaja enne Azure'i Mitmikautentimise jaoks terminaliteenused juba kehtib.
-  Kui "Nõua Azure'i Mitmikautentimise kasutaja match" on märgitud, ja te ei ole loendis kasutaja, ei saa sisse logida kohale, taaskäivitage arvuti pärast.
-  Usaldusväärsete IP-d sõltub sellest, kas rakenduse saate sisestada kliendi IP autentimise abil. Praegu ainult terminaliteenused on toetatud.  







>[AZURE.NOTE]See funktsioon ei toeta turvalist terminaliteenused Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Rakenduse Windowsi autentimise tagamiseks tehke järgmist.

1. Azure'i mitmekordne autentimise Server ikooni Windowsi autentimist.
![Windowsi autentimine](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Märkige märkeruut Luba Windowsi autentimist. Vaikimisi on see ruut on märkimata.
3. Rakendused vahekaardil võimaldab administraatoril üks või mitu rakendust Windowsi autentimise konfigureerimine.
4. Valige serveri või rakendus – saate määrata, kas selle serverirakenduse on lubatud. Klõpsake nuppu OK.
5. Klõpsake nuppu Lisa... nupp.
6. Vahekaart usaldusväärsed IP-d saate vahele jätta Azure'i mitmekordne Windowsi autentimist seansid pärinevate teatud IP-d. Näiteks kui töötajad rakendust office ja kodus, võib juhtuda, te ei soovi oma telefoni helin Azure'i Mitmikautentimise samal ajal office. Selleks määrate office alamvõrgu kirjena usaldusväärsed IP-d.
7. Klõpsake nuppu Lisa... nupp.
8. Valige üks IP, kui soovite vahele jätta ühe IP-aadress.
9. Kui soovite kogu IP-vahemiku vahele jätta, valige IP vahemik. Näide 10.63.193.1-10.63.193.100.
10. Valige alamvõrku, kui soovite määrata erinevaid IP alamvõrgu esituses abil. Sisestage soovitud alamvõrgu käivitamine IP ja valige sobiv Võrgumask rippmenüü loendist.
11. Klõpsake nuppu OK.
