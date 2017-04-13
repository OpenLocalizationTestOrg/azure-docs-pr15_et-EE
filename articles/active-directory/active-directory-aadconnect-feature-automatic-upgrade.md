<properties
   pageTitle="Azure'i AD-ühendus: Automaatne versioonitäiendus | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse sisseehitatud automaatne versioonitäienduse funktsioon Azure'i AD-ühenduse sünkroonimine."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure'i AD-ühendus: Automaatne versioonitäiendus
See funktsioon võeti kasutusele koostamine 1.1.105.0 (avaldatud veebruar 2016).

## <a name="overview"></a>Ülevaade
Veenduge, et teie Azure'i AD-ühenduse install on alati ajakohased on kunagi varem **automaatse versioonitäienduse** funktsioon. See funktsioon on vaikimisi kiire installid ja DirSync uuendamine. Uus versioon, uuendatakse automaatselt installi.

Automaatne versioonitäiendus on vaikimisi järgmist:

- Kiire sätted installi ja DirSync uuendamine.
- Kasutades SQL-i kiire LocalDB, mis on, mis Express sätted Kasuta alati. SQL-i Expressiga DirSync kasutada ka LocalDB.
- AD on loodud Express sätted ja DirSync MSOL_ vaikekonto.
- On vähem kui 100 000 objekte metaversumi.

Praeguse oleku automaatse versiooniuuenduse saab vaadata PowerShelli cmdleti `Get-ADSyncAutoUpgrade`. See on järgmistest.

Olek | Kommentaari
---- | ----
Lubatud | Automaatne versioonitäiendus on lubatud.
Peatatud | Määrake ainult süsteem. Süsteem pole enam saada automaatne uuendamine.
Keelatud | Automaatne versioonitäiendus on keelatud.

Saate muuta vahel **lubatud** või **keelatud** koos `Set-ADSyncAutoUpgrade`. Ainult süsteemi peaks seatud **Suspended**olekus.

Automaatne versioonitäiendus on seisund Azure'i AD-ühenduse kasutamise versioonitäienduse taristu. Automaatse uuendada töötada, veenduge, et olete avanud URL-ide puhverserverisse **Azure'i AD-ühenduse** seisundi nagu [Office 365 URL-id ja IP-aadresside vahemikud](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)dokumenteerida.

Kui server töötab **Sünkroonimise Service Manager** UI, siis uuele versioonile on peatatud enne UI on suletud.

## <a name="troubleshooting"></a>Tõrkeotsing
Kui ühendus installi uuendada ise oodatud viisil, järgige neid juhiseid, et teada saada, mis võiks olla valesti.

Teil tuleb arvestada esmalt automaatse versiooniuuenduse saab proovitakse esimesel päeval uus versioon. Enne täiendamist proovitakse nii, et ärge muretsege, kui installi pole kohe täiendada on ka tahtlikult juhuslikkust.

Kui arvate, et midagi ei ole õige, siis esmalt käivitada `Get-ADSyncAutoUpgrade` tagada Automaatne versioonitäiendus on lubatud.

Seejärel veenduge, et olete avanud vajalik URL-ide tulemüüri või puhverserveri. Automaatne uuendamine on kasutusel Azure'i AD-ühenduse seisund, nagu on kirjeldatud [Ülevaade](#overview). Kui kasutate proxy, veenduge, et seisund on konfigureeritud kasutama [puhverserverit](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Testige ka [seisundi Ühenduvus](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) Azure AD.

Ühenduvus Azure AD, mis on kinnitatud, on aeg on eventlogs uurida. Alustage soovitud Sündmusevaatur ja vaadake, kas **rakenduse** Eventlog. Lisada eventlog filter **Azure'i AD-ühenduse täiendamine** allika ja sündmuse id vahemikus **300-399**.  
![Automaatse versiooniuuenduse Eventlog filter](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Nüüd näete eventlogs, mis on seotud automaatse versiooniuuenduse olek.  
![Automaatse versiooniuuenduse Eventlog filter](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Tulemuse kood on eesliite riigi ülevaade.

Tulem koodi eesliide | Kirjeldus
--- | ---
Edu | Installimine on edukalt täiendatud.
UpgradeAborted | Ajutiste tingimus on peatatud uuele versioonile. See proovitakse uuesti ja eeldatakse, et see ei õnnestu hiljem.
UpgradeNotSupported | Süsteem on blokeerib uuendatakse automaatselt süsteemi konfiguratsioon. See proovitakse kuvamiseks, kui olek muutub, kuid eeldatakse, et süsteemi tuleb muuta käsitsi.

Siit leiate loendi leiate levinumate sõnumid. See loend kõik, kuid tulemi sõnumi peaks olema tühjendage mis probleem on.

Tulemuste teade | Kirjeldus
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Registri ei kirjuta.
UpgradeAbortedInsufficientDatabasePermissions | Sisseehitatud administraatorite rühma ei saa andmebaasi õigused. Uusima versiooni Azure'i AD-ühenduse selle probleemi lahendamiseks käsitsi värskendada.
UpgradeAbortedInsufficientDiskSpace | Pole piisavalt vaba kettaruumi toetada täiendamist.
UpgradeAbortedSecurityGroupsNotPresent | Ei leia ja lahendamiseks kasutada mootori Sünkrooni kõik turberühmad.
UpgradeAbortedServiceCanNotBeStarted | NT teenus **Microsoft Azure AD sünkroonimine** ei saa käivitada.
UpgradeAbortedServiceCanNotBeStopped | NT teenus **Microsoft Azure AD sünkroonimise** peatamine nurjus.
UpgradeAbortedServiceIsNotRunning | NT teenus **Microsoft Azure AD sünkroonimine** ei tööta.
UpgradeAbortedSyncCycleDisabled | [Ajasti](active-directory-aadconnectsync-feature-scheduler.md) SyncCycle suvand on keelatud.
UpgradeAbortedSyncExeInUse | [Sünkroonimise service manager UI](active-directory-aadconnectsync-service-manager-ui.md) on avatud server.
UpgradeAbortedSyncOrConfigurationInProgress | Installimise viisard töötab või väljaspool ajasti oli ajastatud sünkroonimine.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Olete lisanud oma kohandatud reeglite konfiguratsiooni.
UpgradeNotSupportedDeviceWritebackEnabled | Teil on lubatud [seadme tagasikirjutusega](active-directory-aadconnect-feature-device-writeback.md) funktsiooni.
UpgradeNotSupportedGroupWritebackEnabled | Teil on lubatud [rühma tagasikirjutusega](active-directory-aadconnect-feature-preview.md#group-writeback) funktsiooni.
UpgradeNotSupportedInvalidPersistedState | Installimise pole mõni Express sätted või DirSync versioonitäiendus.
UpgradeNotSupportedMetaverseSizeExceeeded | Teil on üle 100 000 objektide metaversumi.
UpgradeNotSupportedMultiForestSetup | Loote rohkem kui üks mets. Kiirhäälestus ühendab ainult üks mets.
UpgradeNotSupportedNonLocalDbInstall | Te ei kasuta SQL Server Express LocalDB andmebaasi.
UpgradeNotSupportedNonMsolAccount | [AD Connectori konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) pole enam MSOL_ vaikekonto.
UpgradeNotSupportedStagingModeEnabled | Server on seatud olema [lavastus režiim](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | Teil on lubatud [kasutaja tagasikirjutusega](active-directory-aadconnect-feature-preview.md#user-writeback) funktsiooni.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
