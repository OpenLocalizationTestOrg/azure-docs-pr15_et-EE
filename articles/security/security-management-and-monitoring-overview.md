<properties
   pageTitle="Azure'i turvalisuse haldamise ja järelevalve ülevaade | Microsoft Azure'i"
   description=" Azure'i pakub turvalisus võimaldada haldamise ja jälgimine ja pilveteenustega Azure'i virtuaalmasinates abi.  Selles artiklis antakse ülevaade nende core funktsioonid ja teenused. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Azure'i turvalisuse haldamise ja järelevalve ülevaade

Azure'i pakub turvalisus võimaldada haldamise ja jälgimine ja pilveteenustega Azure'i virtuaalmasinates abi. Selles artiklis antakse ülevaade nende core funktsioonid ja teenused. Pakutakse linke artiklitele, mis annab iga andmeid, nii et saate lisateavet.

Oma Microsofti pilveteenustega turvalisus on partnerlus ja teie ja Microsofti vahelise. Jagatud vastutuse tähendab, et Microsoft vastutab Microsoft Azure'i ja füüsiline turvalisus andmete andmekeskuste (Turvalisus kaitstud nagu lukustatud märgi kirje uksed, aiad ja piirded) abil. Lisaks Azure pakub tugev cloud security tarkvara kihis rangete klientide turvalisus, privaatsus ja vastavus vajadustele vastavat taset.

Saate oma andmete ja identiteedid, eest kaitsmise neile oma kohapealse ressursse turvalisus ja pilveteenuse komponendid, mille on teie kontrolli turvalisus. Microsoft annab teile turbemeetmed ja võimalusi aitab kaitsta teie andmeid ja rakendused. Teie eest turvalisus põhineb pilveteenuses tüüp.

Järgmises tabelis on kokkuvõte kohustusi kliendi ja Microsoft saldo.

![Jagatud vastutuse][1]

