<properties
   pageTitle="Kuidas rakendused lisatakse Azure Active Directory."
   description="Selles artiklis kirjeldatakse, kuidas rakenduste lisatakse Azure Active Directory eksemplar."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Kuidas ja miks rakenduste lisatakse Azure AD

Üks esialgu segadust tekitavate asju, kui teie Azure Active Directory eksemplar rakenduste loendi vaatamiseks mõistmine kui rakenduste tulid ja miks neid on.  See artikkel annab kõrge ülevaade kuidas rakendused on esindatud kataloogis ja pakkuda konteksti, mis aitavad teil mõista, kuidas rakenduse tuli kataloogis olla.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Milliseid teenuseid pakkuda Azure AD rakendustele?

Rakenduste lisatakse Azure AD ära kasutada ühte või mitut pakub teenuseid.  Need teenused on järgmised:

* Rakenduse autentimise ja luba
* Kasutaja autentimise ja luba
* Ühekordne sisselogimine (SSO) liitmist või parooli abil
* Kasutajate ettevalmistamine ja sünkroonimine
* Rollipõhine juurdepääsu reguleerimine; Kataloogi rakenduse rollide täitmiseks rollide määratlemiseks vastavalt rakenduse autoriseerimine kontrollid.
* OAuthi autoriseerimine teenused (kasutab Office 365 ja muude Microsofti rakenduste lubada API-de/ressurssidele.)
* Teenuserakenduse avaldamine ja puhverserveri; Rakenduse privaatvõrk Internetis avaldamine

## <a name="how-are-applications-represented-in-the-directory"></a>Kuidas rakendused kataloogis esitatud?

Rakendused on esindatud kasutamine 2 objektide Azure AD: objekti rakenduste ja teenuste peamine eesmärk.  On ühe rakenduse objekt, registreeritud "home" ja "omanik" või "avaldamise" directory ja ühe või mitme teenuse põhisumma objektid, mis tähistab iga kataloogi, kus see toimib rakenduses.  

Objekti application kirjeldatakse rakenduse Azure AD (mitme rentniku teenus) ja võib sisaldada ühte järgmistest: (*Märkus*: see ei ole tulemus.)

* Nimi, Logo ja Publisher
* Saladusi (sümmeetriline ja/või asümmeetriline võtmed kasutatakse rakendus)
* API sõltuvused (OAuthi)
* API-de/ressursid/otsinguulatuste avaldatud (OAuthi)
* Rakenduse rollid (RBAC)
* SSO metaandmete ja konfigureerimine (SSO)
* Kasutaja ettevalmistamise metaandmete ja konfigureerimine
* Puhverserveri metaandmete ja konfigureerimine

Teenuse põhilise on iga kaust, kui rakendus toimib, sh oma kodune kataloogi rakenduse kirje.  Teenuse põhilise:

* Viitab objekti rakenduse kaudu atribuudi rakenduse id
* Kirjete kohalik kasutaja ja rühma rakenduse rollimääranguid
* Kirjete kohalik kasutaja ja administraatori õigused antud rakendus
    * Näide: rakenduse teatud kasutajate e-posti juurdepääsu õigus
* Kirjete Kohalikud poliitikad, sh juurdepääsu poliitika
* Kirjete kohaliku alternatiivse kohaliku sätted rakendus
    * Nõuded teisendus reeglid
    * Atribuudi vastendused (kasutaja ettevalmistamise)
    * Rentniku kindla rakenduse rollid (kui see rakendus toetab kohandatud rollid)
    * Nime/Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Rakenduse objektid ja teenuse põhisumma üle kataloogide skeem

![Skeem, mis illustreerivad, kuidas rakenduse objektide ja teenuse põhisumma olemasolevaid Azure AD eksemplarid.][apps_service_principals_directory]

Nagu näete joonis.  Kahe kataloogi Microsoftil ettevõttesiseselt (vasakul) kasutab rakenduste avaldada.

* Üks Microsofti Apps (Microsofti teenuste kataloogis)
* Üks eelnevalt integreeritud 3 tootja rakenduste (rakenduse Galerii directory)

Rakenduse tootjad/müüjad, kes integreerimine Azure AD peavad olema avaldamise kataloogi.  Mõned (SAAS Directory).

Rakendused, mida saate lisada endale järgmised.

* Rakendusi saate välja töötada (integreeritud AAD)
* Saate ühendatud rakendused ühekordse sisselogimise
* Rakenduste avaldatud Azure AD Rakenduse puhverserveri abil.

### <a name="a-couple-of-notes-and-exceptions"></a>Paar märkmeid ja erandid

* Kõik teenuse põhisumma osutage tagasi rakenduse objektid.  Ta on? Kui Azure AD algselt ehitatud rakenduste teenused on palju piiratud ning teenuse põhilise piisavad loomise rakendus identiteedi.  Algse põhisumma teenuse lähemal kuju Windows Server Active Directory teenuse konto.  Seetõttu on võimalik luua teenuse põhisumma esmalt luua rakenduse objekti Azure AD PowerShelli abil.  Graph API nõuab objekti rakenduse enne loomine teenuse põhisumma.
* Eespool kirjeldatud teavet kõigilt on praegu avatud programmiliselt.  Järgnevalt on ainult UI saadaval.
    * Nõuded teisendus reeglid
    * Atribuudi vastendused (kasutaja ettevalmistamise)
