<properties
    pageTitle="Azure'i AD-ühendus: Järgmised etapid ja kuidas hallata Azure'i AD-ühenduse | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i AD-ühenduse konfiguratsiooni vaike- ja töötoimingute pikendada."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Järgmised etapid ja Azure'i AD-ühenduse haldamine
Funktsionaalseid teemad, mis võimaldavad teil kohandada Azure Active Directory ühendust teie ettevõtte vajadustele ja nõuetele on täpsemalt.  

## <a name="add-additional-sync-administrators"></a>Täiendavad sünkroonimine administraatorite lisamine
Vaikimisi saab kasutaja, kes ei installi ja kohalike administraatorite haldamine installitud sync engine. Täiendavad inimesed saaksid juurdepääsuks ja haldamiseks sync engine, otsige rühma nimega ADSyncAdmins kohalikus serveris ja selle rühma lisada.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Azure'i AD Premium ja ettevõtte mobiilsus kasutajatele litsentside määramine

Nüüd, kui kasutajad on sünkroonitud pilveteenusesse, peate määrama litsentsi nii, et nad saavad alustamine näiteks Office 365 pilve rakendustega.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Azure AD Premium- või Enterprise Mobility komplekti litsentsi määramiseks
--------------------------------------------------------------------------------
1. Logige sisse Azure portaali administraatorina.
2. Valige vasakul **Active Directory**.
3. Topeltklõpsake lehel Active Directory kataloogi, mille soovite lubada kasutajate.
4. Directory lehe ülaosas valige **litsentsid**.
5. Klõpsake lehel litsentside valige Active Directory Premium või ettevõtte mobiilsus ja seejärel klõpsake nuppu **Määra**.
6. Dialoogiboksis, valige kasutajad, millele soovite määrata litsentse ja seejärel klõpsake ikooni märge muudatuste salvestamiseks.


## <a name="verifying-the-scheduled-synchronization-task"></a>Kinnitamise tööülesande ajastatud sünkroonimise
Kui soovite kontrollida sünkroonimise olekut saate seda teha, märkides ruudu Azure'i portaalis.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Ajastatud sünkroonimise ülesande kinnitamiseks
--------------------------------------------------------------------------------
1. Logige sisse Azure portaali administraatorina.
2. Valige vasakul **Active Directory**.
3. Topeltklõpsake lehel Active Directory kataloogi, mille soovite lubada kasutajate.
4. Directory lehe ülaosas valige **Kataloogi integreerimise**.
5. Klõpsake jaotises integreerimine Märkus kohaliku active directory sünkroonimine viimast.

<center>![Pilveteenuse](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Ajastatud sünkroonimise tööülesande käivitamine
Kui peate käivitama ülesande sünkroonimine saate seda teha, käivitades uuesti viisard Azure'i AD-ühenduse kaudu.  Peate oma Azure AD mandaati.  Viisardis valige tööülesandeloendist **kohandamine sünkroonimise suvandid** ja klõpsake nuppu edasi viisardiga. Lõpuks, veenduge, et märgitud on ruut **Käivita sünkroonimine kohe, kui esialgne konfiguratsioon on lõpule jõudnud** .

<center>![Pilveteenuse](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Azure'i AD-ühenduse sünkroonimise kohta lisateabe saamiseks: ajasti, leiate [Azure'i AD-ühenduse ajasti](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Täiendavad toimingud saadaval Azure'i AD-ühenduse
Pärast algse installi Azure'i AD-ühenduse, saate alati käivitada viisardi uuesti Azure'i AD-ühenduse start lehe või töölaua otsetee.  Märkate, et läheb viisardiga uuesti pakub ka uusi võimalusi lisatoiminguid kujul.  

Järgmises tabelis esitatakse need ülesanded ja lühikirjeldus kokkuvõte igal neist.

![Reegli liitumine](./media/active-directory-aadconnect-whats-next/addtasks.png)


Täiendavad ülesanne | Kirjeldus
------------- | ------------- |
Saate vaadata valitud stsenaarium  |Võimaldab vaadata praeguse Azure'i AD-ühenduse lahendus.  See hõlmab üldsätted, sünkroonitud kataloogide, sätete sünkroonimine, jne.
Sünkroonimise suvandite kohandamine | Saate muuta konfiguratsioonile, sh lisada täiendavad Active Directory kogumit konfiguratsiooni või lubamine sünkroonimine suvandid, nt kasutaja, rühma, seadme või parooli allahindluse.
Luba lavastus režiim |  See võimaldab teil etapi teave, mida hiljem sünkroonida, kuid midagi eksporditakse Azure AD või Active Directory.  See võimaldab teil eelvaade kuvatakse sünkroonimise enne nende tekkimist.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
