<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: head tavad vaikimisi konfiguratsiooni muutmise | Microsoft Azure'i"
    description="Vaikimisi sünkroonimine Azure'i AD-ühenduse konfiguratsiooni muutmise pakub parimaid tavasid."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure'i AD-ühendus sünkroonimine: head tavad vaikimisi konfiguratsiooni muutmine
Selles teemas eesmärk toetatavad ja mittetoetatavad muudatuste sünkroonimine Azure'i AD-ühenduse kirjeldamiseks.

Loodud Azure'i AD-ühenduse konfiguratsiooni töötab "on" Enamik keskkonnas, mis on Azure AD kohapealse Active Directory sünkroonimine. Mõnel juhul on vajalik mõne muudatuse rakendamine konfiguratsiooni vaja või nõue.

## <a name="changes-to-the-service-account"></a>Teenuse konto muudatuste
Azure'i AD-ühendus sünkroonimine töötab teenus kasutajakontot loodud installimise viisard. Selle teenuse konto sünkroonimine kasutatav andmebaas on krüptimise võtmed. See on loodud 127 tähemärki pikk parooliga ja parool on seatud ei aeguks.

- See on **toetamata** muuta või selle teenuse konto parooli lähtestamine. Nii hävitab krüptimise võtmed ja teenus ei saa andmebaasile juurde pääseda ja ei saa alustada.

## <a name="changes-to-the-scheduler"></a>Muudatuste ajasti
Alates ehitada 1.1 versioonidega (veebruar 2016) saate konfigureerida [Toiminguajasti](active-directory-aadconnectsync-feature-scheduler.md) on erinevate sünkroonimine tsükkeldiagramm kui vaikimisi 30 minutit.

## <a name="changes-to-synchronization-rules"></a>Muudatuste sünkroonimine reeglid
Installimise viisard pakub konfiguratsiooni, mis peaks töötama kõige levinumad stsenaariumid. Kui teil on vaja muuta konfiguratsiooni, peate järgima järgmisi reegleid ikka veel toetatud konfiguratsiooni.

- Saate [muuta atribuudi puhul](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) kui vaikimisi otsese atribuut puhul ei sobi teie ettevõtte jaoks.
- Kui soovite, et [meilivoo atribuut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) ja eemaldage kõik olemasolevad atribuudi väärtused Azure AD, siis peate selle stsenaariumi reegli loomiseks.
- [Reegli soovimatute sünkroonimise keelamine](#disable-an-unwanted-sync-rule) asemel kustutage see. Kustutatud reegel on versiooniuuenduse uuesti luua.
- [Reegli ruut-, muutmine](#change-an-out-of-box-rule), tuleks teha koopia algses reegli ja keelata reegli ruut-. Sünkroonimise reegli redaktori palub ja aitab teil.
- Kohandatud sünkroonimise reeglite abil sünkroonimise reeglite redaktori eksportida. Redaktori pakub PowerShelli skripti, saate neid hõlpsasti katastroof taastamise stsenaarium uuesti.

>[AZURE.WARNING] Reeglite out box sünkroonimine on soovitud sõrmejälje. Kui teete muudatuse reegleid, on sõrmejälje enam sobitamine. Probleeme võib olla tulevikus, kui proovite rakendada uue versiooni Azure'i AD-ühenduse. Ainult muuta nii, nagu on kirjeldatud selles artiklis.

### <a name="disable-an-unwanted-sync-rule"></a>Reegli soovimatute sünkroonimise keelamine
Kustutage reegli ruut-ja sünkroonimine. Uuesti luua järgmise uuele versioonile üleminekul.

Mõnel juhul installimise viisard on koostanud konfiguratsiooni, mis ei tööta teie topoloogia jaoks. Näiteks, kui teil on mõni konto ressursi mets topoloogia, kuid teil on laiendada koos skeemi Exchange'i konto mets skeemiga, siis Exchange'i reeglid ja on loodud konto mets ressursi mets. Sel juhul peate keelata sünkroonimine reegel Exchange.

![Keelatud sünkroonimine reegel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Ülaltoodud joonisel on installimise viisard leidnud skeemiga vana Exchange 2003 konto mets. Selle skeemi laiendamise lisati enne ressursi mets võeti kasutusele Fabrikam's keskkonnas. Sünkroonitakse ühtegi atribuuti ja vana Exchange'i rakendamise tagamiseks peaksite sünkroonimine reegel keelatud, nagu on näidatud.

### <a name="change-an-out-of-box-rule"></a>Muuta reegli ruut.
Kui teil on vaja muuta reegli ruut-, tuleks reegli ruut-, kopeerida ja keelata algse reegel. Tehke soovitud muudatused kloonitud reegel. Sünkroonimise reegli redaktori aitab teil neid juhiseid. Reegli ruut-, avamisel on esitatud selle dialoogiboksi:  
![Hoiatus välja kasti reegel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Valige **Jah** koopia reegli loomiseks. Seejärel avatakse kloonitud reegel.  
![Kloonitud reegel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Klõpsake selle kloonitud reegli teha vajalikud muudatused ulatus, nendega liitumine ja teisendused.

## <a name="next-steps"></a>Järgmised sammud

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