* Lisateavet üksikasjalikku teavet teenuse põhilise ja Azure AD Graph REST API viide dokumentatsiooni palun vaadake rakenduse objektid.  *Vihje*: The Azure AD Graph API dokumentatsiooni on kõige lähemal asi skeemi viite Azure AD, mis on praegu saadaval.  
    * [Rakenduse](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Teenuse põhimakse.](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Kuidas rakendused lisatakse minu Azure AD eksemplari?
On palju võimalusi rakenduse saab lisada Azure AD:

* [Azure Active Directory rakenduste galeriisse](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/) rakenduse lisamine
* Logi üles- / into a 3. tootja rakenduse integreeritud Azure Active Directory (näiteks: [Smartsheet](https://app.smartsheet.com/b/home) või [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Ajal üles- / kasutajatel palutakse anda oma profiili juurdepääsuks õigus rakendus ja muud õigused.  Esimene isik nõusolekut põhjustab teenuse põhilise tähistav rakenduse kataloogi lisada.
* Üles- / teenuse Microsoft online services nagu [Office 365](http://products.office.com/) sisse logida
    * Kui Office 365 tellimine või ühe või mitme teenuse põhisumma luuakse tähistav erinevad teenused, mida on kasutatud funktsioone, mis on Office 365-ga seostatud pakkuda kataloogis prooviversiooni alustada.
    * Mõne Office 365 teenuste nagu SharePointi loomine teenuse põhisumma alusel käimas lubamiseks turvaline side komponendid, sh töövoogude vahel.
* Arendate rakenduse lisamine leiate Azure'i haldusportaal: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Lisage rakenduse arendate Visual Studio abil vaadata.
    * [ASP.net-i autentimismeetodite](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Ühendatud teenused](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* [Azure'i AD Rakenduse puhverserveri](https://msdn.microsoft.com/library/azure/dn768219.aspx) kasutada rakenduse lisamine
* Ühenduse loomine rakenduse jaoks ühekordse sisselogimise SAML või parooli SSO abil
* Paljud teised, sh erinevaid arendaja kogemusi Azure ja/API Exploreri kogemusi üle arendaja andmekeskuste

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kellel on õigus lisada minu Azure AD eksemplari rakendusi?

Ainult globaalse administraatorid teha järgmist.

* Rakenduste lisamine galeriist Azure AD Rakenduse (eelnevalt integreeritud 3 tootjate rakendused)
* Rakenduse abil Azure AD Rakenduse puhverserveri avaldamine

Kõigi kasutajate kataloogis on õigused rakendusi, et nad on välja ja oma äranägemisel üle millised rakendused need ühiskasutus/anda juurdepääsu oma ettevõtte andmete lisamine.  *Ärge unustage kasutaja sisselogimine üles/rakenduse ja õiguste andmine võib kaasa tuua teenuse põhilise, mis on loodud.*

See võib olla esialgu heli kohta, kuid pidage meeles järgmist.

* Rakendused on võimalus kasutada Windows Server Active Directory kasutaja autentimise aastaid nõudmata taotlust registreeritud/salvestatud kataloogis.  Nüüd asutuses on parem nähtavus täpselt mitu rakendus kasutab kataloogi ja mida.
* Administraator lähtuv rakenduse avaldamine registreerimis protsess ei ole vaja.  Active Directory Federation Services see on tõenäoline, et administraator oli nimega osaleja nimel arendajate rakenduse lisamiseks.  Nüüd saate arendajate iseteenindusliku.
* Sisselogimine/kuni rakenduste kaudu oma ettevõtte kontod business eesmärgil kasutajad on hea.  Kui lahkumist hiljem organisatsiooni, kaotavad Accessi rakendus, mida nad kasutavad oma kontole.
* Kas teil on, millised andmed on ühiskasutuses, millega rakendus on hea kirje.  Andmed on rohkem kui kunagi varem teisaldatavad ja Eemalda kirje, kes milliseid andmeid ühiskasutusse millised rakendused on kasulik.
* Rakendused, kes kasutavad Azure AD OAuthi otsustada täpselt milliseid õigusi, mida kasutajad saavad anda rakenduste ja õigusi nõudvad nõustumiseks administraator.  Selle sihtkoha selge, et ainult administraatorid saate nõustute suuremat otsinguulatuste ja olulisemad õigused.
* Kasutajate lisamine ja rakenduste pääsevad oma andmetele juurde on auditeerida sündmused nii, et saate vaadata auditiaruanded Azure'i haldamine portaalis määrata, kuidas rakendus on lisatud kataloogi.

**Märkus:** *Microsoft ise opsüsteem abil mitu kuud nüüd vaikimisi konfiguratsiooni.*

Kõik see ütles, on võimalik, et takistada kasutajatel kataloogis rakenduste lisamine ja oma äranägemisel teostada üle millist teavet nad ühiselt kasutada rakenduste kataloogi konfiguratsiooni Azure'i haldusportaal.  Pärast konfiguratsioon pääseb Azure haldusportaali oma kataloogi "Konfigureerimine" menüü sees.

![Kuvatõmmis UI integreeritud rakenduse sätete konfigureerimine][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Lisateavet selle kohta, kuidas lisada Azure AD rakenduste ja teenuste jaoks rakenduste konfigureerimine.

* Arendajad: [saate teada, kuidas integreerida AAD rakendus](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Arendajad: [Läbivaatus proovi kood rakenduste integreeritud Azure Active Directory github](https://github.com/AzureADSamples)
* Arendajad ja IT-spetsialistidele: [Azure Active Directory Graph API REST API dokumentatsiooni läbivaatus](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* [Saate teada, kuidas kasutada Azure Active Directory eelnevalt integreeritud rakendused rakendus galeriist](https://msdn.microsoft.com/library/azure/dn308590.aspx) tuttavaks
* IT-spetsialistid: [õpetused teatud eelnevalt integreeritud rakenduste konfigureerimine](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* [Saate teada, kuidas avaldada rakenduse abil Azure Active Directory rakenduse puhverserveri](https://msdn.microsoft.com/library/azure/dn768219.aspx) tuttavaks

## <a name="see-also"></a>Vt ka

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
