<properties
   pageTitle="Azure'i AD-ühenduse: Väljaanne versiooniajalugu | Microsoft Azure'i"
   description="Selles teemas on loetletud kõik versioonides Azure'i AD-ühenduse ja Azure AD sünkroonimine"
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
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure'i AD-ühenduse: Väljaanne versiooniajalugu

Azure Active Directory meeskonnatöö regulaarselt värskendab Azure'i AD-ühenduse uute funktsioonide kohta. Kõik täiendused kehtivad kõigi sihtrühmade.

See artikkel on mõeldud aitab teil silma peal, versioonid, mis on välja ja et aru saada, kas peate värskendama, et uusim versioon või mitte.

See on seotud teemade loend.

Teema: |  
--------- | --------- |
Juhised: Azure'i AD-ühenduse täiendamine | [Uuele versioonile mõnelt varasemalt versioonilt uusim](active-directory-aadconnect-upgrade-previous-version.md) väljaanne Azure'i AD-ühenduse saate mitmel viisil.
Vajalikke õigusi | Värskenduse vajalike õiguste, vaadake teemat [kontod ja õigused](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Laadi alla| [Laadi alla Azure AD ühenduse loomine](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Välja: 2016 August

**Fikseeritud küsimustes:**

- Muudatuste sünkroonimine intervall ei toimu, kuni pärast järgmise sünkroonimise tsükli on lõpule jõudnud.
- Azure'i AD-ühendus viisard ei aktsepteeri Azure AD konto, mille kasutajanimi algab allkriips (\_).
- Azure'i AD-ühendus viisard ei autentida Azure AD konto, kui parool sisaldab liiga palju erimärke. Tõrketeade "ei saa kinnitada identimisteabe. Ilmnes ootamatu tõrge ilmnes." tagastatakse.
- Desinstallige lavastus server keelab parooli sünkroonimise Azure AD rentniku ja põhjustab parooli sünkroonimise nurjumise active server.
- Parooli sünkroonimine nurjub aeg-ajalt pole talletatud kasutaja parooli räsi korral.
- Azure'i AD-ühenduse server lubamisel lavastus režiimi parooli tagasikirjutusega ajutiselt pole keelatud.
- Azure'i AD-ühendus viisard ei kuvata tegelik parooli sünkroonimise ja parooli tagasikirjutusega konfiguratsiooni, kui server on lavastus režiim. See näitab need alati, nagu on keelatud.
- Konfiguratsiooni muudatuste sünkroonimine parool ja parooli tagasikirjutusega mitte hoitakse viisardiga Azure'i AD-ühenduse, kui server on lavastus režiim.

**Täiustused.**

- Värskendatud algus-ADSyncSyncCycle cmdlet-käsu Määrake, kas see on võimalik hakata edukalt uus sünkroonimine tsükkel või mitte.
- Lisatud Peata-ADSyncSyncCycle cmdlet-käsu lõpetada sünkroonimise tsükli ja toiming, mis on praegu on pooleli.
- Värskendatud Peata-ADSyncScheduler cmdlet-käsu lõpetada sünkroonimise tsükli ja toiming, mis on praegu on pooleli.
- Azure'i AD-ühenduse viisardi [Directory laiendid](active-directory-aadconnectsync-feature-directory-extensions.md) konfigureerimisel saate nüüd valitud AD-atribuut tüüpi "Teletexi string".

## <a name="111890"></a>1.1.189.0
Välja: juuni 2016

**Fikseeritud probleemi lahenduse ja täiustuse:**

- Azure'i AD-ühendus saate nüüd olema installitud serveris FIPS nõuetele.
    - Parooli sünkroonimise, lugege teemat [parooli sünkroonimise ja FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Fikseeritud küsimus, kus NetBIOS nime ei saanud lahendada FQDN rakenduses Active Directory konnektor.

## <a name="111800"></a>1.1.180.0
Välja: mail 2016

**Uued funktsioonid:**

- Hoiatab ja aitab teil kinnitatava domeeni, kui te ei tee seda enne käivitamist Azure'i AD-ühenduse.
- [Microsofti pilveteenuse Saksamaa](active-directory-aadconnect-instances.md#microsoft-cloud-germany)lisatud tugi.
- Lisatud tugi uusim [Microsoft Azure Governmenti](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) pilvetaristu, uus URL-i nõuetele.

**Fikseeritud probleemi lahenduse ja täiustuse:**

- Lisatud filtreerimine sünkroonimine reegel Editor teha lihtne leida sünkroonimine reeglid.
- Paranenud jõudlus kustutamisel konnektori ruumi.
- Fikseeritud on probleeme, kui samale objektile nii kustutati ja lisatud sama käivitamine (nn Kustuta/lisamine).
- Keelatud sünkroonimine reegel ei saa enam uuesti lubamiseks sisalduv objektide ja atribuutide versioonitäienduse või kataloogi skeemi värskendamine.

## <a name="111300"></a>1.1.130.0
Välja: aprillis 2016

**Uued funktsioonid:**

- Mitme väärtusega atribuute [Directory laiendid](active-directory-aadconnectsync-feature-directory-extensions.md)lisatud tugi.
- Lisatud tugi veel konfiguratsiooni variatsioonide [automaatse versiooniuuenduse](active-directory-aadconnect-feature-automatic-upgrade.md) võib lugeda uuele versioonile.
- Lisada [kohandatud](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)Scheduleri mõned cmdlet-käsud.

## <a name="111190"></a>1.1.119.0
Välja: märts 2016

**Fikseeritud küsimustes:**

- Tehtud kindel Express installi ei saa kasutada Windows Server 2008 (eel-R2) pärast parooli sünkroonimist ei toetata selle operatsioonisüsteemi.
- DirSync täiendamist kohandatud filtri konfiguratsiooni ei tööta ootuspäraselt.
- Uuem väljaanne täiendamisel ja pole konfiguratsioonis muudatusi, siis ei saa ajastada täielik importimine ja sünkroonimine.

## <a name="111100"></a>1.1.110.0
Välja antud: 2016 veebruar

**Fikseeritud küsimustes:**

- Kui install ei ole **C:\Program failide** vaikekausta täiendamist varasemates versioonides ei tööta.
- Kui installite ja valimise, **alustage sünkroonimist protsessi …** viisardi installimise lõpus, käivitades uuesti installimise viisard ei võimalda ajasti.
- Ajasti ei tööta oodatud viisil serverites kui kuupäev ja kellaaeg vormingus ei ole U.S.-en. Blokeerib ka `Get-ADSyncScheduler` õige aeg tagastamiseks.
- Kui installisite mõni varasem versioon Azure'i AD-ühenduse ADFS-i valik Logi sisse ja täiendamine, ei saa käivitada installimise viisard uuesti.

## <a name="111050"></a>1.1.105.0
Välja antud: 2016 veebruar

**Uued funktsioonid:**

- [Automaatse versioonitäienduse](active-directory-aadconnect-feature-automatic-upgrade.md) funktsioon Express sätted klientidele.
- Üldadministraator, kasutades MFA ja PIM installiviisardis tugi.
    - Peate esmalt lubama oma puhverserveri ka https://secure.aadcdn.microsoftonline-p.com-liikluse lubamiseks, kui kasutate MFA.
    - Peate https://secure.aadcdn.microsoftonline-p.com lisamine oma usaldusväärsete saitide loendisse jaoks MFA õigesti töötada.
- Luba kasutaja sisselogimise meetod algse installimise järel.
- Luba [domeeni ja OU filtreerimine](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) installiviisardis. See võimaldab ühenduse metsade, kus kõik domeenid on saadaval.
- [Ajasti](active-directory-aadconnectsync-feature-scheduler.md) on sisseehitatud sync engine.

**Esiletõstetud eelvaatest ga funktsioonid.**

- [Seadme tagasikirjutusega](active-directory-aadconnect-feature-device-writeback.md).
- [Directory laiendid](active-directory-aadconnectsync-feature-directory-extensions.md).

**Uue eelvaade funktsioonid.**

- Uus vaikevorming sünkroonida tsükkeldiagramm intervall on 30 minutit. Varem 3 tundi kõik varasemates versioonides. Lisab toe [ajasti](active-directory-aadconnectsync-feature-scheduler.md) käitumise muutmiseks.

**Fikseeritud küsimustes:**

- Lehel kinnitamine DNS-i domeene ei tunne alati ära domeenid.
- Domeeni administraatori identimisteave konfigureerimisel ADFS-i puhul kuvatavaid juhiseid.
- Asutusesisene AD kontod neid ei tuvasta installimise viisard kui domeeni DNS-i erinevate puu kui ka juurdomeeni asub.

## <a name="1091310"></a>1.0.9131.0
Välja antud: 2015 detsember

**Fikseeritud küsimustes:**

- Parooli sünkroonimise ei pruugi paroolide AD DS, kuid töötab muutmisel, kui määrate parooli.
- Kui teil on puhverserverit, installi- või un ajal võib nurjuda autentimise Azure AD versioonitäienduse lehel konfiguratsioon.
- Alates eelmisest versioonist, Azure'i AD-ühenduse täieliku värskendamine nurjub SQL serveri, kui te ei ole SA SQL.
- SQL Serveri värskendamine eelmisest versioonist, Azure'i AD-ühenduse Interneti kaudu kuvatakse tõrge "Ei saa ADSync SQL-i andmebaasile juurde pääseda".

## <a name="1091250"></a>1.0.9125.0
Välja antud: 2015 November

**Uued funktsioonid:**

- ADFS-i Azure AD usalduskeskuse abil saate konfigureerida.
- Saate värskendada Active Directory skeem ja taastada sünkroonimine reeglid.
- Saate keelata sünkroonimine reegel.
- Saate määratleda uute literaalmärgid sünkroonimine reeglis "AuthoritativeNull".

**Uue eelvaade funktsioonid.**

- [Azure'i AD-ühenduse seisund sünkroonimist](active-directory-aadconnect-health-sync.md).
- [Azure'i AD domeeniteenused](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) parooli sünkroonimise tugi.

**Uue toetatud stsenaarium:**

- Toetab mitu asutusesisese Exchange'i organisatsiooni. Lisateabe saamiseks vaadake [hübriidjuurutuste koos mitme Active Directory kogumit](https://technet.microsoft.com/library/jj873754.aspx) .

**Fikseeritud küsimustes:**

- Parooli sünkroonimise probleemid:
    - Objekti teisaldamine out ulatus – ulatus ei ole oma parooli sünkroonitud. See hõmab OU ja atribuut filtreerimine.
    - Valides uue OU kaasata sünkroonis täielik parooli sünkroonimise ei vaja.
    - Keelatud kasutaja lubamisel parooli ei sünkrooni.
    - Parool uuesti järjekord on lõpmatu ja eelmise kuni 5000 objektide pensionile on eemaldatud.
    - [Täiustatud tõrkeotsing](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Ei saa ühendust Active Directory Windows Server 2016 mets otstarbekas tase.
- Ei saa muuta rühma kasutatakse jaotises filtreerimine pärast esialgse installida.
- Uue kasutajaprofiili enam iga kasutaja parooli muutmine parooli tagasikirjutusega PivotTable-liigendtabelid lubatud teha server Azure'i AD-ühenduse luua.
- Ei saa kasutada pikk täisarv väärtuste sünkroonis reeglid otsinguulatuste.
- Märkeruut "seadme tagasikirjutusega" endiselt keelatud, kui seal on kättesaamatu domeenikontrollerid.

## <a name="1086670"></a>1.0.8667.0
Välja antud: 2015 August

**Uued funktsioonid:**

- Azure'i AD-ühenduse installimise viisard on nüüd lokaliseeritud Windows Server kõigi keelte.
- Lisatud tugi konto avada Azure AD parooli halduse kasutamisel.

**Fikseeritud küsimustes:**

- Azure'i AD-ühendus installimise viisard krahh, kui teise kasutaja ei lahene, installimine, mitte installi esmalt käivitanud isikule.
- Kui eelmine uninstall ja desinstallida Azure'i AD-ühenduse sünkroonimine puhtalt Azure'i AD-ühenduse nurjub, ei ole võimalik uuesti installima.
- Ei saa installida Azure AD-ühenduse kasutamise Express installi, kui kasutajal pole mets juurdomeeni või mitte-ingliskeelsed versiooni Active Directory kasutatakse.
- Kui Active Directory kasutajakonto FQDN ei lahene, kuvatakse eksitavat tõrketeade "Kinnita skeemiga nurjus".
- Konto, mida kasutatakse Active Directory konnektor muutmisel väljaspool viisardi viisard ei õnnestu järgnevate jookseb.
- Mõnikord ei Azure'i AD-ühendus domeenikontrolleri installida.
- Ei saa lubamine ja keelamine "Lavastus mode", kui lisatud laiend atribuute.
- Parooli tagasikirjutusega nurjub mõnes konfiguratsiooni Active Directory konnektor halb parool.
- DirSync ei saa täiendada, kui dn kasutatakse atribuutide filtreerimine.
- Liigse CPU hõivatus parooli lähtestamine kasutamisel.

**Funktsioonid, eemaldatud eelvaade.**

- Eelvaate funktsioon [kasutaja tagasikirjutusega](active-directory-aadconnect-feature-preview.md#user-writeback) ajutiselt eemaldatud eelvaade klientide tagasiside põhjal. See uuesti lisatakse hiljem kui me on adresseeritud esitatud tagasiside.

## <a name="1086410"></a>1.0.8641.0
Välja: juuni 2015

**Azure'i AD-ühenduse algset versiooni.**

Muudetud nimi Azure AD sünkroonimine: Azure'i AD-ühenduse.

**Uued funktsioonid:**

- [Sätete Expressi](./connect/active-directory-aadconnect-get-started-express.md) installimine
- Saate [konfigureerida ADFS-i](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Saate [uuendada DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Vältida juhuslikku kustutab](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Kasutusele [lavastus režiim](active-directory-aadconnectsync-operations.md#staging-mode)

**Uue eelvaade funktsioonid.**

- [Kasutaja tagasikirjutusega](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Rühma tagasikirjutusega](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Seadme tagasikirjutusega](active-directory-aadconnect-feature-device-writeback.md)
- [Directory laiendid](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Välja: mai 2015

**Uue nõue:**

- Azure'i AD Sünkrooni kohe nõuab .net Frameworki 4.5.1 versiooni installimiseks.

**Fikseeritud küsimustes:**

- Parooli tagasikirjutusega: Azure'i AD nurjub tõrkega servicebus Ühenduvus.

## <a name="104910413"></a>1.0.491.0413
Välja: aprill 2015

**Fikseeritud probleemi lahenduse ja täiustuse:**

- Active Directory konnektor protsessi kustutab õigesti kui prügikasti on lubatud ja mets on mitu domeeni.
- Imporditoimingute jõudlust on täiustatud Azure Active Directory konnektor.
- Kui rühm on ületanud liikmelisuse limiit (vaikimisi limiit on seatud 50k objektid), Azure Active Directory rühma kustutatud. Uus käitumine on jäävad rühma, tõrge Expression.Error ja uute liikmete muutusteta eksporditakse.
- Uue objekti ei saa ette valmistatud, kui etapiviisilise Kustuta koos sama DN on juba olemas, konnektori ruumis.
- Mõned objektid on märgitud sünkroonitakse delta sünkroonimise ajal, kuigi ei muutu etapiviisilise objekti.
- Sunnib parooli sünkroonimise eemaldatakse ka näiteks Põhiliselt loendis.
- CSExportAnalyzer on mõned objektid riigid probleeme.

**Uued funktsioonid:**

- Ühenduse saate nüüd serveriga "Tahes" objekti tüübi MV.

## <a name="104850222"></a>1.0.485.0222
Välja antud: 2015 veebruar

**Täiustused.**

- Täiustatud impordi jõudlus.

**Fikseeritud küsimustes:**

- Parooli sünkroonimise kiitusega kasutatav atribuut filtreerimine cloudFiltered atribuut. Filtreeritud objekte ei saa enam parooli sünkroonimise ulatuses.
- Harvaesinevate olukordades, kus topoloogia oli väga palju domeenikontrollereid, parooli sünkroonimine ei tööta.
- Azure'i AD/Intune on lubanud "On peatatud-server" importimisel: Azure'i AD Connector pärast seadme haldamine.
- Liitumine võõrkeelsed turvalisus põhisumma (FSPs) sama mets mitu domeeni põhjustab mitmetähenduslik ühendus tõrge.

## <a name="104751202"></a>1.0.475.1202
Välja antud: 2014 detsember

**Uued funktsioonid:**

- See on nüüd toetatud parooli sünkroonimise atribuudi alusel filtreerimine teha. Lisateavet leiate teemast [parooli sünkroonimise, filtreerimine](active-directory-aadconnectsync-configure-filtering.md).
- Atribuut vaevuste-ExternalDirectoryObjectID kirjutatakse tagasi AD. See lisab tugi Office 365 rakenduste kasutamine OAuth2 juurde nii võrgus ja kohapealsed postkastid Exchange'i hübriidjuurutuse.

**Fikseeritud versioonitäienduse küsimustes:**

- Funktsiooni sisselogimisabimees uuem versioon on serveris saadaval.
- Kohandatud Installitee kasutati installida Azure AD sünkroonimine.
- Mõne sobimatu kohandatud Liitu kriteeriumi blokeerib uuele versioonile.

**Muud parandused:**

- Fikseeritud Office Pro Plusi malle.
- Fikseeritud installiprobleemide põhjustatud kasutajate nimed, mis algavad sidekriipsu.
- Fikseeritud sourceAnchor säte kaotada, kui installi viisardit teist korda.
- Fikseeritud ETW jälitus jaoks parooli sünkroonimine

## <a name="104701023"></a>1.0.470.1023
Välja antud: 2014 oktoober

**Uued funktsioonid:**

- Parooli sünkroonimine mitme asutusesisene AD Azure AD.
- Kõigi Windows Server keelte lokaliseeritud installi UI.

**Täiendamine AADSync 1.0-GA**

Kui teil on juba installitud Azure AD sünkroonimine on täiendavad sammhaaval, tuleb teil teha juhul, kui olete muutnud out box sünkroonimise reeglid. Pärast versiooniks on 1.0.470.1023 vabastage reeglid, mida olete muutnud dubleeritakse sünkroonimine. Iga muudetud sünkroonimise reegli tehke järgmist.

- Otsige üles sünkroonimine reegel on muudetud ja pange kirja soovitud muudatused.
- Sünkroonimise reegli kustutamine
- Leidke uus sünkroonimine reegel loodud Azure AD sünkroonimine ja muudatused uuesti rakendada.

**AD konto õigused**

AD konto peab olema antud saama lugeda parooli hashes AD täiendavaid õigusi. Õiguste andmiseks nimega "Imitatsiooniga Directory muudatused" ja "Imitatsiooniga Directory kõik muutused". Mõlemad õigused on nõutavad lugeda parooli hashes

## <a name="104190911"></a>1.0.419.0911
Välja antud: 2014 mai

**Azure AD sünkroonimine algset versiooni.**

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
