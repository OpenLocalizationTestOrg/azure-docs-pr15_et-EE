<properties
    pageTitle="Häälestamiseks Azure Active Directory ise teenust access Rakendusehaldus | Microsoft Azure'i"
    description="Iseteeninduslik rühma haldamine võimaldab kasutajatel luua ja hallata turberühmad või Azure Active Directory teenusekomplekti Office 365 rühmade ja kasutajate pakub võimalust taotluse turberühma või Office 365 rühmaliikmeid"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Kuidas häälestada Azure Active Directory jaoks iseteenindusliku rühma haldamine

Iseteeninduslik rühma haldamine võimaldab kasutajatel loomine ja haldamine turberühmad või Office 365 rühmade Azure Active Directory (Azure AD). Kasutajad saavad nõuab turberühma või Office 365 rühmaliikmeid ja seejärel rühma omanik kinnitamine või keelduda. Nii saate igapäevase juhtimise rühmakuuluvus delegeeritud inimestele, kes mõista business kontekstis selle liikmesuse ülevaade. Iseteeninduslik rühma haldamine funktsioonid on saadaval ainult turberühmad ja Office 365 rühmade jaoks, kuid mitte meililoaga turberühmad ja leviloendid.

Iseteeninduslik rühma haldamine praegu hõlmab kahte põhistsenaariumi: delegeeritud haldus ja iseteenindusliku rühma haldamine.

- **Delegeeritud haldus** 
   näide on administraator, kes on juurdepääsu SaaS rakendus, mis kasutab ettevõtte haldamine. Nende juurdepääsu õiguste haldamine muutub tülikas, nii, et see administraatori palub ettevõtte omanik uue rühma loomine. Administraator määrab Accessi rakenduse uude rühma ja lisab rühma kõik juurdepääs juba rakendusse inimesed. Ettevõtte omanik, siis saate lisada kasutajaid ja nende kasutajate on ette valmistatud automaatselt rakendusse. Ettevõtte omanik ei pea ootama administraator hallata kasutajate jaoks. Kui administraator annab sama õiguse ülemuse erinevate business rühma, siis saate hallata sellele inimesele oma kasutajate jaoks. Ettevõtte omanik ega ülemus saate vaadata või teiste kasutajate haldamine. Administraator, saate ikkagi vaadata kõik kasutajad, kellel on juurdepääs rakenduse ja Blokeeri juurdepääsuõigused vajaduse korral.

- **Iseteeninduslik rühma haldamine** 
   stsenaariumi näide on kaks kasutajad, kellel on SharePoint Online'i saitidel, mis need häälestamine iseseisvalt. Nad soovivad oma saitide üksteise meeskondadel juurdepääsu andmine. Selleks nad saavad luua ühe rühma Azure AD ja SharePoint Online'i üks neist valib selle rühma oma saitidele juurdepääsu. Kui keegi soovib juurdepääsu, nad seda taotleda juurdepääsu paneel ja pärast kinnitamist nad saavad nii SharePoint Online'i saitidele juurdepääsu automaatselt. Hiljem, üks neist otsustab, et kõik inimesed saidile peaks ka juurdepääs SaaS rakenduseks. Administraator SaaS rakenduse saate lisada rakenduse SharePoint Online'i saidile juurdepääsu õigused. Seejärel annab taotlused, mis on kinnitatud saada juurdepääsu kaks SharePoint Online'i saitidel ja selle SaaS rakenduse.

## <a name="making-a-group-available-for-end-user-self-service"></a>Rühma kättesaadavaks tegemine lõppkasutaja iseteenindusliku

1. Avage [Azure'i klassikaline portaalis](https://manage.windowsazure.com)Azure AD kataloogi.

2. Klõpsake menüü **konfigureerimine** määratud **delegeeritud haldus** lubatud.

3. **Kasutajad saavad luua turberühmad** või **kasutajad saavad luua Office rühmade** määramine lubatud.

Kui **kasutajad saavad luua turberühmad, kellel** on lubatud, kataloogis kõik kasutajad tohivad loomine uue turberühmad ja nende rühmade liikmete lisamine. Ka näitaks nende uute rühmade juurdepääsu paneeli kõigi teiste kasutajate jaoks. Kui rühma poliitikasätete lubab seda, luua teiste kasutajate kutsed liitumiseks nende rühmade. Kui **kasutajad saavad luua turberühmad** on keelatud, kasutajad ei saa luua rühmad ja rühmi, mille nad on omaniku ei saa muuta. Siiski nad endiselt nende rühmade liikmestaatuste haldamine ja teiste kasutajate taotluste liituda nende rühmade kinnitamine.

Saate **kasutajatele, kes saab kasutada turberühmad jaoks iseteenindusliku** saavutamiseks veel kohandatud Accessi juhtida iseteenindusliku rühma haldamine kasutajate jaoks. Kui **kasutajad saavad luua rühmad** on lubatud, kataloogis kõik kasutajad tohivad luua uusi rühmi ja nende rühmade liikmete lisamine. Määrata ka **kasutajatele, kes saab kasutada turberühmad jaoks iseteenindusliku** mõnda, olete piiramine rühmitada ainult piiratud kasutajate rühma haldus. Kui seda parameetrit on seatud mõned, peate lisama kasutajate rühma SSGMSecurityGroupsUsers luua uusi rühmi ja liikmete lisamine. Kõik **kasutajad, kes saab kasutada turberühmad jaoks iseteenindusliku** seadmisega lubada kõigi kasutajate kataloogis luua uusi rühmi.

**Mida saate kasutada turberühmad jaoks iseteenindusliku** Rühmaboksi abil saate määrata kohandatud rühma, mille liikmed saavad kasutada iseteenindusliku nimi.

## <a name="additional-information"></a>Lisateave

Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)

* [Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)

* [Mis on Azure Active Directory?](active-directory-whatis.md)

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
