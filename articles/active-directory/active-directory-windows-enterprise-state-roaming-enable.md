<properties
    pageTitle="Luba ettevõtte olekus rändlusteenuse Azure Active Directory | Microsoft Azure'i"
    description="Korduma kippuvad küsimused Windows seadmete sätete Enterprise olekus rändlusteenuse kohta. Ettevõtte olekus rändlusteenuse võimaldab kasutajatel on ühendatud kogemus kõigis seadmetes Windows ja vähendab vaja konfigureerida uue seadme aega."
    services="active-directory"
    keywords="ettevõtte olekus rändluse windows pilves, enterprise olekus rändlusteenuse lubamine"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Luba ettevõtte olekus rändlusteenuse Azure Active Directory 's

Ettevõtte olekus rändlusteenuse on saadaval mis tahes organisatsiooni Premium Azure Active Directory (Azure AD) tellimus. Lisateavet selle kohta, kuidas hankida Azure AD tellimus, lugege teemat [Azure AD toote lehele](https://azure.microsoft.com/services/active-directory).

Kui ettevõtte olekus rändlusteenuse, ettevõtte automaatselt antakse tasuta, piiratud kasutada tellimuse jaoks litsentside Azure Rights Management. Selles tasuta tellimus on piiratud krüptimine ja dekrüptimine enterprise sätted ja rakenduse andmete sünkroonitud Enterprise olekus rändlusteenuse teenus; tasulisele tellimusele Azure Rights Management täielik võimaluste kasutamiseks peab teil olema.

Pärast Azure AD Premium tellimuse, tehke järgmist lubamiseks Enterprise olekus rändlusteenuse.

1. Azure'i klassikaline portaali sisse logida.
2. Klõpsake vasakul, valige **ACTIVE DIRECTORY**ja valige kataloogi, mille jaoks soovite lubada Enterprise olekus rändlusteenuse.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Minge menüüsse **KONFIGUREERIMINE** peal.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Kerige lehel Valige **kasutajad võivad SÜNKROONIDA sätted ja ettevõtte rakenduse andmed**ja seejärel klõpsake nuppu **Salvesta**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Windows 10 seade ringi liikuda Enterprise olekus rändlusteenus sätted, peab seadme autentida abil identiteedi Azure AD. Seadmetes, mis on ühendatud Azure AD esmane kasutajanime on Azure AD identiteedi nii täiendavaid konfiguratsiooni on vaja. IT-administraator peab seadmed, mis kasutavad traditsioonilise kohapealse Active Directory, [domeeni ühendatud seadmete Azure AD Windows 10 kogemusi](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Sünkroonimise andmete talletamine
Ühe või mitme [Azure'i piirkondade](https://azure.microsoft.com/regions/ ) kõige joondatakse koos Azure Active Directory eksemplar riik/regioon väärtus on majutatud Enterprise olekus rändlusteenuse andmete. Enterprise olekus rändlusteenuse andmed on liigendatud kolme põhilist geograafiliste piirkondade alusel: Põhja-Ameerika, EMEA ja APAC. Ettevõtte olekus rändlusteenuse andmed rentniku asub kohalikult geograafilise asukoha ja kopeeritud piirkondade lõikes.  Näiteks kliendid, kellel on mõni EMEA regiooni riikides riik/regioon väärtus nagu "Prantsusmaa" või "Sambia" on nende andmete majutatud ühel või Azure piirkondade Euroopa.  Kliendid, kes oma riik/regioon väärtus Azure AD ühele Põhja-Ameerika riigid nagu "USA" või "Kanada" on majutatud ühe või mitme Azure'i piirkondade USA nende andmete.  Kliendid, kes oma riik/regioon väärtus Azure AD ühte APAC riiki nagu "Austraalia" või "Uus-Meremaa" on majutatud ühte või mitut Azure regioonide Aasias nende andmete.  Lõuna-Ameerika riigid ja Antarktika andmed olema majutatud ühe või mitme Azure'i piirkondades USA.  Riik/regioon väärtus on määratud Azure AD directory loomisprotsessi osana ja hiljem muuta. 

Kui vajate rohkem üksikasju, klõpsake andmete talletuskoht, palun fail on Piletite koos [Azure'i tugi](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Ettevõtte olekus rändlusteenuse haldamine
Azure'i AD globaalse administraatorid lubada ja keelata Enterprise olekus rändlusteenuse Azure klassikaline portaalis.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globaalne administraatorite saate piirata teatud turberühmad sätete sünkroonimine.

Üldadministraatorid saate vaadata ka valida teatud kasutaja Active Directory eksemplar **kasutajate** loend ja klõpsake vahekaarti **seadmed** ja valige vaate **sätted ja ettevõtte rakenduse andmete sünkroonimine seadmete**kasutaja seadme sünkroonimine olekuaruanne.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Andmete säilitamine
Azure'i Enterprise olekus rändlusteenuse kaudu sünkroonitud andmete säilitatakse lõputult kui käsitsi Kustutustoiming sooritatakse või andmete määratakse olla aegunud. 

**Konkreetsete kustutamise:** Andmed on kustutatud Azure administraator kustutab kasutaja või kataloogis või administraator taotleb selgesõnaliselt, et andmed on välja jätta.

- **Kasutajate kustutamise**: Kasutaja kustutamisel Azure AD rändlusteenuse andmete kasutajakonto kustutamiseks märgitud ja kustutatakse abil 180 90 päeva. 
- **Kustutamiseks Directory**: Azure AD terve kausta kustutamine on kohe toiming. Sätted andmete seotud, et kataloogi kustutamiseks märgitud ja kustutatakse abil 180 90 päeva. 
- **Taotluse kustutamise**: kui Azure AD administraator soovib käsitsi mõne kindla kasutaja või sätted andmete kustutamiseks, admin saate faili lisamine Piletite Azure [tugi](https://azure.microsoft.com/support/). 

**Aegunud andmete kustutamine**: üks aasta ("säilitusperiood") käsitletakse Restoran ja on kustutatud Azure on pole külastatud andmeid. Säilitusperiood muutuda, kuid ei ole väiksem kui 90 päeva. Kehtetud andmed võivad olla määratud Windowsi/rakendus sätted või kasutaja jaoks kõik sätted. Näiteks:
 
- Kui ei pääse teatud sätted (nt, eemaldatakse rakenduse seadme või sätete rühm, näiteks "Teema" on keelatud kõikides kasutaja seadmetes), siis selle saidikogumi muutub pärast säilitusperiood aegunud ja on kustutatud. 
- Kui kasutaja on välja lülitatud sätete sünkroonimine kõigis oma seadmetes, siis pole andmete sätete juurde ja kõik sätted andmed kasutaja jaoks muutub aegunud ja on kustutatud pärast säilitusperiood. 
- Kui Azure AD directory administraator lülitatakse välja Enterprise olekus rändlusteenuse kogu kataloogist, siis kõikide kasutajate jaoks katkestab directory sünkroonimise sätted ja kõik sätted andmed kõikide kasutajate jaoks muutub aegunud ja on kustutatud pärast säilitusperiood. 

**Kustutatud andmete taastamine**: säilituspoliitika andmeid ei saa konfigureerida. Kui andmed on jäädavalt kustutatud, seda ei saa Taastatavad. Siiski on oluline märkida sätted andmed ainult Azure, mitte lõppkasutaja seadme kustutatakse. Kui mis tahes seadme taastatakse hiljem ühendus Enterprise olekus rändlusteenus, sünkroonitud ja Azure talletatud uuesti sätteid.


## <a name="related-topics"></a>Seotud teemad
- [Ettevõtte olekus rändlusteenuse ülevaade](active-directory-windows-enterprise-state-roaming-overview.md)
- [Sätted ja andmete rändluse FAQ](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Rühma poliitika ja MDM-i sätete sätete sünkroonimine](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 rändluse sätted viited](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
