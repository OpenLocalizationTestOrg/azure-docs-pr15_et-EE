<properties
   pageTitle="Azure Active Directory auditilogi aruande sündmused | Microsoft Azure'i"
   description="Auditeerida vaatamine ja allalaadimine oma Azure Active Directory jaoks saadaolevad sündmused"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory auditilogi aruande sündmused

*Need dokumendid on osa [Azure Active Directory aruandluse juhend] (aktiivne-directory aruandlus – guide.md).*

Azure Active Directory auditilogi aruande aitab tuvastada õigustega toiminguid, mida saavad Azure Active Directory toimunud kliendid. Õigustega hõlmavad kõrguse muutusi (nt rolli loomise või parooli lähtestamise) muutmisega poliitika konfiguratsioone (nt parooli poliitikate) või muudatuste kataloogi konfiguratsioon (nt muudatuste domain federation sätted). Aruannetes auditilogi kirje sündmuse nimi, kes sooritatud toimingu, muutmine, kuupäev ja kellaaeg (UTC) poolt mõjutatud target ressursi osaleja. Kliendid saavad tuua auditilogi sündmused loendi jaoks oma Azure Active Directory [Azure portaali](https://portal.azure.com/)kaudu, nagu on kirjeldatud vaates [Oma Audit logid](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Auditilogi aruande sündmuste loendi
<!--- audit event descriptions should be in the past tense --->

Sündmused                               | Sündmuse kirjeldus
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Kasutaja sündmused**                      |
Kasutaja lisamine                             | Kasutaja lisatakse kataloogi.
Kasutaja kustutamine                          | Kustutatud kasutaja kataloogist.
Litsentsi määramine atribuudid               | Kataloogis kasutaja jaoks litsentsi atribuutide seadmine.
Kasutaja parooli lähtestamine                  | Kataloogis kasutaja parooli lähtestamine.
Kasutaja parooli muutmine                 | Kataloogis kasutaja parooli muuta.
Muuda kasutajalitsentsi.                  | Muudetud kataloogis kasutajale määratud litsents. Kui soovite näha, millised litsentsid värskendamine, pilk [värskenduse kasutaja](#update-user-attributes) atribuutide allpool
Värskenda kasutaja                          | Värskendatud kataloogis kasutaja. [Allpool on](#update-user-attributes) atribuute, mida saab värskendada.
Jõusta seadmine kasutaja parooli muutmine       | Määrake atribuudi, mis jõustab kasutaja parooli login.
Kasutajatunnust värskendamine        | Kasutaja parooli muutnud  
**Jaotises sündmused**                     |
Rühma lisamine                            | Kataloogis loonud rühma.
Rühma värskendamine                         | Värskendatud kataloogis rühma. Näha, millised atribuudid jaotises värskendamine, vaadake allpool jaotises [Rühma atribuudid auditeerida](#update-group-attributes)
Rühma kustutamine                         | Kustutatud rühma kataloogist.
Liikme lisamine rühma            | Lisatud kataloogis rühma liige.
Liikme eemaldamiseks rühmast         | Eemaldada kataloogis rühma liige.
CreateGroupSettings              | Loodud rühma sätted
UpdateGroupSettings          | Värskendatud rühma sätted. Näha, millised Rühmasätete värskendamine, vaadake allpool jaotises [Rühma atribuudid auditeerida](#update-group-attributes)
DeleteGroupSettings            | Kustutatud rühma sätted
SetGroupLicense                  | Rühma litsentsi.
SetGroupManagedBy              | Rühma haldab kasutaja
AddGroupMember               | Lisatud rühma liige
RemoveGroupMember            | Liikme eemaldamiseks rühmast
AddGroupOwner                | Lisatud rühma omanik
RemoveGroupOwner                 | Rühmast eemaldatud omanik
**Rakenduse sündmused**               |
Teenuse põhilise lisamine                | Teenuse põhilise lisatakse kataloogi.
Teenuse põhilise eemaldamine             | Eemaldatud teenuse põhilise kataloogist.
Peamine mandaati lisamine    | Teenuse põhilise lisatud mandaate.
Peamine mandaati eemaldamine | Eemaldatud identimisteabe põhisumma teenust.
Delegeerimine kirje lisamine                 | Kataloogis loonud mõne [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) .
Kirje määramine delegeerimine                 | Värskendatud [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) kataloogis.
Delegeerimine kirje eemaldamine              | Kustutatud [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) kataloogis.
**Roll sündmused**                      |
Rollirühma rolli liikme lisamine              | Kasutaja lisatakse directory roll.
Rollirühma rolli liige eemaldamine         | Eemaldada kasutajalt directory roll.
Ettevõtte kontaktteabe seadmine      | Ettevõtte tasemel kontakti eelistuste määramine. See hõlmab e-posti aadressid, turundus, samuti tehnilise teenuse Microsoft Online Services kohta teatiste saamiseks.
Delegeerimine kirje lisamine                 | Kataloogis loonud mõne [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) .
Kirje määramine delegeerimine                 | Värskendatud [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) kataloogis.
Delegeerimine kirje eemaldamine              | Kustutatud [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) kataloogis.
AddSevicePrincipalOwner          | Teenuse põhilise lisatud omanik.
RemoveSevicePrincipalOwner   | Teenuse põhilise omanik eemaldada.
AddApplication  | Lisa rakendus.
UpdateApplication | Rakenduse värskendamine Näha, millised rakenduse sätete värskendamine, vaadake allpool olevat jaotist [Rakenduse atribuudid auditeerida](#update-application-attributes)
DeleteApplication | Rakenduse kustutada.
RestoreApplication|Rakenduse taastada.
AddApplicationOwner   |     Lisage rakenduse omanik.
RemoveApplicationOwner    |     Omaniku eemaldamiseks rakenduse.
**Roll sündmused**                         |
Rollirühma rolli liikme lisamine                 | Kasutaja lisatakse directory roll.
Rollirühma rolli liige eemaldamine            | Eemaldada kasutajalt directory roll.
AddRoleDefinition               | Lisatud rolli määratlus.
UpdateRoleDefinition                | Värskendatud rolli määratlus. Näha, millised rollirühma sätete värskendamine, et viidata [Rolli määratlus atribuudid auditeerida](#update-role-definition-attributes) allpool jaotises
DeleteRoleDefinition                | Kustutatud rolli määratlus.
AddRoleAssignmentToRoleDefinition | Lisatud Rollimäärang roll määratlus.
RemoveRoleAssignmentFromRoleDefinition | Eemaldatud Rollimäärang rolli määratlus.
AddRoleFromTemplate     | Lisatud rolli malli põhjal.
UpdateRole  | Värskendatud roll.
AddRoleScopeMemberToRole    | Lisatud jäävates liikme rollile.
RemoveRoleScopedMemberFromRole  | Eemaldatud jäävates rolli liige.
**Seadme sündmuste (kõigi uute sündmuste)**                    |
AddDevice               | Lisatud seade.
UpdateDevice            | Värskendatud seadme. Näha, mida seadme atribuutide värskendamine, vaadake allpool olevat jaotist [seadme atribuutide auditeeritud](#update-device-attributes)
DeleteDevice            | Kustutatud seade.
AddDeviceConfiguration      | Lisatud seadme konfigureerimine.
UpdateDeviceConfiguration   | Värskendatud seadme konfigureerimine. Näha, mida seadme konfiguratsiooni atribuutide värskendamine, vaadake allpool olevat jaotist [seadme konfiguratsiooni atribuudid auditeeritud](#update-device-configuration-attributes)
DeleteDeviceConfiguration   | Kustutatud seadme konfiguratsiooni.
AddRegisteredOwner     | Seadme lisatud registreeritud omanik.
AddRegisteredUsers     | Seadme registreeritud kasutajat lisada.
RemoveRegisteredOwner    | Seadme registreeritud omanik eemaldada.
RemoveRegisteredUsers    | Seadme registreerunud kasutajad eemaldada.
RemoveDeviceCredentials  | Eemaldage seadme mandaat.
**B2B sündmused**                       |
Paketi kutsed üles laadida.              | Administraator on üles laaditud faili, mis sisaldab kutsete partneri kasutajatele saata.
Paketi kutsed töödeldud.             | Faili, mis sisaldab kutsete partneri kasutajad on töödeldud.
Väliste kasutajate kutsumine.                | Väliskasutaja on kutsutud kataloogi.
Väliste kasutajate kutsumine Lunasta.         | Väliskasutaja on lunastada saavad kutse kataloogi.
Väliste kasutajate lisamine rühma.          | Väline kasutaja on määratud liikmelisuse kataloogis rühma.
Väliste kasutajate määrata rakenduse. | Väline kasutaja on määratud otse rakenduse.
Viiruse rentniku loomine.               | Kutse tagastusväärtus on loodud Azure AD uue rentniku.
Viiruse kasutaja loomist.                 | Kasutaja on loonud olemasoleva rentniku Azure AD kutse väljaostmine.
**Haldus üksused (kõigi uute sündmuste)**             |
AddAdministrativeUnit   |   Lisage haldus üksus.
UpdateAdministrativeUnit|   Värskenduse haldus üksus. Näha, millised haldus üksuse atribuutide värskendamine, vaadake allpool jaotises [haldus üksuse atribuutide auditeerida](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Haldus üksuse kustutada.
AddMemberToAdministrativeUnit|  Liikme lisamiseks haldus üksus.
RemoveMemberFromAdministrativeUnit|     Liikme eemaldamine haldus üksus.
**Directory sündmused**                 |
Ettevõtte partneri lisamine               | Partneri lisatakse kataloogi.
Ettevõtte partneri eemaldamine          | Eemaldatud partneri kataloogist.
DemotePartner   |   Partneri langetamine.
Ettevõtte domeeni lisamine                | Domeeni lisanud kataloogi.
Ettevõtte domeeni eemaldamine           | Kataloogi domeeni eemaldada.
Värskenda Domeen                        | Värskendatud domeeni kataloogi. Näha, millised domeeni atribuutide värskendamine, vaadake allpool olevat jaotist [Atribuudid domeeni auditeerida](#update-domain-attributes)
Seadmine domeeni autentimine            | Muudetud ettevõtte domeeni vaikesätteks.
Ettevõtte kontaktteabe seadmine      | Ettevõtte tasemel kontakti eelistuste määramine. See hõlmab e-posti aadressid, turundus, samuti tehnilise teenuse Microsoft Online Services kohta teatiste saamiseks.
Domeeni federation sätete määramine    | Domeeni federation sätteid värskendada.
Domeeni kinnitamine                        | Kataloogi Domeen kinnitatud.
Kinnitage e-posti kinnitatud Domeen         | Kinnitatud Domeen abil meilikinnituse kataloogi.
Ettevõtte DirSyncEnabled lipu seadmine   | Määrake atribuudi, mis võimaldab kataloogi Azure AD sünkroonimine.
Määra parool poliitika                  | Kasutaja parooli pikkus ja märgi piiranguid seada.
Sea ettevõtte teave              | Värskendatavat teavet ettevõtte tasemel. [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShelli cmdleti üksikasjalikumat teavet teemast.
SetCompanyAllowedDataLocation  |    Ettevõtte määramine lubatud andmete asukoht.
SetCompanyDirSyncEnabled    |   Määrake DirSyncEnabled lipp.
SetCompanyDirSyncFeature |  Määrake DirSync funktsiooni.
SetCompanyInformation |   Sea ettevõtte teave.
SetCompanyMultiNationalEnabled    |     Seadmine ettevõtte rahvusvaheline funktsioon on sisse lülitatud.
SetDirectoryFeatureOnTenant   |     Sea kataloog funktsiooni rentniku kohta.
SetTenantLicenseProperties  |   Seadmine rentniku litsentsi atribuudid.
CreateCompanySettings   |   Ettevõtte sätted loomine
UpdateCompanySettings   |   Ettevõtte sätete värskendamine. Näha, millised ettevõtte atribuutide värskendamine, vaadake allpool jaotises [ettevõtte atribuudid auditeerida](#update-company-attributes)
DeleteCompanySettings   |   Kustuta ettevõtte sätted
SetAccidentalDeletionThreshold   |  Piirmäära seadmine juhuslikku kustutamist.
SetRightsManagementProperties   |   Teabeõiguste halduse atribuutide seadmine.
PurgeRightsManagementProperties     |   Likvideerite teabeõiguste halduse atribuudid.
UpdateExternalSecrets   |   Välise saladusi värskendamine
**Poliitika sündmused (kõigi uute sündmuste)**           |
AddPolicy   |   Lisage poliitika.
UpdatePolicy    |   Värskendage poliitika.
DeletePolicy    |   Kustutage poliitika.
AddDefaultPolicyApplication     |   Rakenduse poliitika lisada.
AddDefaultPolicyServicePrincipal    |   Saate lisada teenuse põhilise poliitika.
RemoveDefaultPolicyApplication  |   Poliitika eemaldamine rakendusest.
RemoveDefaultPolicyServicePrincipal     |   Teenuse põhilise poliitika eemaldamine.
RemovePolicyCredentials     |   Eemaldamine teabehalduspoliitika mandaat.

## <a name="audit-report-retention"></a>Auditilogi aruande säilitus.
Sündmuste Azure AD auditilogi aruande säilitatakse 180 päeva. Säilituspoliitika aruannete kohta leiate lisateavet teemast [Azure Active Directory aruande Säilituspoliitikad](active-directory-reporting-retention.md).

Klientidele, kes talletamise sündmustega auditilogi säilituspoliitika pikemaks, saab kasutada aruannete API regulaarselt tõmba auditilogi sündmused eraldi andmebaasi. Üksikasjad leiate [Alustamine aruannete API-ga](active-directory-reporting-api-getting-started.md) .

## <a name="properties-included-with-each-audit-event"></a>Auditilogi iga sündmuse kaasas atribuudid

Atribuut      | Kirjeldus
------------- | --------------------------------------------------------------
Kuupäev ja kellaaeg | Kuupäeva ja kellaaja auditilogi sündmusele
Näitleja         | Kasutaja või teenuse põhilise, mis sooritatud toiming
Toiming        | Toiming, mille
Target (sihtkoht)        | Kasutaja või teenuse põhilise, mis on sooritatud tegevus


## <a name="update-user-attributes"></a>"Värskendada kasutaja" atribuudid
"Värskenda kasutaja" auditi sündmus sisaldab täiendavat teavet, mida kasutaja atribuutide värskendati. Iga atribuut on kaasatud nii eelmine väärtus ja uus väärtus.

Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Kasutaja saab sisse logida.
AssignedLicense                     | Kõik kasutaja määratud litsentsid.
AssignedPlan                        | Teenus lepingute tuleneb kasutajale määratud litsentsid.
LicenseAssignmentDetail             | Üksikasjade kasutajale määratud litsentsid. Näiteks kui rühma-põhise litsentsimise on kaasatud, see hõlmab rühma, kus antud litsentsi.
Mobile                              | Kasutaja mobiiltelefon.
OtherMail                           | Kasutaja alternatiivne meiliaadress.
OtherMobile                         | Kasutaja alternatiivse mobiiltelefon.
StrongAuthenticationMethod          | Kontrollimise meetodite konfigureeritud kasutaja Mitmikautentimise, nt kõne kõneposti, SMS või kinnitamine koodi mobiilirakenduses loend.
StrongAuthenticationRequirement     | Kui Mitmikautentimise jõustamist lubatud või keelatud selle kasutaja jaoks.
StrongAuthenticationUserDetails     | Kasutaja telefoninumber, alternatiivne telefoninumber ja e-posti aadress kasutada Mitmikautentimise ja parooli lähtestamine kontrollimiseks.
StrongAuthenticationPhoneAppDetail  | Üksikasjalike forof telefoni rakenduste registreeritud täitmiseks 2FA sisselogimine
TelephoneNumber                     | Kasutaja telefoninumber.
AlternativeSecurityId               | Alternatiivne Turvalisus ID objekti.
CreationType                        |(Kas kutse või viiruse) kasutaja loomise meetod.
InviteTicket                        |Kutse piletid kasutaja loend.
InviteReplyUrl                      |Kutse nõustumisel vasta URL-ide loendit.
InviteResources                     |Ressursse, mis on kutsutud kasutaja loend.
LastDirSyncTime                     |Viimast objekti värskendamist tõttu sünkroonimise funktsiooni autoriteetsete directory (klient, kohapealne).
MSExchRemoteRecipientType           |Kaartide MSO adressaatide tüüpi. Viidata [MSO adressaadi tüübid] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx adressaatide jaoks
PreferredDataLocation               |Eelistatud kasutaja, rühma, kontakti, avaliku kausta asukoht või seadme andmeid.
ProxyAddresses                      |Aadress, mille alusel tuvastatakse Exchange Serveri adressaatide objekti välis-posti süsteemis.
StsRefreshTokensValidFrom           |Viib värskendada Turbeloa tühistamise teave – mis tahes STS värskendamise sõned enne seekord peetakse aegunud.
UserPrincipalName                   |Mis on Interneti-laadis sisselogimisnimi, kasutaja UPN.
UserState                           |Kasutaja ootab kinnitamist/PendingAcceptance/aktsepteeritud/PendingVerification olek.
UserStateChangedOn                  |Viimase muudatuse UserState ajatempli. Kasutatakse elutsükli töövood.
UserType                            |Kasutaja tüüp. Liikme (0), külaline (1) Viral(2).

## <a name="update-group-attributes"></a>"Värskenduste rühma" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Liigitamine            | Liigitamine ühendatud rühma (HBI, MBI jne).
Kirjeldus               | Inimeste loetav kirjeldavate lausete objekti kohta.  
DisplayName               |Objekti kuvatav nimi
DirSyncEnabled            |Näitab, kas sünkroonimise kaudu mõne autoriteetsete directory (klient, kohapealne).
GroupLicenseAssignment    |Litsentsi määramine rühmale
GroupType                 |Tippige rühma ühendatud (0)
IsMembershipRuleLocked    |Näitab, et MembershipRule atribuut on seatud rühma iseteenindusliku halduse teenus ja kasutajad ei saaks muuta. Kehtivad ainult rühmad, kus GroupType sisaldab GroupType.DynamicMembership
IsPublic                  |Lipp, mis näitab, kui rühm on avalik/era.
LastDirSyncTime           |Viimast objekti värskendamist tulemusena sünkroonimise funktsiooni autoriteetsete (klient eeldusel) kataloogi.
E-posti                      |Esmane meiliaadress
MailEnabled               |Näitab, kas see rühm sisaldab e-posti võimalus.
MailNickname              |Objekti address book hüüdnimi, tavaliselt see osa oma e-posti nime eelmise funktsiooni "@" sümbol.   
MembershipRule            |Kriteeriumide alusel rühma iseteenindusliku halduse teenuse selle rühma peaksid kuuluma liikmete määratlemiseks väljendada string. Vt ka IsMembershipRuleLocked. Kehtivad ainult rühmad, kus GroupType sisaldab GroupType.DynamicMembership.
MembershipRuleProcessingState| Loetelu väärtus määratletud rühma iseteenindusliku halduse teenus töötlemine selle rühma liikmeks oleku määratlemine. Kehtivad ainult rühmad, kus GroupType sisaldab GroupType.DynamicMembership.
ProxyAddresses            |Aadress, mille alusel tuvastatakse Exchange Serveri adressaatide objekti välis-posti süsteemis.
RenewedDateTime           |Kui rühma viimati uuendatud ajatempli kirje.   
SecurityEnabled           |Näitab, kas rühma liikmed võivad mõjutada autoriseerimine otsuseid.
WellKnownObject           |Märgib directory objekti, mis tähistavad selle ühe eelmääratletud kogum.

## <a name="update-device-attributes"></a>"Uuendada seadme" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Näitab, kas turvalisus subjekt saab autentida.
CloudAccountEnabled | Näitab, kas saate autentida turvalisus subjekt. Kui seade on õppinud eeldusel, kirjutatud InTune.
CloudDeviceOSType | Seadme tüüp põhjal OS näiteks Windows RT iOS-i. Kui määratud mõnda pilveteenusesse (nt Intune), see atribuut muutub autoriteetsete kataloogis DeviceOSType.
CloudDeviceOSVersion | OS versioon. Kui määratud mõnda pilveteenusesse (nt Intune), see atribuut muutub autoriteetsete kataloogis DeviceOSVersion.
CloudDisplayName | DisplayName LDAP atribuudi väärtust. Kui määratud mõnda pilveteenusesse (nt Intune), see atribuut muutub autoriteetsete kataloogis displayName jaoks.
CloudCreated |Näitab, kas objekti lõi pilveteenustega.
CompliantUntil | Mis kellaajal kuni seadme loetakse nõuetele.
DeviceMetadata |Kohandatud metaandmete seadme jaoks
DeviceObjectVersion | See atribuuti kasutatakse skeemi versiooni seadme tuvastamiseks.
DeviceOSType |Seadme tüüp põhjal OS näiteks Windows RT iOS-i. Teenus kirjutanud ja mõeldud on MDM, halduse teenuse või STS lihtsustatud haldamise teenust.
DeviceOSVersion |OS versioon. Teenus kirjutanud ja mõeldud on MDM, halduse teenuse või STS lihtsustatud haldamise teenust.
DevicePhysicalIds |Mitme väärtusega atribuut mõeldud seadme identifikaatorid talletamiseks. See võib hõlmata BIOS ID-d, TPM thumbprints, riistvara teatud ID-d, jne.
DirSyncEnabled | Näitab, kas olete autoriteetsete (klient, eeldusel) kaudu ilmneb sünkroonimise kataloogi.
DisplayName |Objekti kuvatav nimi
IsCompliant | Selle atribuudi abil saate hallata seadme mobiilsideseadme halduse olek.
IsManaged |See atribuut kasutatakse seadme haldab pilv MDM
LastDirSyncTime |Viimast objekti värskendamist tõttu sünkroonimise funktsiooni autoriteetsete (klient eeldusel) kataloogi.

## <a name="update-device-configuration-attributes"></a>"Uuendada seadme konfiguratsiooni" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Eemaldamist käsitletakse seade võib olla passiivne enne selle päevade arvu ülempiir.
RegistrationQuota   |Poliitika kasutatud seadme registreerimine ühe kasutaja jaoks lubatud arv piirata.

## <a name="update-service-principal-configuration-attributes"></a>"Update teenuse põhisumma konfigureerimine" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Näitab, kas turvalisus subjekt saab autentida.
AppPrincipalId | Väliste, rakenduse määratletud identiteedi turvalisus subjekt.
DisplayName |Objekti kuvatav nimi
Andke    | Teenuse põhilist nime, mis sisaldab "nimi/asutus", kus nime saate määrata rakenduse klassi väärtuse ja asutuse sisaldab vähemalt hostname [: port] või "nimi", mis täpsustab identifikaatori jaoks teenuse põhilise.

## <a name="update-app-attributes"></a>"Värskendada rakenduse" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Aadresside (ümber suunata URL-id) teenuse põhilise määratud kogum.
AppId                               |Rakenduse rakenduste ID-d   
AppIdentifierUri | Rakenduse URI, mis tuvastab rakendus.  See on tavaliselt rakenduse access URL.
AppLogoUrl |Talletatud CDN-ID rakenduse logo pildi URL-i.
AvailableToOtherTenants | True rakendus on mitme rentniku rakendus (nt saab kasutada muude rentnike).
DisplayName | Rakenduse nimi kuvatav nimi
Õigus |Teenuserakenduse õiguste loend.
ExternalUserAccountDelegationsAllowed |Tähis, mis näitab, kas ressursi rakenduse on usaldusväärne ja delegeerimine kirjete väliste kasutajakontode loomiseks.
GroupMembershipClaims |Rühma liikmete nõuete poliitika.
PublicClient | Väärtus TRUE, kui klient ei saa hoida salajane (nt konfidentsiaalne klient OAuth2.0)
RecordConsentConditions | Tüüpi nõusoleku tingimused, nagu on määratletud lepingu tingimustega: pole (0) SilentConsentForPartnerManagedApp(1). See väärtus puutuvad Graph API skeemi ja saab ainult Sea/muuta rentnikuadministraatorid.
RequiredResourceAccess |XML-sisu RequiredResourceAccess atribuudi väärtuse.
Web Appis |Kui väärtus on true, näitab, et see rakendus on web appi.
WwwHomepage |Esmane veebilehele.

## <a name="update-role-attributes"></a>"Värskendada rolli" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Aadresside (ümber suunata URL-id) teenuse põhilise määratud kogum.
BelongsToFirstLoginObjectSet |Kui väärtus on true, näitab, et see objekt kuulub objektide vajalikud login uue rentniku esimene administraator.
Sisseehitatud |Näitab, kas objekti eluiga kuulub süsteem.
Kirjeldus | Inimeste loetav kirjeldavate lausete objekti kohta.  
DisplayName |Objekti kuvatav nimi
MailNickname | Objekti address book hüüdnimi, tavaliselt see osa oma e-posti nime eelmise funktsiooni "@" sümbol.
RoleDisabled |Näitab, kas roll ignoreerida juurdepääsu kontrollimiseks.
RoleTemplateId | Identiteedi rolli malli.
ServiceInfo |Teenuse ettevalmistamise teavet võib tarbitud MOAC ja/või muude teenuste versioonide (samas või mõnes muus teenuse tüüpi).
TaskSetScopeReference |Tuvastab on TaskSet ja seotud rolli või RoleTemplate otsinguulatuste kogumist.
ValidationError |Ühendatud teenus, mis kirjeldab-mööduv, teenuse-spetsiifiliste viga, atribuudid või toimingu objekti administraatori lahendamiseks link avaldatud teave.
WellKnownObject |Märgib directory objekti, mis tähistavad selle ühe eelmääratletud kogum.

## <a name="update-role-definition-attributes"></a>"Roll definitsiooni värskendamiseks" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Luba otsinguulatuste viidatakse selle RoleDefinition määramisel turvalisus subjekt kogum.
DisplayName     |Objekti kuvatav nimi
GrantedPermissions  |Selles RoleDefinition antud õigused.


## <a name="update-administrative-unit-attributes"></a>"Värskendada haldus ühiku" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Kirjeldus |   Selle atribuudi värskendatakse, kui muudate haldus üksuse kirjeldus.
DisplayName |   Kui muudate haldus üksuse nime, värskendatakse selle atribuudi.

## <a name="update-company-attributes"></a>"Update ettevõtte" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Asukoht, kus ettevõtte kasutajad tohivad ette valmistada.
AuthorizedServiceInstance   | Nimed, millele lepingu kasutusele võtta eksemplare.
DirSyncEnabled |Näitab, kas olete autoriteetsete (klient, eeldusel) kaudu ilmneb sünkroonimise kataloogi.
DirSyncStatus |Näitab, kas olete autoriteetsete (klient, eeldusel) kaudu ilmneb sünkroonimine address book objekte rentniku kontekstis directory; Ettevõtte objektide atribuudi DirSyncEnabled laiendamine.
DirSyncFeatures |Bitise lipp silma peal rentniku jaoks lubatud ja keelatud dirsync funktsioonide komplekt.
DirectoryFeatures |Lubatud või keelatud directory funktsioonid.
DirSyncConfiguration |Sisaldab kõiki DirSync konfiguratsiooni praeguse rentniku kohase.
DisplayName |Objekti kuvatav nimi
IsMnc |Kahendmuutujaga lipu seatud "true" iff ettevõtte on lubatud rahvusvaheline ettevõte funktsiooni.
ObjectSettings | Kogum sätted kehtivad objekti ulatust.
PartnerCommerceUrl |Partneri äri saidi URL-i.
PartnerHelpUrl |Partneri abi saidi URL-i.
PartnerSupportEmail | URL-i e-posti partneri tugi.
PartnerSupportTelephone |Partneri klienditoe telefonil URL-i.
PartnerSupportUrl |Partneri tugi saidi URL-i.
StrongAuthenticationDetails |StrongAuthentication seotud andmed.
StrongAuthenticationPolicy |Ettevõtte poliitika tugev autentimine.
TechnicalNotificationMail |E-posti aadress teatada ettevõtte tehnilise küsimustes.
TelephoneNumber |Telefoninumbrid, mis vastavad ITU soovitus E.123.
TenantType |Rentniku tüüp. Kui see väärtus pole määratud, on rentniku ettevõte. Muul juhul võimalikud väärtused on: MicrosoftSupport (0), SyndicatePartner (1), (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) (4) ResellerPartnerDelegatedAdmin ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |DNS-i domeeninimedega seotud ettevõtte kogum.

## <a name="update-domain-attributes"></a>"Update Domain" atribuudid
Atribuut                           | Kirjeldus
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Võimalused    |Bit võimaluste domeeninime, mis kirjeldab lipud, kui mõni.
Vaikimisi |Näitab, kas domeen on vaikeväärtus; Näiteks vaikimisi UserPrincipalName järelliide kui administraator loob uue kasutaja MOAC.
Algne |Näitab, kas domeen on algseks domeeniks ettevõttele, nagu OCP eraldatud. Algne domeen on kordumatu alamdomeeni Microsoft Online'i domeeni; e.g.contoso3.microsoftonline.com.
LiveType    |Vastavate Windows Live'i nimeruumi, kui mis tahes tüüpi.
Nimi    | Lõpp-punkti ID.
PasswordNotificationWindowDays  |Teavitatakse enne parooli aegumise teatise saamist kasutaja päevade arv.
PasswordValidityPeriodDays  | Muuta parooli on hea enne, kui see päevade arv.

Auditilogi kirjed on nõutavad juhtelemendi palju nõuetele vastavus määruste. Klientidele, kes kasutavad Azure Active Directory auditilogi aruande täita saavad nõuetele vastavus määruste, on soovitatav, et kliendi esitada koopia spikriteema koopia kliendi eksporditud auditilogi aruande selgitada aruande üksikasjad. Kui audiitor mõista vastavuse määrus, mis praegu vastab Azure, otse audiitor [vastavuse lehe](https://azure.microsoft.com/support/trust-center/compliance/) Microsoft Azure'i Trust Center.
