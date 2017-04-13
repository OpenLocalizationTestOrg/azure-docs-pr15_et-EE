<properties
    pageTitle="Azure AD õigustega juurdepääsu turvamiseks | Microsoft Azure'i"
    description="Teema, mis selgitab lähenemisel õigustega juurdepääsu Azure, Azure Active Directory ja teenuse Microsoft Online Services kõikidel turvamiseks."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Turvamise õigustega juurdepääsu Azure AD

Õigustega juurdepääsu turvamiseks on kriitiline esimene samm kaasaegse ettevõtte ettevõtte vara kaitsmiseks. Õigustega kontod on need, mis ja IT-süsteemide haldamiseks. Küberründajate suunata nende kontodega juurde pääseda ettevõtte andmed ja süsteemid. Õigustega juurdepääsu tagamiseks peaksite eristada, kontode ja süsteemide risk pahatahtlik kasutaja puutuda.

Kasutajaid on hakanud õigustega juurdepääs pilveteenustega kaudu. See võib hõlmata Office365 globaalse administraatorid, Azure tellimuse administraatoritele ja kasutajatele, kellel on juurdepääs haldus VMs või SaaS rakendused.

Microsoft soovitab jälgite seda teejuht [turvaliseks õigustega](https://technet.microsoft.com/library/mt631194.aspx)juurdepääsu.

Klientidele, kes kasutavad Azure Active Directory juurdepääsu Azure, Office 365, või muude Microsofti teenuste ja rakenduste haldamiseks, need põhimõtted kehtivad kasutajakontode haldavad ja Active Directory või Azure Active Directory autenditud. Järgmistes jaotistes Lisateavet Azure AD funktsioonide turvaliseks õigustega juurdepääsu toetamiseks.

## <a name="multi-factor-authentication"></a>Mitmikautentimise

Administraatori autentimise turvalisus suurendamiseks tuleks nõuda mitmikautentimise (MFA), õiguste andmist. MFA on meetodi kontrollida, kes te olete, mida tuleb kasutada rohkem kui lihtsalt kasutajanime ja parooliga. Teise kasutaja sisselogimist ja tehingud turvalisus kiht pakub.

Azure'i Mitmikautentimise aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See pakub tugeva autentimise kaudu lihtne kinnitamise suvandid, sh telefonikõnede, tekstsõnumeid, mobiilirakenduse teavitusi või kinnituskood ja 3 tootja VANDE sõned vahemikuks.

Ülevaate sellest, kuidas Azure'i Mitmikautentimise leiate järgmisest videost.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Lisateavet leiate teemast [MFA jaoks Office 365 ja Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Aja seotud õigusi

Mõni ettevõte võite avastada, et neil on liiga palju kasutajaid suuremate õigustega rollid. Kasutaja lisatud rolli teatud tegevuste, nt teenuse kasutajaks, kuid ei kasuta sageli hiljem need õigused.

Vähendage õigusi käes aega ja nende kasutamist oma nähtavust suurendada, piirata kasutajate ainult võttes nende õiguste lihtsalt aega (JIT) vajadusel toimingu sooritamiseks. Azure Active Directory ja teenuse Microsoft Online Services, saate kasutada [Azure AD õigustega identiteetide haldus (PIM)](http://aka.ms/AzurePIM).


![PIM armatuurlaud][2]


## <a name="attack-detection"></a>Rünnak automaattuvastus

[Azure Active Directory identiteedi kaitse](../active-directory-identityprotection.md) pakub risk sündmused ja võimalike nõrkade kohtade mõjutamata ettevõtte identiteedid konsolideeritud vaadet. Identiteedi kaitse põhjal risk sündmusi, arvutab kasutaja risk tase iga kasutaja, mistõttu saate konfigureerida riski põhjal kaitsmiseks poliitikaid rakendada automaatselt teie ettevõtte identiteedid. Need poliitikad koos muude juurdepääsu juhtelementide Azure Active Directory ja EMS, saab automaatselt blokeerida kasutaja või pakub soovitusi, mis sisaldavad parooli lähtestamise ja mitmikautentimise täitmist.

![Azure'i AD identiteedi kaitse][3]

## <a name="conditional-access"></a>Tingimusvormingu juurdepääs

Tingimusvormingu juurdepääsu kontroll, kontrollib Azure Active Directory valida autentimist kasutaja, enne kui lubate juurdepääsu rakenduse eritingimused. Kui nendest tingimustest on täidetud, autenditud kasutaja ja rakenduse juurdepääs lubatud.


![Sätte tingimusvormingu juurdepääsureeglid MFA][4]


## <a name="related-articles"></a>Seotud artiklid

- [Azure'i Mitmikautentimise](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) lubamine
- [Azure'i AD õigustega identiteedi haldamise](../active-directory-privileged-identity-management-configure.md) lubamine
- [Azure'i AD identiteedi kaitse](../active-directory-identityprotection.md) lubamine
- [Juhtelementide juurdepääsu](../active-directory-conditional-access.md) lubamine


Täieliku turvalisuse teejuht koostamise kohta leiate lisateavet jaotisest "Kliendi kohustused ja teejuht" [Microsoft Cloud Security Enterprise arhitektidele](http://aka.ms/securecustomer) dokumendi. Lisateavet Microsofti teenuste abistamine mõnda neist teemadest, võtke ühendust oma Microsofti esindajaga või külastage meie [küberturbe lahenduste lehe](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)kaasamine.

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
