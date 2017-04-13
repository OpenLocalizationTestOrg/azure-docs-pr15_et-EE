<properties
   pageTitle="Azure'i AD-ühendus: Uuendamine varasemalt versioonilt | Microsoft Azure'i"
   description="Selgitatakse, saate uusimat versiooni Azure Active Directory ühendust luua, sh versiooniuuenduse ja kiik migreerimise versioonile mitmel viisil."
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure'i AD-ühendus: Versioonile mõnelt varasemalt versioonilt Viimane
Selles teemas kirjeldatakse neid erinevaid viise, saate uuendada oma Azure'i AD-ühenduse installi uusim versioon. Soovitame, et hoiate enda praeguse versioonidega, Azure'i AD-ühenduse. [Migreerimise kiik](#swing-migration) kirjeldatud juhiseid kasutatakse ka siis, kui teete olulisi konfiguratsiooni muutmine.

Kui soovite uuendada DirSync, lugege teemat [uuendada Azure AD sünkroonimistööriist (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) asemel.

On mõni muu strateegiad versioonile Azure'i AD-ühenduse.

Meetod | Kirjeldus
--- | ---
[Automaatne versioonitäiendus](active-directory-aadconnect-feature-automatic-upgrade.md) | Klientidele, kes on kiire installimisega, see on kõige lihtsam viis.
[Versiooniuuenduse](#in-place-upgrade) | Kui teil on ühe server, võtke kasutusele installi-asukohta samas serveris.
[Migreerimise kiik](#swing-migration) | Kahe serverid, saate ühe uue väljaande või konfiguratsiooni serverid ette valmistada ja muuta active server, kui olete valmis.

Vajalikke õigusi, leiate teemast [täiendamine vajalikud õigused](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>Versiooniuuenduse
Kohapealse täienduse töötab teisaldamine Azure AD sünkroonimine või Azure'i AD-ühenduse kaudu. See ei toimi DirSync või lahenduse FIM + Azure AD konnektor.

See meetod on eelistatud, kui teil on ühe server ja alla umbes 100 000 objektid. Kui kõik muudatused out box sünkroonimine reegleid, täielik import ja täieliku sünkroonimise tekkida pärast uuele versioonile. See tagab, et uus konfiguratsioon on rakendatud kõik olemasolevad objektid süsteemi. Sync engine ulatus võib selleks kuluda mõni tund sõltuvalt objektide arvu. Tavaline delta sünkroonimise ajasti, vaikimisi iga 30 minuti järel peatatakse, kuid endiselt parooli sünkroonimine. Peaksite tegema selle versiooniuuenduse ajal koosneva nädalavahetuse. Kui uus Azure'i AD-ühenduse väljaandesse out box konfiguratsiooni muudatusi, siis tavaline delta impordi/sünkroonimise hakatakse selle asemel.  
![Versiooniuuenduse](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Kui teie tehtud muudatused väljal-, sünkroonimise reeglid, seatakse need vaikimisi konfiguratsiooni uuendada tagasi. Veenduge, et teie konfiguratsiooni säilitatakse vahel täienduste, veenduge, et on tehtud muudatusi, nagu on kirjeldatud [head tavad vaikimisi konfiguratsiooni muutmine](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Migreerimise kiik
Kui teil on keerukas juurutamise või väga palju objekte, võib olla otstarbekas on versiooniuuenduse reaalajas süsteemi teha. See võib mõne võtta mitu päeva ja selle aja jooksul töödeldakse delta muudatusi. See meetod kasutatakse ka siis, kui plaanite oma konfiguratsioon oluliste muudatuste tegemiseks ja te soovite proovida enne, kui need on lükata pilveteenusesse.

Need stsenaariumid soovitatav meetod on kasutada kiik migreerimist. Peate (vähemalt) kaks serverid üks aktiivne ja üks lavastus server. Laadi aktiivse tootmise vastutab active server (ühtlase sinine read järgmisel pildil). Lavastus server (kriipsjoontega lilla read järgmisel pildil) on valmis uus versioon või konfigureerimine ja täielikult valmis, kui see server on tehtud aktiivne. Eelmise active server, nüüd installitud, konfigureerimine ja vana versioon on tehtud lavastus server ja uuendada.

Kaks serverid saate kasutada erinevaid versioone. Näiteks active server plaanite ajagraafiku saate kasutada Azure AD sünkroonimine ja saate uue lavastus server Azure'i AD-ühenduse. Kui kiik migreerimise abil saate luua uue konfiguratsiooni on hea mõte on kaks serverites sama versioon.  
![Lavastus server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Märkus: On märgitud, et mõned kliendid soovi on kolm ja neli serverid selle stsenaariumi. Kui lavastus serveri versioon on täiendatud, ei ole varukoopia Serveri korral [katastroofiabi](active-directory-aadconnectsync-operations.md#disaster-recovery). Kolm või neli serverid, üks rida primaarne/puhkerežiimis olev serverid uue versiooniga saate valmis, tagamiseks on alati lavastus server valmis üle võtta.

Teisaldada Azure AD sünkroonimine või lahenduse FIM + Azure AD konnektor töötab ka järgmist. Neid juhiseid ei toimi DirSync, kuid samas Kiik (nimetatakse ka paralleelse juurutamine) migreerimise meetod juhistega DirSync leiate [Täiendamine Azure Active Directory sünkroonimine (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Migreerimise juhiseid kiik

1. Kui kasutate nii Azure'i AD-ühenduse ja teha ainult konfiguratsiooni muutus kavandamine, veenduge, et teie active server ja lavastus server on mõlemad kasutades sama versiooni. Mis on hõlpsam võrrelda hiljem erinevused. Kui täiendate Azure AD sünkroonimine, on need serverid erinevaid versioone. Kui teostate vanema versiooni Azure'i AD-ühenduse, on mõistlik alustada kahe serverid, kasutades sama versiooni, kuid ei ole vaja.
2. Kui olete teinud mõned kohandatud konfiguratsioon ja teie lavastus server ei ole seda, järgige jaotises [kohandatud Otsingukonfiguratsiooni aktiivne lavastus serveri teisaldamine](#move-custom-configuration-from-active-to-staging-server).
3. Kui täiendate mõni varasem versioon Azure'i AD-ühenduse, uuendada lavastus serveri uusima versiooni. Kui teil on teisaldada: Azure'i AD sünkroonimine, installige Azure'i AD-ühenduse lavastus serverisse.
4. Lubage sync engine käivitada täielik import ja täieliku sünkroonimise lavastus serverisse.
5. Veenduge, et uue konfiguratsiooni põhjustanud ootamatud muudatused üle juhised lõigus **kinnitamine** kasutamine [kinnitamine serveri konfiguratsiooni](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Kui midagi pole õige ootuspäraselt, käivitada impordi- ja sünkroonimine ja kontrollige kuni andmete sobib. Need juhised leiate lingitud teema.
6. Aktiveerige lavastus tuleb active server server. See on selle Viimane toiming **aktiveerimine active server** [server konfiguratsiooni kinnitamine](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Kui täiendate Azure'i AD-ühenduse, võtke kasutusele serveri nüüd lavastus režiimis, et uusim versioon. Järgige samu juhiseid nagu enne andmed ja konfiguratsiooni üle läinud. Kui läksite Azure AD sünkroonimine, saate nüüd välja lülitada ja vana serverisse eemaldada.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Kohandatud Otsingukonfiguratsiooni aktiivne teisaldamiseks lavastus server
Kui olete muutnud konfiguratsiooni active server, peate veenduge, et sama muudatused rakendatakse lavastus serveriga.

Olete loonud kohandatud sünkroonimine reegleid saab teisaldada PowerShelli abil. Muud muudatused samal viisil operatsioonisüsteemides nii ei rakendata ja ei viida.

Te peate veenduge, et asi on konfigureeritud ühtemoodi nii:

- Sama metsade ühendus.
- Mis tahes domeeni ja OU filtreerimine.
- Valikulised funktsioonid samad, nagu parooli sünkroonimise ja parooli tagasikirjutusega.

**Teisalda sünkroonimise reeglid**  
Kohandatud sünkroonimise reegli teisaldamiseks tehke järgmist.

1. Avage oma active server **Sünkroonimise reeglid redaktor** .
2. Valige oma kohandatud reegli. Klõpsake nuppu **ekspordi**. Kuvatakse tabeliveergude notepad aken. Ajutiste failide salvestamine PS1 laiendiga. See muudab PowerShelli skripti. Kopeerige fail ps1 lavastus serveriga.  
![Sünkroonimise reegli eksportimine](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Konnektor GUID erineb lavastus serveris ja seda tuleb muuta. GUID saamiseks **sünkroonimise reeglite**redaktori, valige üks järgmistest out box reeglitest tähistav sama ühendatud süsteemi ja klõpsake nuppu **ekspordi**. Asendage GUID PS1 failis GUID lavastus serverist.
4. Käivitage PowerShelli käsuviibas PS1 fail. See loob kohandatud sünkroonimine reegel lavastus serveris.
5. Kui teil on mitu kohandatud reeglid, korrake kõigi kohandatud reeglid.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
