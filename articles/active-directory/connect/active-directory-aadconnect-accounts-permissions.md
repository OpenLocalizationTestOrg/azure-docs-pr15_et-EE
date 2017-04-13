<properties
   pageTitle="Azure'i AD-ühendus: Kontod ja õigused | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse kontodelt hääletamist ja kasutada ja vajalikud õigused."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure'i AD-ühendus: Kontod ja õigused
Azure'i AD-ühenduse installimise viisard pakub kahte erinevat teed:

- Kiirsätteid, viisardi nõuab rohkem õigusi, nii, et see häälestamine teie konfiguratsiooni hõlpsasti, ilma peaksite kasutajate loomiseks ja konfigureerimiseks õigused eraldi.

- Kohandatud sätteid, viisard pakub rohkem valikuid ja suvandeid, kuid on olukordi, kus soovite tagada, et teil on vajalikud õigused ise.

## <a name="related-documentation"></a>Seotud dokumendid
Kui te ei loe dokumentatsiooni [integreerimine Azure Active Directory oma kohapealse identiteedid](../active-directory-aadconnect.md), järgmises tabelis toodud seotud teemade lingid.

Teema: |  
--------- | ---------
Installige Express sätete abil | [Kiire Azure'i AD-ühenduse installimine](active-directory-aadconnect-get-started-express.md)
Installige, kasutades kohandatud sätteid | [Kohandatud installi Azure'i AD-ühenduse](active-directory-aadconnect-get-started-custom.md)
DirSync täiendamine | [Uuendada Azure AD sünkroonimistööriist (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Kiire sätted installimine
Express sätetes installimise viisard küsib AD DS ettevõtte administraatori identimisteave nii, et teie kohapealse Active Directory saab konfigureerida Azure'i AD-ühenduse vajalikke õigusi. Kui täiendate DirSync, kasutatakse AD DS ettevõtte administraatoritel identimisteabe DirSync kasutatav konto parooli lähtestamine. Samuti vajate Azure AD globaalne administraatori identimisteave.

Viisardi lehel  | Kogutud identimisteave | Vajalikud õigused| Kasutatakse
------------- | ------------- |------------- |-------------
N/A|Kasutaja installimise viisardit| Kohaliku serveri administraator| <li>Loob kohaliku konto, mida kasutatakse [sünkroonimine engine teenusekonto](#azure-ad-connect-sync-service-account).
Ühenduse loomine Azure AD| Azure'i AD directory mandaat | Globaalne administraatoriroll Azure AD | <li>Azure AD kataloogi sünkroonimise lubamine.</li>  <li>[Azure AD konto](#azure-ad-service-account) , mida kasutatakse käimas sünkroonimine tegevuste Azure AD loomine.</li>
Ühenduse loomine AD DS | Kohapealse Active Directory mandaat | Ettevõtte administraatorid (EA) rühma Active Directorys| <li>Loob [konto](#active-directory-account) Active Directory ja õigused seda määrab. Selle konto loonud kasutatakse lugemine ja kirjutamine kataloogi teabe sünkroonimise ajal.</li>

### <a name="enterprise-admin-credentials"></a>Ettevõtte administraatori identimisteave
Neid mandaate kasutatakse ainult installimise ajal ja kasutatakse kui installimine on lõpule jõudnud. See on ettevõtte administraator ja domeeni administraator, veenduge, et Active Directory õiguste saate seada kõigi domeenides.

### <a name="global-admin-credentials"></a>Globaalne administraatori identimisteave
Neid mandaate kasutatakse ainult installimise ajal ja ei kasutata, kui installimine on lõpule jõudnud. Seda kasutatakse kasutatav muudatuste sünkroonimine Azure AD [Azure AD konto](#azure-ad-service-account) loomiseks. Konto võimaldab sünkroonimine funktsioonina Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Õiguste loodud AD DS-i konto jaoks kiirsätteid
Loodud lugemine ja kirjutamine AD DS-i [konto](#active-directory-account) on kiirsätteid loomisel järgmisi õigusi:

Õiguste | Kasutatakse
---- | ----
<li>Ise Directory muudatused</li><li>Paljunda Directory kõik muudatused | Parooli sünkroonimine
Lugemis-ja kirjutamisõigusega kõik atribuudid kasutaja | Impordi- ja Exchange'i hübriidjuurutuse
Lugemis-ja kirjutamisõigusega kõik atribuudid iNetOrgPerson | Impordi- ja Exchange'i hübriidjuurutuse
Lugemis-ja kirjutamisõigusega kõik atribuudid rühmitamine | Impordi- ja Exchange'i hübriidjuurutuse
Lugemis-ja kirjutamisõigusega kõik atribuudid, võtke ühendust | Impordi- ja Exchange'i hübriidjuurutuse
Parooli lähtestamine | Parooli tagasikirjutusega lubamine ettevalmistamine

## <a name="custom-settings-installation"></a>Kohandatud sätteid installimine
Kasutades kohandatud sätteid, tuleb enne installimist luua konto, mida kasutatakse ühenduse loomine Active Directory. Annate selle konto õigused leiate [AD DS konto loomine](#create-the-ad-ds-account).

Viisardi lehel  | Kogutud identimisteave | Vajalikud õigused| Kasutatakse
------------- | ------------- |------------- |-------------
N/A | Kasutaja installimise viisardit|<li>Kohaliku serveri administraator</li><li>Kui täielik SQL Serveri kasutamisel kasutaja peab olema süsteemi administraator (SA SQL)</li>| Vaikimisi luuakse kohalik konto, mida kasutatakse [sünkroonimine engine teenusekonto](#azure-ad-connect-sync-service-account). Konto on loodud ainult siis, kui administraatoril määrata, kindla konto.
Installige sünkroonimisteenused, teenuse konto suvand | AD või kohalik kasutaja konto identimisteave | Kasutaja õigused antakse installimise viisard | Kui admin konto, kasutatakse selle konto teenusekonto sünkroonimise teenuse kohta.
Ühenduse loomine Azure AD | Azure'i AD directory mandaat| Globaalne administraatoriroll Azure AD| <li>Azure AD kataloogi sünkroonimise lubamine.</li>  <li>[Azure AD konto](#azure-ad-service-account) , mida kasutatakse käimas sünkroonimine tegevuste Azure AD loomine.</li>
Ühenduse loomine oma kataloogide | Iga mets, mis on ühendatud Azure AD kohapealse Active Directory mandaat | Õiguste sõltuvad sellest, millist funktsioonide lubamine ja [konto loomine AD DS](#create-the-ad-ds-account) leiduvad |Seda kontot kasutatakse lugeda ja kirjutada kataloogi teabe sünkroonimise ajal.
AD FS server | Loendis iga server, viisardi kogub mandaat, kui sisselogimismandaate kasutaja viisardit ei piisa ühenduse loomine | Domeeni administraator | Installimine ja konfigureerimine AD FS server roll.
Web teenuserakenduse puhverserverite |Loendis iga server, viisardi kogub mandaat, kui sisselogimismandaate kasutaja viisardit ei piisa ühenduse loomine | Kohalik administraator sihtarvutis | Installimine ja konfigureerimine WAP serveri roll.
Puhverserveri usalda identimisteave |Federation teenus (mandaadi puhverserverist kasutab registreerimiseks usalda tunnistus FS sisselogimisteavet usaldada |Domeeni kontoga, mis on kohaliku administraatori AD FS server | Algne FS-WAP usalda serdi kuulumist.
AD FS-i teenuse konto lehel "Domeeni kasutaja konto suvand Kasuta" | AD kasutajatunnust konto | Domeeni kasutaja | Teenuse AD FS-i konto sisselogimisandmed kasutatakse AD kasutajakonto, mille mandaat on olemas.

### <a name="create-the-ad-ds-account"></a>AD DS konto loomine
Kui installite Azure'i AD-ühenduse, saate määrata **oma kataloogide ühendamine** lehel konto kohal Active Directory ja on nõutav antud. Installimise viisard kinnitamine, õiguste ja probleemidest leitakse ainult sünkroonimise ajal.

Millised õigused vajate sõltub valikulised funktsioonid lubada. Kui teil on mitu domeeni, tuleb kõik domeenid mets õigusi anda. Kui te ei luba nende funktsioonide vaikeõigused **Domeeni kasutaja** on piisavalt.

Funktsioon | Õigused
------ | ------
Parooli sünkroonimine | <li>Ise Directory muudatused</li>  <li>Paljunda Directory kõik muudatused
Exchange'i hübriidjuurutus | Kirjutamise õigused dokumenteerida [tagasikirjutusega Exchange'i hübriidjuurutuse](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) kasutajate, rühmade ja kontaktide atribuute.
Parooli tagasikirjutusega | Kirjutamise õigused dokumenteerida [Alustamine parooli haldus](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) kasutajate atribuute.
Seadme tagasikirjutusega | PowerShelli skripti, nagu on kirjeldatud [seadme tagasikirjutusega](../active-directory-aadconnect-feature-device-writeback.md)andnud õigused.
Rühma tagasikirjutusega | Lugeda, luua, värskendada ja kustutada OU, kus jaotuse rühmad peaksid asuma objektide rühmitamine.

## <a name="upgrade"></a>Versiooni täiendamine
Kui täiendate versiooni Azure'i AD-ühenduse uus versioon, peate järgmised õigused.

Peamine | Vajalikud õigused | Kasutatakse
---- | ---- | ----
Kasutaja installimise viisardit | Kohaliku serveri administraator | Värskendage kahendfaile.
Kasutaja installimise viisardit | ADSyncAdmins liige | Sünkroonimise reeglid ja muude konfiguratsiooni muudatusi teha.
Kasutaja installimise viisardit | Kui kasutate täielik SQL server: DBO (või sarnase) sünkroonimise mootori andmebaasi | Teha andmebaasi saiditasemel muudatusi, näiteks tabelid koos uute veergude värskendamine.

## <a name="more-about-the-created-accounts"></a>Lisateavet loodud kontod

### <a name="active-directory-account"></a>Active Directory kontot
Kui kasutate kiirsätteid, siis konto on loodud Active Directory, mida kasutatakse sünkroonimise. Loodud konto asub mets juurdomeeni kasutajate ümbrises ja on oma nime eesliide **MSOL_**. Pikk ja keerukas parool, mis ei lõpe luuakse konto. Kui teil on oma Domeen parooli poliitika, tehke kindlasti pikk ja keerukate paroolide selle konto jaoks lubatud.

![AD konto](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure'i AD-ühendus sünkroonimise teenuse kontod
Kohaliku teenusekonto on loodud installimise viisard (kui te ei määra konto, mida soovite kasutada kohandatud sätteid). Konto on eelkinnitatud **AAD_** ja tegeliku sünkroonimise teenuse kasutatava käivitada. Kui installite domeenikontrolleri Azure'i AD-ühenduse, luuakse konto domeeni. Kui kasutate remote serveris, kus töötab SQL serveri või kui kasutate puhverserverit, mis nõuab autentimist, asuma **AAD_** teenusekonto Domeen.

![Sünkroonimine teenusekonto](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Pikk ja keerukas parool, mis ei lõpe luuakse konto.

Seda kontot kasutatakse muudelt kontodelt paroolid turvaliselt säilitada. Need kontod paroolid on talletatud krüptitud andmebaasi. Privaatvõtmete krüptimise võtmed kaitstud cryptographic services salajane-key krüptimise abil Windows andmete kaitse API (DPAPI). Teenuse konto parooli lähtestamist peaks pole kuna Windows hävitab seejärel krüptimise võtmed turvalisuse põhjustel.

Kui kasutate täielik SQL serveri, teenuse konto on loodud andmebaasi sync engine DBO. Teenus ei tööta eeldatud viisil muude õigustega. Luuakse ka SQL sisselogimine.

Konto antakse ka failid, registrivõtmed ja muude objektide Sync Engine seotud õigused.

### <a name="azure-ad-service-account"></a>Azure'i AD teenusekonto
Konto Azure AD luuakse sünkroonimise teenuse kasutamiseks. Selle konto võib olla tähistatud selle kuvatav nimi.

![AD konto](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Konto kasutatakse serveri nime saate määratletud teine osa kasutajanimi. Järgmisel pildil serveri nimi on FABRIKAMCON. Kui teil on lavastus serverid, iga server on oma konto. Azure AD on piirmäära 10 sünkroonimise teenuse kontod.

Teenuse konto loomist ei lõpe pikk ja keerukas parool. See on antud eriline roll **Directory sünkroonimise kontod** , mis on ainult õigused directory sünkroonimise tööülesannete täitmiseks. Väljaspool Azure'i AD-ühenduse viisard ei saa anda selle teisiti sisseehitatud roll ja Azure portaali kuvab selle konto rolliga **kasutaja**.

![AD konto roll](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](../active-directory-aadconnect.md).
