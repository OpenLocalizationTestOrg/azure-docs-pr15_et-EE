<properties
    pageTitle="Azure Active Directory sisselogimise tegevuse aruanne API viide | Microsoft Azure'i"
    description="Azure Active Directory sisselogimise tegevuste aruande API tutvustus"
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
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory sisselogimise tegevuse aruanne API viide


Selles teemas on osa teemade kohta aruannete API Azure Active Directory kogum.  
Azure'i AD aruandlus annab teile API, mis võimaldab juurdepääsu sisse aruandeandmete koodi või seotud tööriistade abil.
Selles teemas on teile teavet **sisselogimine tegevuse aruande API**kohta.

Järgmistest teemadest.

- Lisateavet kontseptuaalne [sisselogimise tegevuste](active-directory-reporting-azure-portal.md#sign-in-activities)
- Lisateavet aruannete API [Azure Active Directory aruannete API töötamise alustamine](active-directory-reporting-api-getting-started.md) .

Küsimused, probleemid või tagasiside, pöörduge [AAD aruandlus aitab](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Kes pääseb API andmeid?

- Kasutajate turvalisuse administraator või turvalisus lugeja roll

- Üldadministraatorid

- Mis tahes rakendus, mis sisaldab Luba juurdepääs API (rakenduse saab luba häälestus ainult globaalne administraator õiguste alusel)



## <a name="prerequisites"></a>Eeltingimused

Aruande aruannete API kaudu juurdepääsemiseks peab teil olema:

- [Azure Active Directory Premium P1 või P2 edition](active-directory-editions.md)

- Lõpetatud [eeltingimused Azure AD, aruannete API juurdepääsu](active-directory-reporting-api-prerequisites.md). 


##<a name="accessing-the-api"></a>Juurdepääs API

Kas pääsete juurde selle API [Graphi Exploreri](https://graphexplorer2.cloudapp.net) kaudu või programmiliselt näiteks PowerShelli kaudu. PowerShelli õigesti tõlgendada OData filter süntaks AAD Graphi ülejäänud kõnede selleks, kasutage funktsiooni backtick (ehk: graavis) märgi "escape" märk $. Backtick märgi toimib [PowerShelli 's Paomärgi](https://technet.microsoft.com/library/hh847755.aspx), võimaldades PowerShelli sõnasõnaline tõlgendamine märk $ teha ja vältida segadust see PowerShelli muutuja nimena (st: $filter).

Selles teemas keskendutakse Graphi Explorer. Näiteks PowerShelli näha selle [PowerShelli skripti](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API lõpp-punkti

Pääsete juurde selle API abil Järgmine alus URI.  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Andmete maht, kuna see API on üks miljon tagastas kirjed piiri. 

Selle kõne tagastab andmed pakettidena. Iga paketi on kuni 1000 kirjed.  
Järgmise paketi kirjete saamiseks kasutage järgmise lingi. Tagastatud kirjete esimeste [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) teavet saada. Jäta luba tulemi lõpus seatakse.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Toetatud filtrid

Te saate API tagastatud kirjete arvu piiritlemiseks vormi filtri sisse helistada.  
Sisselogimise API seotud andmeid, on toetatud järgmised filtrid:

- **$top =\<arv kirjeid nii, et tagastada\> ** – tagastatud kirjete arvu piiritlemiseks. See on kallis toiming. Ärge kasutage seda filtrit kui soovite tuhandeliste objektide tagastamiseks.  
- **$filter =\<oma filtri lause\> ** – määramiseks alusel toetatud filtriväljade, teid huvitab kirjete tüüp



## <a name="supported-filter-fields-and-operators"></a>Filtriväljade toetatud ja tehtemärgid

Määramaks, et teid huvitab kirjete tüüp, saate koostada filter lause, mis sisaldab ühte või kombinatsiooni filter järgmised väljad:

- [signinDateTime](#signindatetime) - määratleb kuupäeva või kuupäevavahemikku

- [kasutajanimi](#userid) - määratleb kindla kasutaja vastavalt kasutaja ID-ga.

- [userPrincipalName](#userprincipalname) - määratleb kindla kasutaja vastavalt kasutaja kasutaja turvasubjektinimi (UPN)

- [appId](#appid) - määratleb kindla rakenduse vastavalt rakenduse ID

- [appDisplayName](#appdisplayname) - määratleb kindla rakenduse vastavalt rakenduse kuvatav nimi

- [loginStatus](#loginStatus) - määratleb selle sisselogimise olekut (edu ja tõrge)


> [AZURE.NOTE] Graafik Explorer kasutamisel on teil vaja kasutada iga kirja jaoks õige juhul oma filtriväljade.


Tagastatud andmete ulatuse piiritlemiseks, saate koostada toetatud filtrid ja filtriväljade kombinatsioonid. Näiteks järgmine lause tagastab ülemine 10 kirjete juuli 1st 2016 ja juuli 6 2016 vahel:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Toetatud tehtemärgid**: eq, ge, le, gt, lt

**Näide**:

Kindlale kuupäevale abil

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Kuupäevavahemiku abil    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Märkmete**:

Datetime parameetri peaks olema UTC-vormingus 


----------

### <a name="userid"></a>Kasutajanimi

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Märkmete**:

Kasutajanimi väärtus stringiväärtus



----------

### <a name="userprincipalname"></a>userPrincipalName

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Märkmete**:

UserPrincipalName väärtus stringiväärtus

----------

### <a name="appid"></a>appId

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Märkmete**:

AppId väärtus stringiväärtus

----------


### <a name="appdisplayname"></a>appDisplayName

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Märkmete**:

AppDisplayName väärtus stringiväärtus

----------

### <a name="loginstatus"></a>loginStatus

**Toetatud tehtemärgid**: eq

**Näide**:

    $filter=loginStatus+eq+'1'  


**Märkmete**:

On kaks võimalust selle loginStatus: 0 - edu, 1 - tõrge

----------



## <a name="next-steps"></a>Järgmised sammud

- Kas soovite vaadata näiteid filtreeritud tegevuste jaoks? Lugege teemat [Azure Active Directory sisselogimise tegevuste aruande API näidised](active-directory-reporting-api-sign-in-activity-samples.md).

- Kas soovite rohkem teada Azure AD, aruannete API? Teemast [Azure Active Directory aruannete API töötamise alustamine](active-directory-reporting-api-getting-started.md).