Süvitsi sukelduvad turvalisus haldus, leiate [Azure'i turvalisus haldus](azure-security-management.md).

Siit leiate selle artikli alla põhilisi funktsioone:

- Rollipõhine juurdepääsu reguleerimine
- Ründevaratõrje
- Mitmikautentimise
- ExpressRoute
- Virtuaalse võrgu lüüsid
- Õigustega identiteetide haldus
- Identiteedi kaitse
- Turbekeskus

## <a name="role-based-access-control"></a>Rollipõhine juurdepääsu reguleerimine

Rollipõhine juurdepääsu juhtimine (RBAC) leiate Azure'i ressursid kohandatud juurdepääsu haldamine. RBAC kasutamisel saate anda inimesed ainult summa juurdepääsu, mida tuleb teha oma tööd.  RBAC aitab teil tagada, et kui asutusest lahkub nad kaotaks juurdepääsu pilveteenuses ressursse.

Lisateave:

- [Active Directory meeskonna ajaveebi RBAC kohta](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure'i Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Ründevaratõrje

Azure'i abil saate olulised turvalisuse müüjad, nagu Microsoft, Symantec, Trend Micro, McAfee ja Kaspersky ründevaratõrje tarkvara kaitsta oma virtuaalmasinates pahatahtlike faile, nuhkvara ja muu ohud.

Microsoft Antimalware pakub võimalust installida nii PaaS rollid ja virtuaalmasinates agent ründevaratõrje. Põhineb System Center Endpoint Protection, funktsioon toob tõestatud kohapealse infotehnoloogia pilveteenusesse.

Pakume sügav integreerimine trendi 's [Sügav turvalisus](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ ja [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ toodete Azure'i platvormi. DeepSecurity viirusetõrje lahenduse ja SecureCloud on lahenduse krüptimine. DeepSecurity juurutatakse VMs abil laiend mudelit, mis sees. Portaali Kasutajaliidese ja PowerShelli abil saate kasutada uue VMs, mis on on kedratud üles või olemasoleva VMs, mis on juba juurutanud DeepSecurity.

Azure'i toetatakse ka Symantec lõpp-punkti kaitse (okt). Portaali kaudu, kliendid on võimalus määrata, et nad kavatsete kasutada okt VM sees. Eraldaja saab installida uus VM Azure portaali kaudu või saab installida mõne olemasoleva VM PowerShelli kaudu.

Lisateave:

- [Ründevaratõrje lahendusi Azure'i Virtuaalmasinates juurutamine](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsofti ründevaratõrje Azure pilveteenustega ja Virtuaalmasinates](../security/azure-security-antimalware.md)
- [Kuidas installida ja konfigureerida teenust Windows VM Trend Micro sügav turvalisus](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Kuidas installida ja konfigureerida Windows VM Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Uue ründevaratõrje suvandite Azure'i Virtuaalmasinates – McAfee Endpoint Protection kaitsmine](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Mitmikautentimise

Azure'i mitmikautentimise (MFA) on ehtsuse, mis on vaja kasutada rohkem kui üks kinnitusmeetod ja lisab kriitilised teine kiht turvalisus kasutaja sisselogimist ja tehingud. MFA aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See pakub tugev autentimist kinnitamine valikuvõimalusi kaudu – telefonikõne, tekstsõnumi või mobiilirakenduses teatist või kinnitamine koodi ja 3. tootja VANDE sõned.

Lisateave:

- [Mitmikautentimise](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mis on Azure Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md)
- [Azure'i Mitmikautentimise tööpõhimõte](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure'i ExpressRoute võimaldab laiendada oma kohapealse võrgu Microsofti pilveteenuse hõlbustav ühenduvuse pakkuja sihtotstarbeline privaatne ühenduse kaudu. ExpressRoute, kus saate luua ühenduse Microsofti pilveteenustega, nt Microsoft Azure'i, Office 365 ja CRM Online'i. Ühenduvus võib olla ka kõik mis (IP VPN) võrgu, kakspunkt Ethernet võrgus või virtuaalse rist-ühenduse kaudu ühenduvus pakkuja saanud poole. Avaliku Interneti kaudu ei lähe ExpressRoute ühendused. See võimaldab ExpressRoute ühendused pakkuda töökindlust, kiirem kiirust, lower latentsused ja suurem turvalisus, kui tüüpilised ühendused Interneti kaudu.

Lisateave:

- [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuaalse võrgu lüüsid

VPN lüüsid, nimetatakse ka Azure virtuaalse võrgu lüüsid, kasutatakse saata võrguliiklust vahel virtuaalne võrkude ja kohapealse asukohad. Neid kasutatakse ka saata mitme virtuaalse võrgu Azure'is (VNet VNet) vahel.  VPN lüüside pakuvad turvalist asutusesiseses Ühenduvus Azure ja oma taristu.

Lisateave:

- [VPN lüüside kohta](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure'i võrgu turvalisuse ülevaade](security-network-overview.md)

## <a name="privileged-identity-management"></a>Õigustega identiteetide haldus

Mõnikord kasutajad peavad Azure ressursse või muudes rakendustes SaaS õigustega toiminguid. See tähendab sageli organisatsioonidel on anda neile pidev õigustega juurdepääs Azure Active Directory (Azure AD). See on pilve majutatud ressursid kasvab ohtu turvalisusele, sest ettevõtted ei saa piisavalt jälgida, mida kasutajad oma õigustega juurdepääsu teed.
Lisaks, kui kasutajakonto õigustega juurdepääsu on rikutud, ühe rikkumise võivad mõjutada üldine cloud turvalisuse. Azure'i AD õigustega identiteetide haldus aitab lahendada see risk langetamine õigusi käes aega ja suurendades nähtavus kasutus.  

Õigustega identiteetide haldamisviis tutvustatakse mõistet ajutine administraatori rolli või "samal ajal" administraatori juurdepääsuõiguste, mis on kasutaja, kes peab lõpule viima aktiveerimine jaoks määratud roll. Aktiveerimisprotsessi muudab ülesande kasutaja rollid Azure AD passiivsed aktiivne määratud aja jooksul periood, nt 1 tund.

Lisateave:

- [Azure'i AD õigustega identiteetide haldus](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Azure'i AD õigustega identiteedi haldamine kasutamise alustamine](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identiteedi kaitse

Azure Active Directory (AD) kaitse pakub kahtlaste tegevuste ja võimalike nõrkade kohtade kaitsta teie ettevõtte konsolideeritud vaadet. Identiteedi kaitse tuvastab kahtlaste tegevuste kasutajatele ja õigustega (haldus) identiteedid, põhineb signaale brute-Jõusta eest, nt lekkinud identimisteabe ja sisselogimist võõras asukohtadest ja nakatunud seadmed.

Teatised ja Soovitatavad parandamise pakkudes identiteedi kaitse aitab reaalajas riske leevendada. Arvutab kasutaja risk raskusaste ja risk poliitikat kaitse rakenduse pääseda tulevaste ohtude automaatselt abil saate konfigureerida.

Lisateave:

- [Azure Active Directory identiteedi kaitse](../active-directory/active-directory-identityprotection.md)
- [Kanali 9: Azure'i AD ja identiteedi Kuva: identiteedi kaitse eelvaade](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Turbekeskus

Azure'i turbekeskus aitab vältida, avastada ja ohtude vastamine ja pakub saate suurendada nähtavus, ja juhtida, oma Azure ressursse turvalisus. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Turbekeskus aitab teil optimeerida ja jälgida oma Azure ressursside turvalisus:

- Võimaldab määratleda poliitikad teie Azure tellimuse ressursid vastavalt oma ettevõtte turvalisus vajadustele ja rakenduste tüübi või andmete tundlikkus iga tellimuse.
- Oma Azure'i virtuaalmasinates seisundi kontrollimine, võrgunduse ja rakendused.
- Esitada tähtsuse Turbeteatiste, sh integreeritud partnerilahenduste koos kiiresti uurimiseks vajalikku teavet ja migreerimisel ning probleemide lahendamisel rünnaku kohta teatiste loendit.

Lisateave:

- [Azure'i turbekeskus tutvustus](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
