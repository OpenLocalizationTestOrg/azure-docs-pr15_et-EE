<properties
   pageTitle="Azure'i AD-ühendus sünkroonimine: töötoimingute ja kasutuse | Microsoft Azure'i"
   description="Selles teemas kirjeldatakse töötoimingute Azure'i AD-ühenduse sünkroonimine ja kuidas ette valmistada opsüsteem selle komponendi jaoks."
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure'i AD-ühendus sünkroonimine: töötoimingute ja arvesse võtta
Selles teemas eesmärk on töötoimingute Azure'i AD-ühenduse sünkroonimise kirjeldamiseks.

## <a name="staging-mode"></a>Lavastus režiim
Lavastus režiimi saab kasutada mitut stsenaariumid, sh:

-   Kõrge-saadavus.
-   Testimine ja Juurutage uus konfiguratsioonimuudatuste.
-   Lisada uus server ja vana eemaldada.

Serveris lavastus režiimis, saate muuta konfiguratsiooni ja eelvaate muudatused enne, kui teete server aktiivne. Lisaks võimaldab täielik impordi- ja kinnitamaks, et kõik muudatused oodatakse enne nende muudatuste üheks tootmiskeskkonnast täieliku sünkroonimise käivitamiseks.

Installimise ajal saate valida **režiimi lavastus**olema serveri. See toiming teeb server importimine ja sünkroonimine aktiivne, kuid see ei tööta, mis tahes eksport. Serveris lavastus režiimis ei tööta parooli sünkroonimise või parooli tagasikirjutusega, isegi siis, kui olete valinud nende funktsioonide installimise ajal. Kui keelate lavastus režiimis, server alustab eksportimist, võimaldab parooli sünkroonimise ja lubab parooli tagasikirjutusega.

Sünkroonimise service manager abil saate jõustada endiselt eksport.

Muudatuste saadud Active Directory ja Azure AD jätkuvalt serveris lavastus režiimis. See on alati viimaste muudatuste kohta ja väga kiiresti saavad võtta koopia üle teise serverisse ülesanded. Kui teete konfiguratsioonimuudatuste teie peamine serveriga, on teie kohustusi sama muudatuste tegemiseks serveri lavastus režiim.

Neile, teadmised vanemate sünkroonimine tehnoloogiate lavastus režiimi on erinev, kuna server ei oma SQL-andmebaasi. See arhitektuur võimaldab režiimi lavastus asuda mõnes muus andmekeskuse serveri.

### <a name="verify-the-configuration-of-a-server"></a>Serveri konfiguratsiooni kontrollimine
See meetod rakendamiseks tehke järgmist.

