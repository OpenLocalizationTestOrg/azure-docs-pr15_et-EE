<properties
    pageTitle="Azure Active Directory auditilogi API viide | Microsoft Azure'i"
    description="Kuidas alustada Azure Active Directory auditilogi API-ga"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory auditilogi API viide

Selles teemas on osa teemade kohta aruannete API Azure Active Directory kogum.  
Azure'i AD aruandlus annab teile API, mis lubab teil juurde pääseda auditiandmete kood või seotud tööriistade abil.
Selles teemas on teile **audit API**teave.

Järgmistest teemadest.

- [Auditilogide](active-directory-reporting-azure-portal.md#audit-logs) kontseptuaalne Lisateavet
- Lisateavet aruannete API [Azure Active Directory aruannete API töötamise alustamine](active-directory-reporting-api-getting-started.md) .

Küsimused, probleemid või tagasiside, pöörduge [AAD aruandlus aitab](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Kes pääseb andmeid?

- Kasutajate turvalisuse administraator või turvalisus lugeja roll

- Üldadministraatorid

- Mis tahes rakendus, mis sisaldab Luba juurdepääs API (rakenduse saab luba häälestus ainult globaalne administraator õiguste alusel)



## <a name="prerequisites"></a>Eeltingimused

Aruande aruannete API kaudu juurdepääsemiseks peavad teil olema:

- [Azure Active Directory tasuta või parem edition](active-directory-editions.md)

- Lõpetatud [eeltingimused Azure AD, aruannete API juurdepääsu](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Juurdepääs API

Kas pääsete juurde selle API [Graphi Exploreri](https://graphexplorer2.cloudapp.net) kaudu või programmiliselt näiteks PowerShelli kaudu. PowerShelli õigesti tõlgendada OData filter süntaks AAD Graphi ülejäänud kõnede selleks, kasutage funktsiooni backtick (ehk: graavis) märgi "escape" märk $. Backtick märgi toimib [PowerShelli 's Paomärgi](https://technet.microsoft.com/library/hh847755.aspx), mis võimaldab teha sõnasõnaline tõlgendamise märk $ ja vältida segadust see PowerShelli muutuja nimena PowerShelli (st: $filter).

Selles teemas keskendutakse Graphi Explorer. Näiteks PowerShelli näha selle [PowerShelli skripti](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API lõpp-punkti


Pääsete juurde selle API abil järgmist URI.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Ei ole piiratud tagastatud Azure AD auditilogi API (abil OData lehekülgjaotuse) kirjete arvu.
Aruannete andmete säilitamise piirangud, tutvuge [Aruandlus Säilituspoliitikad](active-directory-reporting-retention.md).

Selle kõne tagastab andmed pakettidena. Iga on kuni 1000 kirjed.  
Järgmise paketi kirjete saamiseks kasutage järgmise lingi. Tagastatud kirjete esimeste skiptoken teavet saada. Jäta luba tulemi lõpus seatakse.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Toetatud filtrid

Te saate API tagastatud kirjete arvu piiritlemiseks vormi filtri sisse helistada.  
Sisselogimise API seotud andmeid, on toetatud järgmised filtrid:

- **$top =\<arv kirjeid nii, et tagastada\> ** – tagastatud kirjete arvu piiritlemiseks. See on kallis toiming. Ärge kasutage seda filtrit kui soovite tuhandeliste objektide tagastamiseks.     
- **$filter =\<oma filtri lause\> ** – määramiseks alusel toetatud filtriväljade, teid huvitab kirjete tüüp



## <a name="supported-filter-fields-and-operators"></a>Toetatud filtriväljade ja tehtemärgid

Määramaks, et teid huvitab kirjete tüüp, saate koostada filter lause, mis sisaldab ühte või kombinatsiooni filter järgmised väljad:

- [activityDate](#activitydate) - määratleb kuupäeva või kuupäevavahemikku
- [activityType](#activitytype) - määratleb tegevuse tüüp
- [tegevuste](#activity) - määratleb tegevuse string  
- [näitleja/nimi](#actorname) - määratleb osaleja soovitud osaleja nime kujul
- [näitleja/ObjectId väärtuse](#actorobjectid) - määratleb osaleja vormil soovitud osaleja-ID-d   
- [näitleja/upn](#actorupn) - määratleb osaleja kujul Ameerika põhimõte kasutajanimi (UPN) 
- [target/nimi](#targetname) - määratleb sihtkohas soovitud osaleja nime kujul
- [target/ObjectId väärtuse](#targetobjectid) - määratleb sihtkohas vormil soovitud sihtrakenduse ID  
- [target/upn](#targetupn) - määratleb osaleja kujul Ameerika põhimõte kasutajanimi (UPN)   




----------

### <a name="activitydate"></a>activityDate

**Toetatud tehtemärgid**: eq, ge, le, gt, lt

**Näide**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Märkmete**:

kuupäeva ja kellaaja peaks olema UTC-vormingus

----------

### <a name="activitytype"></a>activityType

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=activityType eq 'User'  

**Märkmete**:

tõstutundlik

----------

### <a name="activity"></a>tegevus

**Toetatud tehtemärgid**: eq, sisaldab startsWith

**Näide**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Märkmete**:

tõstutundlik

----------

### <a name="actorname"></a>näitleja/nimi

**Toetatud tehtemärgid**: eq, sisaldab startsWith

**Näide**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Märkmete**:

väiketähed

    

----------
### <a name="actorobjectid"></a>näitleja/ObjectId väärtuse

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Target/nimi

**Toetatud tehtemärgid**: eq, sisaldab startsWith

**Näide**:

    $filter=targets/any(t: t/name eq 'some name')   

**Märkmete**:

Väiketähed

----------

### <a name="targetupn"></a>Target/UPN-i

**Toetatud tehtemärgid**: eq startsWith

**Näide**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Märkmete**:

- Väiketähed
- Peate lisama täieliku nimeruumi, kui päringu Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>Target/ObjectId väärtuse

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>näitleja/UPN-i

**Toetatud tehtemärgid**: eq startsWith

**Näide**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Märkmete**:

- Väiketähed 
- Peate lisama täieliku nimeruumi, kui päringu Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Järgmised sammud

- Kas soovite vaadata näiteid filtreeritud süsteemi tegevuste jaoks? Lugege teemat [Azure Active Directory audit API näidised](active-directory-reporting-api-audit-samples.md).

- Kas soovite rohkem teada Azure AD, aruannete API? Teemast [Azure Active Directory aruannete API töötamise alustamine](active-directory-reporting-api-getting-started.md).