<properties
    pageTitle="Azure Active Directory domeeniteenused: Juurutamise stsenaariumide | Microsoft Azure'i"
    description="Azure'i AD domeeni teenuste juurutamine stsenaariumid"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Stsenaariumid juurutamise ja kasutamise juhtudel
Selles jaotises vaatame paar stsenaariumid ja kasutamise juhtudel kasu domeeniteenused Azure Active Directory (AD).

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Kaitstud manustamine Azure'i virtuaalmasinates
Azure Active Directory domeeniteenused abil saate hallata oma Azure'i virtuaalmasinates sujuvalt. Azure'i virtuaalmasinates saate liita hallatava domeeni, võimaldades teil logige sisse oma ettevõtte AD mandaadi abil. Seda moodust aitab vältida mandaadi hilisemaid probleeme näiteks säilitamine kohaliku administraatorikonto iga teie Azure'i virtuaalmasinates haldus.

Serveri virtuaalmasinates, mis on ühendatud hallatava domeeni saab ka hallata ja turvatud rühmapoliitika abil. Saate rakendada oma Azure'i virtuaalmasinates nõutavate võrdlusandmeid ja lukustades need vastavalt ettevõtte turvalisuse juhised. Näiteks saate piirata tüüpi rakendusi, mida saab käivitada need virtuaalmasinates rühma poliitikavõimalused haldus.

