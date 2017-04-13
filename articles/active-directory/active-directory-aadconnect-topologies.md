<properties
    pageTitle="Azure'i AD-ühendus: Toetatud topoloogiatest | Microsoft Azure'i"
    description="Selles teemas üksikasjad toetatud ja pole toetatud topoloogiatest Azure'i AD-ühenduse"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Topoloogiatest Azure'i AD-ühenduse loomine
Selles teemas eesmärk on erinevatest asutusesisestest ja Azure AD topoloogiatest Azure'i AD-ühenduse sünkroonimisprobleemide lahendusena integreerimise kirjeldamiseks. See kirjeldab nii toega ja toeta konfiguratsioone.

Legendi piltide dokumendis:

Kirjeldus | Ikoon
-----|-----
Kohapealse Active Directory kogumis| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory abil filtreeritud importimine| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure'i AD-ühendus sünkroonimine serveriga| ![Sünkroonimine](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure'i AD-ühendus sünkroonimine serveriga "Lavastus mode"| ![Sünkroonimine](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync FIM2010 või MIM2016| ![Sünkroonimine](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure'i AD-ühendus sünkroonimine serveriga üksikasjalikud| ![Sünkroonimine](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure'i AD-kataloog |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Mittetoetatavad stsenaarium | ![Pole toetatud](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Azure AD rentniku Koputus ühe mets
![Ühe mets ühe rentniku](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Ühe mets on kõige levinum topoloogia asutusesiseselt, üks või mitu domeeni ja ühe Azure AD rentniku. Azure'i AD-autentimine, kasutatakse parooli sünkroonimine. Azure'i AD-ühenduse kiire installimise toetab ainult selle topoloogia.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Mets ühe, mitme ühe Azure AD rentniku sünkroonimine serveriga
![Ühe mets filtreeritud pole toetatud](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

On mitme Azure'i AD-ühenduse sünkroonimine serveriga ühendatud sama Azure AD rentniku, välja arvatud [lavastus server](#staging-server)ei toetata. See ei toetata, isegi juhul, kui need on konfigureeritud sünkroonima üksteist välistavad objektide kogumi. Teil võib olla pidas seda kui te ei jõua kõik domeenid mets ühe serverist või üle mitme serveri Laadi üles.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Mitme investeeringute korral ühe Azure AD rentniku
![Mitme mets ühe rentniku](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Mitme organisatsioonidel on keskkonnas koos mitme kohapealse Active Directory kogumit. On rohkem kui ühe kohapealse Active Directory kogumis on erinevad põhjused. Tüüpilised näited kujunduse koos konto ressursi kogumit ja seetõttu pärast ühinemise või omandamise.

Kui teil on mitu kogumit, tuleb kõik metsade kättesaadav, ühe Azure'i AD-ühenduse sünkroonimine serveriga. Teil pole liituda server domeeni. Vajaduse korral kõik mets saavutamiseks paigutada server võrgu DMZ.

Azure'i AD-ühenduse installimise viisard pakub mitmeid võimalusi konsolideerimiseks esindatud mitme metsade kasutajad. Eesmärk on, et kasutaja on ainult esindatud üks kord Azure AD. On mõned levinud topoloogiatest, mida saate konfigureerida kohandatud Installitee installiviisardis. Valige vastav valik tähistav oma topoloogia **kordumatult tuvastada kasutajate**lehel. Konsolideerimise on ainult kasutajate jaoks konfigureeritud. Vaikimisi konfiguratsioon ei ühendata dubleeritakse rühmad.

Järgmises jaotises kirjeldatud levinud topoloogiatest: [eraldi topoloogiatest](#multiple-forests-separate-topologies), [kogu](#multiple-forests-full-mesh-with-optional-galsync)ja [Konto ressursi](#multiple-forests-account-resource-forest).

Vaikimisi sünkroonimine Azure'i AD-ühenduse konfiguratsiooni eeldab:

1. Kasutajatel on lubatud ainult ühe kontoga ja mets, kus asub see konto on kasutatud autentida. Eeldatakse nii parooli sünkroonimise ja ühinemise jaoks. UserPrincipalName ja sourceAnchor/immutableID pärit see mets.
2. Kasutajatel on ainult üks postkast.
3. Kasutaja postkasti majutav mets on parim andmete kvaliteedi atribuudid, mis on nähtav, klõpsake selle Exchange'i globaalse aadressiloendi (GAL). Kui kasutaja on ühtegi postkasti, klõpsake mis tahes mets saab kasutada aidata nende atribuutide väärtused.
4. Kui teil on lingitud postkasti, siis on ka mõne muu kontoga erinevate mets, kasutatakse Logi sisse.

Kui teie keskkond, mis ei sobi need oletused, siis toimub järgmine:

- Kui teil on rohkem kui ühe aktiivse konto või rohkem kui üks postkast, sync engine huvitavat ühte ja teine ignoreerimine.
- Aktiivse kontoga lingitud postkasti Azure AD ei ekspordita. Kasutaja ei ole esindatud mis tahes rühma liige. Lingitud postkasti DirSync oleks alati esindab tavaline postkasti. See muudatus on teadlikult erinevat käitumist nende paremaks stsenaariumid mitme mets.

Lisateavet leiate [vaikimisi configuation mõistmine](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Mitme investeeringute korral mitme sünkroonimine serveriga ühe Azure AD rentniku
![Toetuseta mitme mets mitme sünkroonimine](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Ei toetata on rohkem kui üks Azure'i AD-ühenduse sünkroonimine serveriga ühendatud ühe Azure AD rentniku. Erandiks on [lavastus serveri](#staging-server)kasutamine.

### <a name="multiple-forests--separate-topologies"></a>Mitme metsade – eraldi topoloogiatest
**Kasutajad on esindatud ainult üks kord üle kõik kataloogid**

![Mitme mets kasutajate üks kord](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Mitme mets eraldi topoloogiatest](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

Selles keskkonnas kõik metsade kohapealse käsitletakse eraldi üksused ja pole kasutaja oleks mõni muu mets Esita.
Iga mets on oma Exchange'i organisatsiooni ning pole GALSync metsade vahel. See topoloogia võib olla olukord pärast ühinemise/tuua või mõnes asutuses, kus töötab kõigis business üksteisest eraldatud. Need metsad Azure AD samas asutuses on ja kus ühendatud GAL kuvada.
Järgmisel pildil on iga objekti iga mets esindatud üks kord metaversumi ja kokkuvõtliku sihtrentnikus Azure AD.

### <a name="multiple-forests--match-users"></a>Mitme metsades-match kasutajad
**Kasutaja identiteedi olemas üle mitme kataloogide**

Kõik need stsenaariumid levinud on see jaotuse ja turberühmad võivad sisaldada kasutajad, kontaktide ja FSPs (välis turvalisus põhisumma)

FSPs kasutatakse lisatakse tähistada liikmete muude metsast turberühmas. Kõik FSPs on otsustanud reaal objekti Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Mitme metsade – täielik võrk koos valikuline GALSync
**Kasutaja identiteedi olemas üle mitme kataloogide. Vastavad abil: atribuuti Mail**

![Mitme mets kasutajate e-posti](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Mitme mets täielik silma](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Kogu topoloogia võimaldab kasutajatel ja ressursse, mis tahes mets asuma ja sagedamini oleks kahesuunaline usalduste metsade vahel.

Kui Exchange'i rohkem kui üks mets, soovi korral võib kohapealse GALSync lahenduse. Igale kasutajale oleks esindatud kõik muud metsades kontaktina. GALSync rakendatakse tihti Forefront Identity Manager 2010 või Microsoft Identity Manager 2016 abil. Azure'i AD-ühendus ei saa kasutada kohapealse GALSync.

Selle stsenaariumi korral on identiteedi objektide liitunud, kasutades atribuuti mail. Kasutaja postkasti ühe mets on ühendatud teiste metsades kontaktidega.

### <a name="multiple-forests--account-resource-forest"></a>Mitme metsades-konto ressursi mets
**Kasutaja identiteedi olemas üle mitme kataloogide. Vastavad abil: ObjectSID ja msExchMasterAccountSID atribuudid**

![Mitme mets kasutajate ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Mitme mets AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

Konto ressursi mets topoloogia, teil on ühe või mitme konto kogumit aktiivne kasutaja kontod. Teil on ka ühe või mitme ressursside kogumit keelatud kontoga.

Selle stsenaariumi (vähemalt ühe) **Ressursi mets** loodab kõik **konto metsade**. Ressursi mets on tavaliselt laiendatud AD skeemiga Exchange'i ja Lynci. Kõikide Exchange'i ja Lynci teenuste kui ka muude ühisteenuste asub see mets. Kasutajatel on keelatud kasutajakonto, see mets ja mets kontoga on seotud postkasti.

## <a name="office-365-and-topology-considerations"></a>Office 365 ja topoloogia kaalutlused
Mõne Office 365 teenustest kehtivad teatud piirangud, et toetatud topoloogiatest. Kui kavatsete kasutada mõnda neist, siis lugege toetatud topoloogiatest teema töökoormus.

Töökoormus |  
--------- | ---------
Exchange Online | Kui seal on rohkem kui üks Exchange'i organisatsiooni kohapealse (ehk teisisõnu öeldes Exchange'i kasutusele võetud rohkem kui üks mets), siis kasutage Exchange 2013 SP1 või uuem versioon. Üksikasjad leiate siit: [koos mitme Active Directory kogumit hübriidjuurutused](https://technet.microsoft.com/library/jj873754.aspx)
Skype'i ärirakenduses | Kui kasutate mitut metsade kohapealse, toetatakse ainult konto ressursi mets topoloogia. Üksikasjade jaoks toetatud topoloogiatest leiate siit: [Skype'i ärirakenduse Server 2015 keskkonna nõuded](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Lavastus server
![Lavastus Server](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure'i AD-ühendus toetab installimist teine server **lavastus**režiimis. Selles režiimis server loeb andmed kõigi ühendatud kataloogide, kuid ei kirjuta midagi ühendatud kataloogide. See on kasutusel tavaline domeenikirjeid ja on seega identiteedi andmed värskendatud koopia. Kui esmane server ei ole katastroof võib nurjuda üle lavastus serveriga. Selleks viisardis Azure'i AD-ühenduse. See teise server võib asuda parim erinevate andmekeskuses Kuna taristu jagatakse esmane server. Peate käsitsi kopeerima konfiguratsiooni muudatused esmane serveris teine server.

Lavastus serveris saate kasutada ka testimiseks uue kohandatud konfiguratsiooni ja selle mõju oma andmete põhjal. Saate vaadata muudatusi ja reguleerimine konfiguratsiooni. Kui olete rahul uue konfiguratsiooni, saate teha lavastus serveri active server ja vana active server seadmine lavastus režiim.

Seda meetodit saate kasutada ka asendamiseks active Synci serveri. Uue serveri ettevalmistamine ja seadke selle lavastus režiimi. Veenduge, et see on hea olekus Keela lavastus režiimi (mis muudab aktiivne), ja praegu aktiivne server sulgeda.

On võimalik on rohkem kui üks lavastus server, kui soovite mitme varukoopiate erinevate andmekeskuste.

## <a name="multiple-azure-ad-tenants"></a>Mitme Azure AD rentniku
Microsoft soovitab, võttes ühe rentniku Azure AD ettevõtte jaoks.
Enne, kui kavatsete kasutada mitme Azure AD rentniku, hõlmab järgmisi teemasid levinud stsenaariumi, mis võimaldab teil kasutada ühe rentniku.

Teema: |  
--------- | ---------
Delegeerimine abil haldus ühikud | [Azure AD haldus üksuste haldus](active-directory-administrative-units-management.md)

![Mitme mets mitme rentniku](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Azure'i AD-ühenduse sünkroonimine serveriga ja on Azure AD rentniku 1:1 seos on. Iga Azure AD rentniku jaoks tuleb teil ühe Azure'i AD-ühenduse sünkroonimine serveri install. Azure AD rentniku eksemplarid on eraldatud kujundus ja ühel kasutajad ei näe kasutajad teine rentnik. Kui see eraldamine on mõeldud, siis see on toetatud konfiguratsiooni, kuid muul juhul tuleks kasutada ühe Azure AD rentniku mudel.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Iga objekti ainult üks kord Azure AD rentniku
![Filtreeritud ühe mets](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

See topoloogia on ühe Azure'i AD-ühenduse sünkroonimine serveriga ühendatud iga Azure AD rentniku. Azure'i AD-ühenduse sünkroonimine serverid peab olema konfigureeritud nii, et iga on objektide sõitmiseks üksteist välistavad kogumi filtreerimiseks. Näiteks saate ulatus iga serveri kindla domeeni või OU. DNS-i domeeni saab registreerida ainult ühe Azure AD rentniku. Funktsiooni UPN-ID asutusesiseses kasutajate AD tuleb kasutada ka eraldi nimeruumid. Näiteks pildi kohal kolme eraldi UPN-i registreeritud sufiksid asutusesisese AD: contoso.com ja fabrikam.com wingtiptoys.com. Kasutajate iga asutusesisese AD domeeni kasutada erinevate nimeruumi.

Ei ole GALsync Azure AD rentniku eksemplarid. Aadressiraamatut Exchange Online'i ja Skype'i ärikasutajatele ainult kuvatakse samal rentnikukontol.

See topoloogia on järgmised piirangud muul viisil toetatud stsenaariumid:

- Ainult ühe Azure AD rentnikega saate lubada Exchange'i hübriidjuurutuse kohapealse Active Directory.
- Windows 10 seadmes saab ainult ühe Azure AD rentniku seostatud.

Üksteist välistavate objektide kogumi nõue kehtib ka tagasikirjutusega. Mõned tagasikirjutusega funktsioonid on toetatud koos selle topoloogia kuna need funktsioonid endale ühe konfiguratsiooni kohapealse:

-   Rühma tagasikirjutusega vaikimisi konfiguratsioon
-   Seadme tagasikirjutusega

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Iga objekti mitu korda on Azure AD rentniku
![Ühe mets mitme rentniku toetuseta](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Toetuseta ühe mets mitme konnektorid](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- See ei toeta mitme Azure AD rentniku sama kasutajal sünkroonida.
- See ei toetata, muuta ühe Azure AD kasutajate kuvatakse teise Azure AD rentniku kontaktidena konfiguratsiooni teha.
- See ei toeta ühenduse mitme Azure AD rentniku sünkroonimine Azure'i AD-ühenduse muutmine.

### <a name="galsync-by-using-writeback"></a>GALsync tagasikirjutusega abil
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure'i AD rentnikud on eraldatud kujundus.

- See ei toeta muuta andmete lugemiseks teise Azure AD rentniku sünkroonimine Azure'i AD-ühenduse konfiguratsiooni.
- See ei toeta kasutajatele nimega kontaktide eksportimine teise kohapealse AD sünkroonimine Azure'i AD-ühenduse kaudu.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync kohapealse sünkroonimine serveriga
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

See on toetatud kasutada FIM2010/MIM2016 asutusesisese Exchange'i organisatsioonide vahelise GALsync kasutajatele. Ühe asutuse kasutajate kuvatakse teises organisatsioonis võõrkeelsed kasutajate/kontaktide hulka. Reklaamid erinevatest asutusesisestest seejärel sünkroonida oma Azure AD rentnikke.

## <a name="next-steps"></a>Järgmised sammud
Saate teada, kuidas installida Azure AD-ühenduse need stsenaariumid, leiate [Azure'i AD-ühenduse kohandatud installi](./connect/active-directory-aadconnect-get-started-custom.md).

Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
