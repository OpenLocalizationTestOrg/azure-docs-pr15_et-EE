<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine – saate määratleda andmete kaitsmise strateegia | Microsoft Azure'i"
    description="Saate määratleda andmete kaitse strateegia lahendusse hübriid identiteedi määratletud äri nõuetele."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Oma hübriid identiteedi lahenduse andmete strateegia määratlemine

Selles ülesandes määratlete andmete kaitse strateegia hübriid identiteedi lahendusse äri nõuetele määratud:

- [Andmekaitse nõudeid määratlemine](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Sisuhaldus nõuete määratlemine](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Juurdepääsu kontrollimise nõuete määratlemine](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Langeva vastuse nõuete määratlemine](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Andmete kaitsmise suvandite määratlemine
Nagu on selgitatud [määratlemine directory sünkroonimise nõuetele](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), asub Microsoft Azure AD saab sünkroonida oma Active Directory domeeniteenused (AD DS) kohapealse. Saavad ettevõtted võimendada Azure AD kinnitamiseks kasutaja mandaat, kui nad üritavad juurdepääs ettevõtte ressursid. Seda saab teha nii stsenaariumid: andmete kohapealse ülejäänud ja pilveteenuses.  Juurdepääs andmetele Azure AD nõuab kasutaja autentimise turvalisus Turbeloa teenuse (STS) kaudu.

Pärast autentimist, autentimine luba ja tiražeeritud partition loetakse kasutaja turvasubjektinimi (UPN) ja vastav kasutaja domeeni container määratakse. Teave kasutaja olemasolu, lubatud oleku ja rolli kasutatakse autoriseerimine süsteemi kindlaks teha, kas sihtrentnikusse nõutud juurdepääs on lubatud selle kasutaja selle seansi. Teatud volitatud toimingute (täpsemalt luua kasutaja parooli lähtestamine) auditilogi rada, mida saab kasutada rentnikuadministraator hallata vastavuse püüete või juurdluste loomine.

Jooksva andmeid oma kohapealse andmekeskuse üheks Azure Storage Interneti-ühenduse kaudu ei pruugi alati olla andmete maht, võrguläbilaskevõime saadavust või muude kasutuse tõttu. [Azure'i salvestusteenus impordi/ekspordi](../storage/storage-import-export-service.md) pakub riistvara põhist suvand pannes/allalaadimise suuri andmemahtusid bloobimälu. See võimaldab saata [BitLockeri krüptitud](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) kõvaketta draivid otse on Azure andmekeskuse kui cloud tehtemärgid üles laadida sisu salvestusruumi konto või nad saavad alla laadida Azure andmete juurde naasmiseks saate oma draivid. Ainult krüptitud ketast aktsepteeritakse see protsess (loodud teenuse töö häälestamise ajal klahvi BitLockeri abil). BitLockeri võti on esitatud Azure eraldi, tagades seega riba võtme ühiskasutusest.

Kuna töödeldavate andmete võib toimuda erinevates stsenaariumides, on oluline teada, et Microsoft Azure'i kasutab [virtuaalse võrgunduse](https://azure.microsoft.com/documentation/services/virtual-network/) eristamiseks rentnikud liikluse üksteisest, kasutades näiteks host ja Külastajate taseme tulemüürid, IP-paketi filtreerimine, port blokeerimine ja HTTPS-i lõpp-punktid. Enamik Azure sisemine suhtlus, sh taristu-taristu ja taristu klientide (kohapealse), on ka krüptitud. Mõne muu oluline stsenaarium sobib suhtlus sees Azure andmekeskuste; Microsoft haldab võrkude kinnitamaks, et pole VM saate jäljendada või pealt teise IP-aadress. SSL/TLS-i kasutatakse kasutamisel Azure Storage või SQL andmebaase, või kui pilveteenustega. Sel juhul administraator Klient vastutab saamise SSL-sert ja juurutamine selle oma rentniku taristu. Andmete liikluse teisaldamine sama juurutuse Virtuaalmasinates või vahel rentnikud ühe juurutamine Microsoft Azure virtuaalse võrgu kaudu saate kaitstud krüptitud protokollide nagu HTTPS, SSL/TLS või teised kaudu.

Sõltuvalt sellest, kuidas saate vastused küsimustele [määratlemine andmekaitse nõudeid](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), tuleks kindlaks teha, kui soovite andmete kaitsmine ja kuidas hübriid identiteedi lahendus aitab teil selle. Tabelis ei toeta Azure võimalusi, mis on saadaval iga andmete kaitse stsenaariumi puhul.


| Andmete kaitsmise suvandite         | Veebisaidil ülejäänud pilveteenuses | Kohapealse ülejäänud veebisaidil | Teel |
|---------------------------------|----------------------|---------------------|------------|
| BitLockeri krüptimise      | X                    | X                   |            |
| SQL serveri andmebaasi krüptimiseks | X                    | X                   |            |
| VM-VM krüptimine             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
Lugege [vastavuse funktsioon](https://azure.microsoft.com/support/trust-center/services/) [Microsoft Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/) rohkem teada kinnitamine, mida iga Azure'i teenus on nõuetele vastavust.
Kuna mitmekihiliste lähenemisviisi andmekaitse suvandite kasutamiseks võrdlus need suvandid ei ole kohaldatavad selle ülesande jaoks. Veenduge, et teil on iga riigi, et andmed on saadaval kõigi suvandite kasutamine.

## <a name="define-content-management-options"></a>Sisuhaldus suvandite määratlemine
Üks eeliseid hübriid identiteedi taristu haldamise Azure AD abil on protsessi läbipaistvad lõppkasutaja seisukohalt. Kasutaja proovib juurde pääseda Ühiskasutusega ressursi, ressurss nõuab autentimist, on kasutaja autentimise taotluse saatmiseks Azure AD, et saada luba ja juurdepääsu ressursile. Selle protsessi toimub taustal, ilma kasutaja suhtlus. Samuti on võimalik anda õiguse kasutajate [rühma](active-directory-manage-groups.md#getting-started-with-access-management) , et nad saaksid teatud levinud toiminguid.

Ettevõtted, mis on tavaliselt andmete privaatsust muret nõuda andmete liigitamine nende lahendus. Kui nende praegune kohapealse taristu juba kasutab andmete liigitamine, on võimalik kasutada Azure AD nimega peamise hoidla kasutaja identiteedi jaoks. Levinud tööriist, et see on kasutatud kohapealse andmete liigitamine nimetatakse [Andmete liigitamine tööriistakomplekt](https://msdn.microsoft.com/library/Hh204743.aspx) Windows Server 2012 R2. See tööriist aitab tuvastada, liigitada ja kaitsta andmeid oma isiklike pilveteenuses faili serverites. Samuti on võimalik kasutada [Automaatse faili liigitamine](https://technet.microsoft.com/library/hh831672.aspx) selleks Windows Server 2012.

Kui teie asutus ei sisalda andmeid liigitamine kohas, kuid peab kaitsta tundlikku faile lisamata uue serverid kohapealse, saavad nad Microsoft [Azure'i õiguste halduse teenust](https://technet.microsoft.com/library/JJ585026.aspx)kasutada.  Azure RMS-i kasutab krüptimist, identiteedi ja luba poliitika turvalisemaks faile ja e-posti ja see toimib mitmes seadmes – telefonide, tahvelarvutite ja arvutites. Kuna Azure RMS-i on mõnes pilveteenuses, ei ole vaja otseselt konfigureerida usalduste koos teiste asutuste enne kaitstud sisu saate neid anda ühiskasutusse. Kui nad on juba teenusekomplekti Office 365 või mõne Azure AD kataloogist, koostöö üle ettevõtted automaatselt toetatud. Saate sünkroonida ka ainult directory atribuute, mida Azure RMS-i peab toetama ühise identiteedi jaoks kontode kohapealse Active Directory Azure Active Directory sünkroonimisteenused (AAD Sync) või Azure'i AD-ühenduse abil.

Sisuhaldus oluline osa on aru saada, kellel on juurdepääs milline ressurss, seetõttu rikkaliku logimise võimalus on oluline identiteedi haldamine lahendus. Azure AD pakub log üle 30 päeva, sh:

- Rollirühma liikmete muudatuste (ex: lisatakse üldadministraatori rolliga kasutaja)
- Värskenduste mandaat (ex: parooli muutmise)
- Domeenide haldamine (ex: kinnitatava domeeni, domeeni eemaldamine)
- Lisada või eemaldada rakendusi
- Kasutajate haldamine (ex: lisamine, eemaldamine, kasutaja värskendamine)
- Lisada või eemaldada litsentse

>[AZURE.NOTE]
Lugege [Microsoft Azure'i turbe- ja auditilogide haldamine](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) Azure logimine võimaluste kohta rohkem teada.
Sõltuvalt sellest, kuidas saate vastused küsimustele [määratlemine sisu haldamise nõuetele](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), peaks oskama otsustada, kuidas soovite sisu tuleb hallata oma hübriid identiteedi lahendus. Kõik suvandid, mis on esitatud tabelis 6 on võimalik integreerimine Azure AD, on oluline määratleda, mis on teie ettevõtte vajadustele kõige sobivam.

| Sisuhaldus suvandid                                                               | Eelised                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Puudused                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tsentraliseeritud kohapealse (Active Directory õiguste halduse Server)                      | Andmete eest vastutav serveri infrastruktuuri üle täielik kontroll <br> Sisseehitatud võimalus Windows Server, ei ole vaja täiendavat litsentsi või tellimusele <br> Saate integreeritud Azure AD hübriidjuurutuse stsenaarium <br> Toetab teabe teabeõiguste halduse (IRM) võimaluste Microsoft Online services, nt Exchange Online'i ja SharePoint Online'i, kui ka Office 365 <br> Toetab kohapealse Microsoft Serveri toodete, näiteks Exchange Serveri ja SharePoint Serveri faili serverid, kus käivitatakse Windows Server ja faili liigitamine taristu (FCI). | Kõrgem, hooldustööd (kursis värskendused, konfigureerimine ja võimalike täienduste), kuna see kuulub Server <br> Nõua kohapealse serveri taristu<br> Doesn'tleverage Azure võimaluste algupäraselt                                     |
| Tsentraliseeritud pilves (Azure RMS)                                                     | Haldamise hõlbustamiseks võrreldes kohapealse lahenduse <br> Saate integreeritud AD DS hübriidjuurutuse stsenaarium <br>  Täielikult integreeritud Azure AD <br> Serveri kohapealse juurutamiseks teenus pole nõutav. <br> Toetab kohapealse serveritoodete Microsoft Exchange Serveri, SharePointi, Server ja faili serverid, kus käivitatakse Windows Server ja faili liigitamine, taristu (FCI) <br> SEDA võib olla oma rentniku klahvi BYOK võimalusega üle täielik kontroll.                                                                                    | Ettevõtte peab olema pilve tellimus, mis toetab RMS-i <br> Ettevõtte peab olema Azure AD-kataloogist kasutaja autentimise RMS-i tugi                                                                                  |
| Hübriid (integreeritud Azure RMS-i, kohapealse Active Directory õiguste halduse Server) | Selle stsenaariumi liidetakse, tsentraliseeritud asutusesiseselt ja pilveteenuste eelised.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Ettevõtte peab olema pilve tellimus, mis toetab RMS-i <br> Ettevõtte peab olema kasutaja autentimise tugi RMS-i, on Azure AD kataloog <br> Nõuab Azure pilveteenuses vahelise ühenduse ja kohapealse taristu |

## <a name="define-access-control-options"></a>Määratlege suvandite juurdepääsu reguleerimine
Kasutamine autentimine, autoriseerimine ja juurdepääs kontrollida võimalusi Azure AD, siis saab luba oma ettevõtte kasutada keskse identiteedi hoidla kasutajate ja partnerite lubades kasutada ühekordse sisselogimise (SSO), nagu on näidatud alloleval joonisel:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Tsentraliseeritud ja täielikult muude kataloogide integreerimine

Azure Active Directory pakub ühekordse sisselogimise tuhandetele SaaS rakenduste ja kohapealse veebirakendusi. Lugege selle [Azure Active Directory federation ühilduvuse loend: kolmanda osapoole identiteedipakkujad, mida saab kasutada ühekordse sisselogimise rakendamiseks](https://msdn.microsoft.com/library/azure/jj679342.aspx) artiklis SSO kolmanda osapoole kohta lisateavet, mis olid testitud Microsoft. See funktsioon võimaldab ettevõttes kasutusele erinevaid B2B säilitades kontrolli identiteeti ja juurdepääsu haldamine. Siiski ajal B2B kujundamise protsess on oluline mõista autentimise meetodit, mis kasutab partneri ja kinnitage, kui seda meetodit toetavad Azure. Praegu on Azure AD poolt toetatud järgmised.

- Turvalisuse väide Markup Language (SAML)
- OAuthi
- Kerberose
- Märkide
- Serdid


>[AZURE.NOTE] Lugege [Azure Active Directory autentimine Protokollid](https://msdn.microsoft.com/library/azure/dn151124.aspx) leida iga protokoll ja Azure oma võimaluste kohta rohkem üksikasju.

Azure AD tugiteenuste kasutamisel mobiilsideseadmete ärirakenduste saate kasutada sama lihtne Mobile teenuste autentimine kogemus võimaldab töötajatel sisselogimiseks oma mobiilirakenduste koos oma ettevõtte Active Directory mandaat. Selle funktsiooni toetatakse Azure AD identiteedi pakkuja Mobile teenuste kõrval koos muude identiteedi pakkujate toetame juba (mis hõlmavad Microsofti Accounts, Facebooki ID, Google ID ja Twitteri ID). Kui kohapealse rakendused kasutab kasutaja mandaat, mis asub ettevõtte AD DS, partnerite ja varsti pilvest kasutajate juurdepääs peaks olema läbipaistev. Kasutaja tingimusvormingu juurdepääsu reguleerimine (pilvepõhist) veebirakenduste, veebi-API, Microsofti pilveteenustega, 3 SaaS rakenduste ja omakliendi (mobiil) rakenduste haldamine ja turvalisus, eelised on kontroll, aruandlus ühes kohas. See on soovitatav kinnitage see mitte tekitamiseks keskkonnas või on piiratud hulgal kasutajad.


>[AZURE.TIP] See on oluline märkida, et Azure AD ei ole rühmapoliitika AD DS-is on. Poliitika seadmete jaoks jõustamiseks peate mobiilsideseadme halduse lahenduse, nt [Microsoft Intune'i](https://technet.microsoft.com/library/jj676587.aspx).

Kui kasutaja on autenditud Azure AD abil, on oluline hindamaks, kui kasutaja on see pääsutaseme. Kasutaja on üle ressursi pääsutaseme võib olla muutuv, ajal Azure AD saate lisada mõne suurema turvalisuse tagamiseks kiht mõned ressursid juurdepääsu kontrollimine, peate Samuti pidage meeles, et ressursi ise võib olla ka oma pääsuloendi eraldi, nt juurdepääsu reguleerimine faili serveris asuvate failide jaoks. Alloleval joonisel on kokku võetud tasemeid võib olla hübriidjuurutuse stsenaariumi juurdepääsu reguleerimine.

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Iga suhtluse skemaatilise diagrammi ilmnes joonis x tähistab ühe juurdepääsu juhtimine stsenaarium, mis võib olla hõlmatud Azure AD. Allpool on teil iga stsenaarium kirjeldus.

1. tingimusvormingu juurdepääsu rakendused, mis on majutatud kohapealse: saate kasutada registreeritud seadmed koos juurdepääsupoliitikaid rakendusi, mis on konfigureeritud Windows Server 2012 R2 AD FS-i abil. Juurdepääsu kohapealse häälestamise kohta leiate lisateavet teemast [häälestamise kohapealse juurdepääsu Azure Active Directory seadme registreerimine abil](active-directory-conditional-access-on-premises-setup.md).
2. juurdepääsu kontroll Azure haldusportaali: Azure'i on ka võimalus määrata juurdepääsu haldusportaali abil RBAC (Rollipõhine juurdepääsu juhtimine). See meetod võimaldab ettevõtte piirata toimingute kohta, mida inimesel saab teha, kui ta on juurdepääs Azure'i haldusportaal summa. Kasutades RBAC juurdepääsu portaali, lähenedes IT-administraatorid ca Delegaadi juurdepääs, kasutades järgmist juurdepääsu juhtimine:

 - Rühma-põhiste Rollimäärang: saate määrata juurdepääsu Azure AD rühmad, mida saate sünkroonida kohaliku Active Directoryst. See võimaldab teil ära kasutada olemasoleva investeeringud teie ettevõttes on tehtud instrumentaarium ja protsesside haldamiseks rühmad. Saate jaotises delegeeritud halduse funktsioon Azure AD Premium.
 - Sisseehitatud rollid Azure võimendada: saate kasutada kolme rolli – omanik, kaasautor ja lugeja, veenduge, et kasutajad ja rühmad on õigus teha ainult oma tööd teha toiminguid.
 - Varundustöö juurdepääsu ressursse: saate määrata kasutajate ja rühmade teatud tellimuste puhul, ressursirühm või on üksikute Azure ressurss, nt veebisaidile või andmebaasi rollid. Nii saate tagada, et kasutajatel on juurdepääs kõik vajalikud ressursid ja puudub juurdepääs ressursse, mis ei pea haldamiseks.

>[AZURE.NOTE] Lugege [Rollipõhine juurdepääsu reguleerimine Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) teada seda võimalust lähemalt. Arendajatele, mis on rakenduste loomine ja neile juurdepääsu reguleerimine kohandatava, on võimalik kasutada Azure AD Rakenduse rollide autoriseerimine. Lugege läbi see [Web Appis-RoleClaims-DotNet näide](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) kuidas luua rakenduse kasutada seda võimalust.

3. tingimusvormingu juurdepääsu Office 365 rakenduste ja Microsoft Intune'i: IT-administraatorid saavad ettevalmistamise tingimusvormingu juurdepääsupoliitikaid seadme ettevõtte ressursid, samal ajal lubamisel Teabetöötajad nõuetele vastavuse seadmes juurdepääsu tagamiseks. Lisateabe saamiseks vt [Tingimusvormingu Juurdepääsupoliitikaid seadme Office 365 teenuste jaoks](active-directory-conditional-access-device-policies.md).

4. tingimusvormingu Accessi rakenduste Saas: [see funktsioon](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) võimaldab teil konfigureerida rakenduse kohta mitmikautentimise juurdepääsureeglid ja blokeerimise pole võrgus usaldusväärsete kasutajate jaoks. Saate rakendada kõigile kasutajatele rakendusse või ainult määratud turvalisus rühmades kasutajate jaoks määratud mitmikautentimise reegleid. Kasutajate võib välistatud mitmikautentimise nõue, kui need on juurdepääs rakenduse IP-aadress, mis ettevõtte sees võrgu kaudu.

Kuna juurdepääsu reguleerimine suvandite kasutamiseks mitmekihiliste lähenemine võrdlus need suvandid ei rakendata, selle ülesande jaoks. Veenduge, et teil on kasutamine kõik suvandid saadaval iga stsenaariumi puhul, mille jaoks on vaja juurdepääsu oma ressursse.

## <a name="define-incident-response-options"></a>Langeva vastamise suvandite määratlemine
Azure AD aitab see identiteedi turberiskidele keskkonnas kasutaja tegevuse jälgimine, saate seda kasutada Azure AD juurdepääsu ja kasutusaruanded võimalus saada nähtavus terviklus ja ettevõtte kataloogi turvalisus. Selle teabe IT-administraator saab paremini määratleda, kus võimalikest võimalik, et nad saavad piisavalt leping neid riske leevendada.  [Azure'i AD Premiumi tellimus](active-directory-get-started-premium.md) on turvalisus aruandeid, mis võimaldab selle teabe hulk. [Azure AD aruannete](active-directory-view-access-usage-reports.md) on liigitatud, nagu allpool näidatud:

- **Normaalne aruannete**: sisaldavad sündmused, mis leidsime olema Anomaalne sisselogimine. Meie eesmärk on teha teile teada selliste tegevuste ja võimaldab teil võimalik teha kindlaks, kas sündmuse on kahtlane.
- **Integreeritud rakenduste aruande**: pakub teadmisi, kuidas teie asutuses kasutatakse pilv rakendusi. Azure Active Directory pakub integreerimine tuhandete pilv rakendusi.
- **Tõrketeated**: tõrkeid, mis võivad ilmneda ettevalmistamise välistes rakendustes kontod.
- **Kasutaja aruandeid**: kuvada andmete mõne kindla kasutaja seade/märki.
- **Logid**: kõik auditeeritud sündmuste kirje sisaldavad viimase 24 tunni jooksul, viimase 7 päeva jooksul või viimase 30 päeva jooksul, kui ka rühma tegevuse muutmine ning parooli lähtestamine ja registreerimise tegevuse.

>[AZURE.TIP]
Teine aruanne, mis aitab juhtumi juhtum vastuse meeskonnas on [kasutaja lekkinud mandaadiga](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) aruanne.  Aruande toob esile vasteid nende lekkinud identimisteabe loendi ja oma rentniku vahel.

Muud olulised sisseehitatud aruannete Azure AD langeva vastuse käigus kasutatavate ning on:

- **Parooli lähtestamise tegevuse**: anda admin teadmisi, kuidas aktiivselt asutuses on kasutusel parooli lähtestamine.
- **Parooli lähtestamise registreerimise tegevuse**: annab ülevaate, kuhu kasutajad nende meetodite parooli lähtestamiseks, registreeritud olema ja millised võimalused valitud.
- **Rühma tegevuse**: pakub muudatuste ajaloo rühma (ex: kasutajaid lisada või eemaldada) mis olid algatatud Access paanil.

Lisaks tuum saadaval Azure AD Premium, mida saate kasutada juhtum vastuse uurimise käigus võimalus aruandluse, saate seda kasutada auditilogi aruande saada teave, näiteks:

- Rollirühma liikmete muudatuste (ex: lisatakse üldadministraatori rolliga kasutaja)
- Värskenduste mandaat (ex: parooli muutmise)
- Domeenide haldamine (ex: kinnitatava domeeni, domeeni eemaldamine)
- Lisada või eemaldada rakendusi
- Kasutajate haldamine (ex: lisamine, eemaldamine, kasutaja värskendamine)
- Lisada või eemaldada litsentse

Kuna langeva vastuse suvandite kasutamiseks mitmekihiliste lähenemine võrdlus need suvandid ei rakendata, selle ülesande jaoks. Veenduge, et teil on kasutamine iga stsenaariumi puhul, mis nõuab teie ettevõtte langeva vastuse käigus Azure AD aruannete võimalus kasutada kõiki võimalusi.

## <a name="next-steps"></a>Järgmised sammud
[Hübriid identiteedi haldustoimingute määratlemine](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
