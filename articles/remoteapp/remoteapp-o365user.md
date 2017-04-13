
<properties 
    pageTitle="Kuidas kasutada Office 365 kasutajakontode Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure RemoteApp minu Office 365 Kasutajakontod"
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



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Kuidas kasutada Azure RemoteApp Office 365 Kasutajakontod

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui teil on Office 365 tellimus, on teil Azure Active Directory, kus talletatakse teie kasutajanimed ja paroolid kasutada Office 365 teenustele juurdepääsuks. Näiteks kui teie kasutajad aktiveerida Office 365 ProPlusi need autentimiseks Azure AD litsentside kontrollida. Enamik klientide soovite Azure RemoteApp kasutamine samas kaustas.

Kui juurutate Azure RemoteApp kasutate tõenäoliselt Azure'i tellimus, mis on seotud erinevate Azure AD. Office 365 kataloogi kasutamiseks peate selle kausta Azure tellimuse viimine.

Juurutamise kohta leiate teavet teemast Office 365 klientrakendustes, [Kuidas kasutada Office 365 tellimuse Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Etapp 1: Registreerimine Office 365 Azure Active Directory tasuta tellimuse
Kui kasutate Azure klassikaline portaali, abil juhiseid [registreerida tasuta Azure Active Directory tellimuse](https://technet.microsoft.com/library/dn832618.aspx) oma Azure AD kaudu Azure'i haldusportaal haldus juurdepääs. Selle protsessi tulemusena peaks olema võimalus Azure portaali sisse logida ja vaadata kataloogi seal – sel hetkel ei kuvata palju kasutate Azure RemoteApp täielikku Azure'i tellimus on erinevate kataloogis.

Pidage meeles ja selles juhises loodud administraatorikonto parool – need on vajalik etapp 2.

Kui kasutate Azure portaali, kontrollida, [Kuidas registreerida ja aktiveerida tasuta Azure Active Directory Office 365 portaalis](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Etapp 2: Saate muuta Azure'i AD, Azure tellimusega seostatud.
Me muuta oma praeguse kataloogi Azure tellimuse Office 365 kataloogi me töötanud etapp 1.

Järgige juhiseid teemas [muutmine Azure Active Directory rentniku Azure RemoteApp](remoteapp-changetenant.md). Erilist tähelepanu järgmist:

- Samm #1: Kui olete juurutanud Azure RemoteApp (ARA) see tellimus, veenduge, et eemaldamisel Azure AD kõik kasutajakontod mis tahes ARA saidikogumid kõigepealt enne midagi muud. Teise võimalusena võite kaaluda mis tahes olemasolevaid saidikogumite kustutamine.
- Samm #2: See on kriitiline samm. Peate kasutama Microsofti kontoga (nt @outlook.com) on teenuse administraatorina tellimust; see on Kuna me ei tohi olla mis tahes Kasutajakontod olemasoleva Azure AD, mis on seotud – kui me ei, me ei saa eri Azure AD liikumiseks.
- Samm #4: Olemasoleva kausta lisamisel süsteemi küsib selle kausta jaoks administraatori kontoga sisse logima. Veenduge, et kasutada administraatorikonto etapp 1.
- Samm #5: Vanema directory tellimuse vahetada oma Office 365 kataloogi. Seetõttu peaks olema, et-jaotises Sätted > Teie tellimus on loetletud kataloogi Office 365 tellimust. 
![Vanema directory tellimuse muutmine](./media/remoteapp-o365user/settings.png)
 

Selles etapis tellimuse Azure RemoteApp on seotud teie Office 365 Azure AD; Saate olemasoleva Office 365 kasutajakontode Azure RemoteApp!