![Sujuv manustamine Azure'i virtuaalmasinates](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Kui serverid ja muude taristu jõuab kasutuselt kõrvaldatud, liigub Contoso rakendustele praegu majutatud kohapealne pilveteenusesse. Oma praeguse IT standard mandaatide serverite hosting ettevõtte rakendused tuleb domeeni ühendatud ja hallatavate rühmapoliitika abil. Contoso on IT-administraator eelistab domeeni Liitu virtuaalmasinates juurutatud Azure, Administreerimine hõlbustamiseks. Selle tulemusena administraatoritele ja kasutajatele saate sisse logida ettevõtte mandaadi abil. Samal ajal saab konfigureerida masinad täitmiseks nõutavate võrdlusandmeid rühmapoliitika abil. Contoso ei soovi juurutada, jälgimine ja haldamine domeenikontrollerid Azure Azure'i virtuaalmasinates tagamiseks on. Seetõttu Azure AD domeeniteenused on väga sobivad selle kasutamise puhul.

**Juurutamise märkmed**

Võtke arvesse järgmisi olulisi fakte stsenaariumi juurutamise jaoks.

- Azure'i AD domeeni teenuste hallatavate domeenide pakuvad ühe kindla OU (Organisatsiooniüksus) struktuuri vaikimisi. Kõik domeeni ühendatud seadmed asuvad ühe kindla OU. Samas võite luua kohandatud organisatsiooniüksused.

- Azure'i AD domeeniteenused toetab lihtsa rühmapoliitika sisseehitatud GPO iga ja kasutajate arvutitesse kujul ümbriste. Ei saa suunata GP OU/osakonna, teha WMI filtreerimine või luua kohandatud lubatu.

- Azure'i AD domeeniteenused toetab base AD arvuti objekt skeemi. Te ei saa arvuti objekti skeemi laiendamine.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lifti-ja shift kohapealse rakendus, mis kasutab LDAP siduda autentimise Azure'i taristu teenused

![LDAP siduda](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso on kohapealse rakendus, mis on ostetud mõni ISV aastaid tagasi. Rakendus on praegu hooldus režiimis, Kontrollimissertifikaadid ja paluda rakenduse muudatused on liiga kallis contoso. See rakendus on veebipõhine frontend, mis kogub kasutajatunnust veebivormi abil ja seejärel autendib kasutajad oma LDAP siduda ettevõtte Active Directory abil. Contoso soovite migreerida selle rakenduse Azure'i taristu teenused. On soovitatav, et rakendus töötab nõudmata muudatused on. Lisaks peaks olema autentida olemasoleva ettevõtte mandaadi abil saavad kasutajad ja ilma koolitada kasutajad teisiti tegutsema. Teisisõnu lõppkasutajal peaks olema tunne, kui rakendus töötab ja migreerimise peaks olema läbipaistev neile.

**Juurutamise märkmed**

Võtke arvesse järgmisi olulisi fakte stsenaariumi juurutamise jaoks.

- Veenduge, et rakendus ei ole vaja muuta või kirjutamiseks kataloogi. LDAP kirjutusõiguse hallatavate domeenide Azure'i AD domeeni teenuste ei toetata.

- Paroolide otse vastu hallatava domeeni ei saa muuta. Lõppkasutajad saaksid oma parooli muuta kas Azure AD Iseteeninduslik parooli muutmine süsteemi abil või kohapealse kataloogi vastu. Need muudatused on hallatava domeeni automaatselt sünkroonitud ja saadaval.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Lifti shift kohapealse rakendus, mis kasutab LDAP lugeda kataloogi Azure'i taristu teenused avamiseks
Contoso on kohapealse joon-, äri (LOB) rakendus, mis on välja töötatud peaaegu kümne tagasi. See rakendus on directory arvestada ja on loodud töötama koos Windows Server AD. Rakendus kasutab LDAP (Lightweight Directory Access Protocol) / atribuute kasutajate kohta lugemiseks Active Directoryst. Rakenduse atribuute muuta või selle kausta teisiti kirjutada. Contoso soovite migreerida selle rakenduse Azure'i taristu teenused ja pensionile vanade kohapealse riistvara praegu hosting see rakendus. Rakendus ei saa ümber tänapäevane directory API-d, nt ülejäänud vastavalt Azure AD Graph API kasutamine. Seetõttu on lifti shift suvand soovitud millega rakenduse migreeritud pilves, ilma koodi muutmine või ümberkirjutamine rakenduse käivitamiseks.

**Juurutamise märkmed**

Võtke arvesse järgmisi olulisi fakte stsenaariumi juurutamise jaoks.

- Veenduge, et rakendus ei ole vaja muuta või kirjutamiseks kataloogi. LDAP kirjutusõiguse hallatavate domeenide Azure'i AD domeeni teenuste ei toetata.

- Veenduge, et rakendus ei pea kohandatud/laiendatud Active Directory skeem. Azure'i AD domeeniteenused skeemi laiendid pole toetatud.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migreerida kohapealse teenuse või daemon rakenduse Azure'i taristu teenused
Mõned rakendused koosnevad mitmetasandiline, kus kasutatakse ühte peab täitma autenditud kõned taustväärtus taseme, nt andmebaasi taseme. Kasutage-juhul kasutatakse tavaliselt Active Directory kontod. Saate lifti shift Azure'i taristu teenused ja kasutage Azure AD Domeeniteenustesse need rakendused identiteedi jaoks teha. Saate kasutada sama teenuse konto, mis on sünkroonitud kohapealse kataloogi Azure AD. Vaheldumisi, saate kõigepealt looma kohandatud OU ja seejärel luua selle OU juurutada sellise rakendused eraldi teenusekonto.

![Teenusekonto abil WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso on lähtuvalt ehitatud vault rakendus, mis sisaldab web vastendamisel, SQL server ja taustväärtus FTP server. Windowsi integreeritud autentimist teenuse kontode kasutatakse autentida web esiosa FTP-serverisse. Veebi on seadistatud kontona teenuse käivitamine. Taustväärtus server on konfigureeritud lubada juurdepääs veebi kaudu teenuse konto. Contoso eelistab olla kasutada domeeni domeenikontrolleri virtuaalse masina pilveteenusesse teisaldada selle rakenduse Azure'i taristu teenused. Contoso on IT-administraator saab juurutada servereid hosting veebi ja SQL server Azure'i virtuaalmasinates FTP-serveriga. Need masinad on ühendatud on Azure AD domeeniteenused hallatavate Domeen. Siis nad saavad kasutada sama oma kohapealse kataloogi rakenduse autentimise eesmärgil. Selle teenuse konto on sünkroonitud Azure AD domeeniteenused hallatava domeeni ja saab kasutada.

**Juurutamise märkmed**

Võtke arvesse järgmisi olulisi fakte stsenaariumi juurutamise jaoks.

- Veenduge, et rakendus kasutab autentimise kasutajanime ja parooli. Azure'i AD domeeniteenused ei toeta sertifikaat/kiipkaardi vastavalt autentimist.

- Paroolide otse vastu hallatava domeeni ei saa muuta. Lõppkasutajad saaksid oma parooli muuta kas Azure AD Iseteeninduslik parooli muutmine süsteemi abil või kohapealse kataloogi vastu. Need muudatused on hallatava domeeni automaatselt sünkroonitud ja saadaval.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp võimaldab Contoso's administraatori domeeni ühendatud saidikogumi loomiseks. See funktsioon võimaldab serveri rakendused kätte Azure RemoteApp käivitamiseks domeeni ühendatud arvutites ja muud ressursid Windowsi integreeritud autentimist kasutades juurdepääsu. Contoso saate kasutada Azure AD domeeniteenused pakuvad hallatava domeeni kasutavad Azure RemoteApp domeeni ühendatud saidikogumid.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Selle stsenaariumi juurutamise kohta leiate lisateavet artiklist Remote'i töölaua teenuste ajaveebi pealkirjaga [lifti shift oma töökoormus Azure RemoteApp ja Azure AD domeeniteenused](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
