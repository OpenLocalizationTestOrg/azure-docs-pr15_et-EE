<properties
    pageTitle="Automaatse seadme registreerimine Windows 8.1 domeeni ühendatud seadmete jaoks konfigureerida | Microsoft Azure'i"
    description=" Windows 8.1 domeeni ühendatud seadmete automaatselt registreerimiseks Azure AD rühmapoliitikaga juhiseid. "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Automaatse seadme registreerimine Windows 8.1 domeeni ühendatud seadmete jaoks konfigureerimine

Active Directory rühmapoliitika abil saate konfigureerida oma Windows 8.1 ühendatud domeeni seadmed automaatselt registreerimiseks Azure AD. Rühmapoliitika konfigureerimiseks peavad teil olema installitud rühmapoliitika funktsiooniga vähemalt üks domeeni ühendatud Windows Server 2012 R2 või Windows 8.1 arvuti. Kui domeeni jaoks on lubatud seda rühmapoliitika, mis tahes domeeni kasutaja logib kohale automaatselt ja vaikselt registreeritakse seadme objekti Azure AD. Seal on üks seadme objekti Azure AD iga seadme registreeritud kasutaja. Kindlasti lugege läbi ja täitke eeltingimused automaatse seadme registreerimine Azure Active Directory for Windows Domain-Joined seadmete kaudu.

>[AZURE.NOTE]
 Uusima juhised, kuidas häälestada automaatse seadme registreerimine teemadest, [Kuidas seadistada automaatse registreerimise Windowsi domeeni liidetud seadmete Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Rühmapoliitika oma Windows 8.1 domeeni ühendatud seadmete jaoks

1. Avage Server Manager ja liikuge **Tööriistad** > **Rühmapoliitika haldus**.
2. Kaudu rühmapoliitika, liikuge domeeni, kus soovite lubada **Automaatse töökoha liituda**domeeni vastav sõlm.
3. Paremklõpsake **Rühmapoliitika objektid** ja valige **Uus**. Pange rühmapoliitika objekti nimi, näiteks **Automaatse töökoha liituda**. Klõpsake nuppu **OK**.
4. Paremklõpsake oma uue rühmapoliitika objekti ja seejärel valige **Redigeeri**.
5. Liikuge **Arvuti konfiguratsioon** > **poliitikate** > **haldus mallid** > **Windowsi komponentide** > **töökoha liituda**.
6. Paremklõpsake automaatselt töökoha Liitu klientarvutite ja valige käsk **Redigeeri**.
7. Klõpsake raadionuppu lubatud ja seejärel klõpsake nuppu Rakenda. Klõpsake nuppu **OK**.
8. Teie valitud asukohta nüüd rühmapoliitika objekti linkida. Link sellele seadnud kõigi ühendatud domeeni Windows 8.1 seadmete teie asutuses lubamiseks domeeni rühmapoliitika.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Windows 8.1 domeeni registreerimise tühistamine liitunud seadmed

Võite valida unregister oma domeeni liitunud Windows 8.1 seadmed, tehes järgmist: muutmine eelmises tööetapis loodud töökoha liitumine rühmapoliitika sätted. Määrake soovitud automaatselt töökoha Liitu arvutite Kliendipoliitika erivajadustega. See aitab vältida uute seadmete automaatselt liitumise töökohal.

Olemasoleva domeeni liitunud Windows 8.1 masinad unregister kahe üks järgmistest:

* Variant 1: **registreerimise tühistamise Windows 8.1 domeeni ühendatud seadme arvuti sätete abil**
  1. Windows 8.1 seadmes, liikuge **Arvutisätete** > **võrgu** > **töökoha**
  2. Valige **jätta**.
Iga domeeni kasutaja, mis on masina sisse logitud ja on automaatselt töökoha liitunud tuleb seda toimingut korrata.

* Variant 2: Unregister Windows 8.1 domeeni ühendatud seadet skripti abil
    1. Avage Windows 8.1 arvutis Käsuviip ja käivitage järgmine käsk:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
See käsk peate käivitama iga kasutaja domeeni, mis on allkirjastatud kontekstis kohale.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Sündmusevaatur ja tõrked Windows 8.1 domeeni ühendatud seadmete

Windowsi sündmuselogi Windows 8.1 arvutis kuvatakse sõnumid, mis on seotud seadme registreerimine. Sõnumite leiate eduka nii õnnestu sündmuste jaoks. 

Sündmuste logi võib leida sündmuse vaatur jaotises rakenduste ja teenuste **logid** > **Microsoft** > **Windows > töökoha liitumine**.

##<a name="additional-details"></a>Täiendavad üksikasjad

Rühmapoliitika võimaldab Ajastatud ülesanne süsteemi, mis töötab kasutaja konteksti ja käivitab kasutaja sisselogimine kohta. Tööülesande vaikselt register kasutaja ja seadme Azure AD pärast lõpuleviimist Logi sisse. Ajastatud ülesanne leiate selle Toiminguajasti teek jaotises **Microsoft**Windows 8.1 seadmete > **Windows** > **Töökoha liituda**. Tööülesande kestab ja kõik Active Directory kasutajad selle Logi-sisse registreerida. 

## <a name="additional-topics"></a>Täiendavad Teemad
- [Azure Active Directory seadme registreerimine ülevaade](active-directory-conditional-access-device-registration-overview.md)
- [Automaatse seadme registreerimine Azure Active Directory Windows 10 jaoks domeeni ühendatud seadmete](active-directory-conditional-access-automatic-device-registration.md)
- [Automaatse seadme registreerimine Windows 7 ühendatud domeeni seadmete jaoks konfigureerimine](active-directory-conditional-access-automatic-device-registration-windows7.md)

