<properties
    pageTitle="Luba Azure AD Rakenduse puhverserveri | Microsoft Azure'i"
    description="Rakenduse puhverserveri Azure klassikaline portaali sisse lülitada, ja installige konnektorid tagant puhverserveri jaoks."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Azure'i portaalis rakenduse puhverserveri lubamine

Selles artiklis tutvustatakse toimingute Microsoft Azure AD puhverserverit cloud kataloogi Azure AD.

Kui te pole varem selliseid mis rakenduse puhverserveri aitavad teha kohta leiate lisateavet selle kohta, [Kuidas tagada turvaline juurdepääs remote kohapealse rakendused](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Rakenduse puhverserveri eeltingimused
Enne kui saate lubada ja kasutada rakenduse puhverserveri teenuseid, peate olema:

- [Microsoft Azure AD basic või premium tellimuse](active-directory-editions.md) ja Azure AD kataloogi, mis olete üldadministraator.
- Serveris, kus töötab Windows Server 2012 R2 või Windows 8.1 või uuem versioon, mille saate installida rakenduse puhverserveri konnektor. Rakenduse puhverserveri teenuste pilveteenuses taotlused saadab server ja seda on vaja rakendusi, mida te avaldate HTTP- või HTTPS ühenduse.

    - Ühekordse sisselogimise avaldatud rakenduste, selle arvuti peaks olema domeeniga liitunud sama nimega rakendusi, mida te avaldate AD domeeni.

- Kui tee on tulemüüri, veenduge, et, et see on avatud, et konnektor saab teha rakenduse puhverserveri HTTPS (TCP) taotlused. Konnektor kasutatakse koos alamdomeene, mis on osa üksikasjalik domeenid msappproxy.net ja servicebus.windows.net järgmised pordid. Veenduge, et avada **Väljaminev** liiklus järgmised pordid.

  	| Pordi Number | Kirjeldus |
  	| --- | --- |
  	| 80 | Luba HTTP väljamineva liikluse turvalisuse valideerimine. |
  	| 443 | Kasutaja autentimise vastu Azure AD (nõutav ainult konnektori registreerimise käigus) |
  	| 10100 – 10120 | Kassa HTTP vastused saadetakse tagasi puhverserverist lubamine |
  	| 9352, 5671 | Luba suhtlust konnektor suunas sissetulevad taotlused Azure'i teenus. |
  	| 9350 | Valikuline lubamine parema jõudluse, sissetulevad taotlused |
  	| 8080 | Luba konnektor bootstrap järjestust ja konnektori automaatne uuendamine |
  	| 9090 | Konnektor registreerimist (nõutav ainult konnektori registreerimise käigus) lubamine |
  	| 9091 | Konnektor usalda serdi automaatse pikendamise lubamine |

    Kui teie tulemüür jõustab vastavalt pärit kasutajad, avage järgmised pordid liikluse pärit võrgu teenust Windows teenuseid. Lisaks veenduge, et NT Authority\System port 8080 lubamise kohta.

- Kui teie asutus kasutab puhverserverid Interneti-ühenduse, palun Heitke pilk üksikasjalikku [töötamine olemasoleva kohapealse puhverserverid](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) ajaveebipostituse need konfigureerimise kohta.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Samm 1: Lubada Azure AD rakenduse puhverserver
1. Logige sisse administraatorina [Azure klassikaline portaalis](https://manage.windowsazure.com/).
2. Avage Active Directory ja valige kaust, kus soovite lubada rakenduse puhverserver.

    ![Active Directory - ikoon](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Valige **Konfigureeri** directory lehelt ja liikuge kerides allapoole jaotiseni **Rakenduse puhverserver**.
4. Tavalise **Lubamine puhverserveri rakendusteenuste selle kausta jaoks** **lubatud**.

    ![Rakenduse puhverserveri lubamine](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Valige **Laadi kohe alla**. See viib teid **Azure AD Rakenduse puhverserveri konnektor alla laadida**. Lugeda ja litsentsitingimustega ja klõpsake nuppu **Laadi alla** salvestada Windows Installeri fail (.exe) konnektor.

## <a name="step-2-install-and-register-the-connector"></a>Samm 2: Installida ja registreerida konnektor
1. Käivitage **AADApplicationProxyConnectorInstaller.exe** server, olete valmis vastavalt eeltingimused.
2. Järgige viisardi installimiseks.
3. Installimise ajal saate küll, palutakse teil registreerida rakenduse puhverserveri oma Azure AD rentniku konnektor.

  - Sisestage mandaat Azure AD üldadministraator. Teie üldadministraator rentniku võib erineda Microsoft Azure'i mandaat.
  - Veenduge, et administraator, kes registreerib konnektor on kui märkisite rakenduse puhverserveri teenuse samas kaustas. Näiteks kui rentniku domeen on contoso.com, admin peaks olema admin@contoso.com või mis tahes muud pseudonüüm domeenis.
  - Kui **IE täiustatud turvalisus konfiguratsiooni** on seatud **klõpsake** serveris kui installite konnektor, võib olla blokeeritud registreerimise ekraani. Järgige juurdepääsu lubamiseks kuvatakse tõrketeade. Veenduge, et Internet Explorer täiustatud turvalisus on välja lülitatud.
  - Kui konnektor registreerimine ei õnnestu, lugege teemat [Tõrkeotsing rakenduse puhverserver](active-directory-application-proxy-troubleshoot.md).  

4. Kui installimine on lõpule jõudnud, lisatakse teie serveri kaks uusi teenuseid.

    - **Microsoft AAD rakenduse puhverserveri Connector** võimaldab Ühenduvus
    - **Microsoft AAD rakenduse puhverserveri konnektor Updater** on automaatne uuendamine teenus, mis perioodiliselt uusi versioone konnektor otsib ning värskendab konnektor vastavalt vajadusele.

    ![Rakenduse puhverserveri konnektor teenused - kuvatõmmis](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Klõpsake aknas installimine **lõpule** .

Kõrge-saadavus huvides peaks juurutamist vähemalt kaks konnektorid. Veel konnektorid juurutada, korrake ülaltoodud juhiseid 2 ja 3. Iga konnektor peab olema eraldi registreeritud.

Kui soovite desinstallida konnektor, desinstallige nii konnektori teenuse ja Updater teenus. Taaskäivitage arvuti teenuse täielikult eemaldada.


## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis [Avalda](active-directory-application-proxy-publish.md)rakenduste rakenduse puhverserver.

Kui teil on rakendusi, mis on eraldi võrkude või eri kohti, saate eri konnektorid loogiline üksuste korraldamiseks konnektor rühmad. Lisateavet [rakenduse puhverserveri konnektorid töötamise](active-directory-application-proxy-connectors.md)kohta.