1. [Ettevalmistamine](#prepare)
2. [Importimine ja sünkroonimine](#import-and-synchronize)
3. [Kontrollige](#verify)
4. [Üleminek active server](#switch-active-server)

#### <a name="prepare"></a>Ettevalmistamine

1. Installige Azure'i AD-ühenduse, valige **lavastus režiimi**ja **käivitage sünkroonimine** installimise viisardi viimasel lehel valimise. Selle režiimi saate sync engine käsitsi käivitamine.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Logige välja ja logige sisse ja valige menüü start kaudu **Sünkroonimise teenuse**.

#### <a name="import-and-synchronize"></a>Importimine ja sünkroonimine

1. Valige **konnektorid**ja valige esimene konnektori tüüp **Active Directory domeeniteenused**. Klõpsake käsku **Käivita**, valige **täielik import**ja **OK**. Seda tüüpi kõik konnektorid tehke järgmist.
2. Valige konnektor tüüp **Azure Active Directory (Microsoft)**. Klõpsake käsku **Käivita**, valige **täielik import**ja **OK**.
3. Veenduge, et vahekaardil ühendused valituks. Iga tüüpi **Active Directory domeeniteenused**Connectori, klõpsake käsku **Käivita**, valige **Delta sünkroonimise**ja **OK**.
4. Valige konnektor tüüp **Azure Active Directory (Microsoft)**. Klõpsake käsku **Käivita**, valige **Delta sünkroonimise**ja **OK**.

Teil on nüüd etapiviisilise ekspordi muutub Azure AD ja kohapealsete AD (kui kasutate Exchange'i hübriidjuurutuse). Järgmiste juhiste abil saab kontrolli, mis on enne tegelikult kataloogid ekspordi muutmine.

#### <a name="verify"></a>Kontrollige

1. Käivitage käsu viip ja minge`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Käivitage:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Sünkroonimise teenuse leiate saatja nimi. See on nimi, mis on sarnane "contoso.com – AAD" Azure AD jaoks.
3. Käivitage:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Teil on % temp % nimega export.csv, et tutvuda Microsoft Exceli faili. See fail sisaldab kõik muudatused, mis on eksportida.
5. Tehke vajalikud muudatused andmete või konfigureerimine ja käivitage järgmist uuesti (importimine ja sünkroonimine ja kinnitamine) kuni muudatusi, mis on eksportida.

**Faili export.csv mõistmine**

Enamik fail on iseenesestmõistetavad. Mõned lühendite mõista sisu:

- OMODT – objekti muutmise tüüp. Näitab, kui tasemel objekti toiming on lisamine, värskendamine või Kustuta.
- AMODT – atribuudi muutmine tüüp. Näitab, kas atribuut tasemel toiming on mõni lisamine, värskendamine või Kustuta.

Kui atribuudi väärtus on mitme väärtusega, kuvatakse iga ei muuda. Ainult väärtuste lisada ja eemaldada arv on nähtav.

#### <a name="switch-active-server"></a>Active server vahetamine

1. Praegu aktiivne server, Azure AD nii, et see on eksportimise server (DirSync/FIM/Azure AD sünkroonimine) välja lülitada või sätestatud lavastus režiimi (Azure'i AD-ühenduse).
2. Installi viisardit kasutada serveri **lavastus režiimi** ja keelake **lavastus režiim**.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Katastroofiabi
Kujunduse rakendamiseks osa on kavandamine, mida teha, kui on katastroof, kus te kaotate sünkroonimine serveriga. On erinevaid mudeleid kasutada ja millist kasutada sõltub mitmest tegurist, sh:

-   Mis on teie hälbe ei ole võimalik teha muudatused objektide Azure AD soovitud tööseisakute ajal?
-   Kui kasutate parooli sünkroonimise, kas kasutajad nõus, endale vana parool kasutamine Azure AD juhuks, kui nad seda muuta kohapealse?
-   Kas teil on sõltuv reaalajas toiminguid, nagu parooli tagasikirjutusega?

Vastused neile küsimustele ja ettevõtte poliitika, sõltuvalt üks järgmistest strateegiaid saab rakendada:

-   Vajadusel taastada.
-   On kasutamata valige server, teadaolevad peatuspaikadena **režiim**.
-   Kasutage virtuaalmasinates.

Kui te ei kasuta sisseehitatud SQL Expressi andmebaasi, siis peaksite ka tutvuma jaotises [SQL suure saadavuse](#sql-high-availability) .

### <a name="rebuild-when-needed"></a>Vajadusel taastada
Toimiv strateegia on kavandamine serveri taastada, kui vaja. Tavaliselt installimist sünkroonimine mootor ja tehke algse impordi- ja sünkroonimise saate lõpetatud mõne tunni jooksul. Kui vaba server pole saadaval, on võimalik kasutada ajutiselt domeenikontrolleri majutada sync engine.

Sünkroonimine engine server pole laoseisu mis tahes objektide kohta nii, et andmebaasi saab uuesti Active Directory ja Azure AD andmetest. **SourceAnchor** atribuuti kasutatakse objektide kohapealse ja pilveteenuse kaudu liituda. Kui te taastada serveris olemasoleva objektide kohapealse ja pilveteenuse, siis sync engine vastab nende objektide koos uuesti uuesti sisse. Asjad, mida soovite dokumendi salvestada on konfiguratsiooni server, nt filtreerimine ja sünkroonimise reeglid tehtud muudatused. Neid kohandatud konfiguratsioone peab uuesti enne alustamist sünkroonimine.

### <a name="have-a-spare-standby-server---staging-mode"></a>On kasutamata valige server - lavastus režiim
Kui teil on veel keskkonnas, ja seejärel valige üks või mitu serverid on soovitatav on kasutada. Installimise ajal saate lubada serveri olema **lavastus režiim**.

Täpsema teabe saamiseks vt [lavastus režiim](#staging-mode).

### <a name="use-virtual-machines"></a>Kasutage virtuaalmasinates
Levinud ja toetatud meetod on virtuaalse masina sync engine läbiviimisel. Selle hosti on probleeme, saate migreerida engine sünkroonimine serveriga pildi teise serverisse.

### <a name="sql-high-availability"></a>SQL suure kättesaadavus
Kui te ei kasuta SQL Server Express kaasneva Azure'i AD-ühenduse, siis kõrge kättesaadavus SQL serveri korral tuleks kaaluda ka. Toetatud ainult kõrge-saadavus lahendus on rühmitamise SQL-i. Mittetoetatavad lahenduste hulka kuuluvad peegeldus ja alati.

## <a name="next-steps"></a>Järgmised sammud

**Teemade ülevaade**  

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)  
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)  
