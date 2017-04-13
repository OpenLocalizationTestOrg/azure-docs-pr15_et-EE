<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: funktsioonide viide | Microsoft Azure'i"
    description="Viite deklaratiivseid ettevalmistamise avaldiste Azure'i AD-ühenduse sünkroonimine."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure'i AD-ühendus sünkroonimine: funktsioonide juhend

Azure'i AD-ühenduse, kasutatakse funktsioonide sünkroonimisel atribuudi väärtust muuta.  
Funktsioonide süntaks on väljendatud järgmises vormingus:  
`<output type> FunctionName(<input type> <position name>, ..)`

Kui funktsiooni on ülekoormatud ja aktsepteerib mitme lauseõpetuse, on loetletud kõik lubatud lauseõpetuse.  
Funktsioonid on tugevalt tipitud ning nad veenduge, et tüüp möödunud vasteid dokumenteeritud tüüp.  
Kui tüüp ei sobi, tõrge Expression.error.

Tüübid on väljendatud järgmise süntaksi abil:

- **prügikasti** – kahendarvuks
- **bool** – kahendmuutuja
- **dt** – UTC kuupäev/kellaaeg
- **loetelu** – teadaolevad konstantide loendamine
- **exp** – avaldis, mis annavad tulemiks on kahendväärtus peaks
- **mvbin** – Multi-Valued kahendarvu
- **mvstr** – Multi-Valued String
- **mvref** – Multi-Valued viide
- **num** – arv
- **ref** – viide
- **str** – String
- **var** – variandi (peaaegu) mis tahes tüüpi
- **kehtetu** – ei tagasta väärtus

Funktsioonide tüübid **mvbin**, **mvstr**ja **mvref** ainult saate töötada mitme väärtusega atribuute. Funktsioonide **prügikasti**, **str**ja **ref** töötada nii ühe väärtusega ja mitme väärtusega atribuute.

## <a name="functions-reference"></a>Funktsioonide juhend

Funktsioonide loendi | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Teisendamine** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Kuupäev / kellaaeg** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Nüüd](#now)
[NumFromDate](#numfromdate) |  
**Kataloog** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Hindamine** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Matemaatika** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**Mitme väärtusega** |  
[Sisaldab](#contains) | [Count](#count) | [Üksuse](#item) | [ItemOrNull](#itemornull)
[Liitumine](#join) | [RemoveDuplicates](#removeduplicates) | [Tükeldatud](#split) |
**Programmi voog** |  
[Tõrge](#error) | [FUNKTSIOONI IIF](#iif)  | [Üleminek](#switch)
**Teksti** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [Funktsioon](#lcase)
[Vasakule](#left) | [Funktsioon LEN](#len) | [Funktsioonid LTrim](#ltrim) | [Mid](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Asendamine](#replace)
[ReplaceChars](#replacechars) | [Paremale](#right) | [RTrim](#rtrim) | [Funktsiooni TRIM](#trim)
[UCase](#ucase) | [Wordi](#word)

----------
### <a name="bitand"></a>BitAnd

**Kirjeldus:**  
Funktsioon BitAnd komplekti määratud bittide väärtus.

**Süntaks:**  
`num BitAnd(num value1, num value2)`

- Väärtus1; väärtus2: arvväärtused, mis peaks olema emafunktsioonivõtmed koos

**Märkused:**  
See funktsioon teisendab nii parameetrite soovitud binaarkuju ja määrab veidi.

- 0 – kui üks või mõlemad vastavate bittide *maski* ja *lipp* on 0
- 1 - kui mõlemad vastavate bittide on 1.

Teisisõnu, tagastab see 0 alati, välja arvatud juhul, kui mõlemad parameetrite vastavate bittide on 1.

**Näide:**  
`BitAnd(&HF, &HF7)`  
Tagastab väärtuse 7, kuna see väärtus väärtuseks kuueteistkümnendarvu "F" ja "F7".

----------
### <a name="bitor"></a>BitOr

**Kirjeldus:**  
Funktsioon BitOr komplekti määratud bittide väärtus.

**Süntaks:**  
`num BitOr(num value1, num value2)`

- Väärtus1; väärtus2: arvväärtused, mis peaks olema OR'ed koos

**Märkused:**  
See funktsioon teisendab nii parameetrite soovitud binaarkuju ja määrab veidi 1, kui üks või mõlemad vastavate bittide maski ja lipp on 1 ja 0, kui mõlemad vastavate bittide on 0. Teisisõnu, tagastab see 1 alati, välja arvatud juhul, kui mõlemad parameetrite vastavate bittide 0.

----------
### <a name="cbool"></a>CBool

**Kirjeldus:**  
Tagastab funktsioon CBool kahendväärtus, mis on hinnatud avaldise põhjal

**Süntaks:**  
`bool CBool(exp Expression)`

**Märkused:**  
Kui avaldise väärtuseks on nullist erinev väärtus, siis CBool tagastab väärtuse True, veel see tagastab väärtuse False.

**Näide:**  
`CBool([attrib1] = [attrib2])`  

Tagastab väärtuse True, kui mõlemad atribuudid on sama väärtusega.

----------
### <a name="cdate"></a>CDate

**Kirjeldus:**  
Stringi eel, tagastab funktsioon CDate UTC DateTime. Kuupäeva ja kellaaja pole kohalikke atribuut tüüp sünkroonitud, kuid mõned funktsioonid kasutab.

**Süntaks:**  
`dt CDate(str value)`

- Väärtus: String kuupäeva, kellaaja, ja soovi korral ajavöönd

**Märkused:**  
Tagastatud stringi on alati UTC-vormingus.

**Näide:**  
`CDate([employeeStartTime])`  
Annab vastuseks kuupäeva ja kellaaja alusel töötaja alguskellaaeg

`CDate("2013-01-10 4:00 PM -8")`  
Tagastab kuupäeva ja kellaaja, mis tähistab "2013-01-11 12:00 AM"

----------
### <a name="cguid"></a>CGuid

**Kirjeldus:**  
Funktsiooni CGuid Teisendab tekstistringi kujutis GUID selle binaarkuju.

**Süntaks:**  
`bin CGuid(str GUID)`

- Stringi vormindatakse see muster: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx või {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Sisaldab

**Kirjeldus:**  
Funktsiooni sisaldab leiab stringi sees mitme väärtusega atribuut

**Süntaks:**  
`num Contains (mvstring attribute, str search)`-tõstutundlik  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-tõstutundlik

- atribuut: mitme väärtusega atribuut otsimiseks.
- Otsing: stringi otsimiseks atribuuti.
- Casetype: CaseInsensitive või CaseSensitive.

Tagastab registri mitme väärtusega atribuut kui stringi ei leitud. tagastatakse 0 kui stringi ei leita.

**Märkused:**  
Mitme väärtusega string atribuudid, otsing leiab kasutatavat võrdluslaadi väärtused.  
Viide atribuudid, otsida stringi peab täpselt vastama väärtuseks pidada vaste.

**Näide:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Kui atribuut proxyAddresses on esmasele meiliaadressile (tähistatud suur "SMTP:"), tagastab funktsioon Kasutajaatribuut atribuut, muidu tagastatakse tõrge.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Kirjeldus:**  
Funktsiooni ConvertFromBase64 teisendab määratud base64 kodeeritud väärtuse tavaline string.

**Süntaks:**  
`str ConvertFromBase64(str source)`-eeldab Unicode'i kodeeringus  
`str ConvertFromBase64(str source, enum Encoding)`

- Allikas: Base64 kodeeringuga stringi  
- Kodeering: Unicode'i, ASCII, UTF8

**Näide**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Mõlemad näited tagastavad "*Tere, maailm!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Kirjeldus:**  
ConvertFromUTF8Hex funktsiooni teisendab määratud väärtus UTF8 Hex kodeeringuga stringi.

**Süntaks:**  
`str ConvertFromUTF8Hex(str source)`

- Allikas: UTF8 2-baidine encoded nõelamine

**Märkused:**  
See funktsioon ja ConvertFromBase64([],UTF8) sisse, et tulemus on sõbralik DN atribuudi erinevus.  
Seda vormingut kasutatakse DN Azure Active Directory abil.

**Näide:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Annab vastuseks "*Tere, maailm!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Kirjeldus:**  
Funktsiooni ConvertToBase64 teisendab stringi Unicode'i base64 string.  
Teisendab väärtuse massiivi täisarvude selle võrdväärse stringi esituse kodeeritud base – 64 numbritega.

**Süntaks:**  
`str ConvertToBase64(str source)`

**Näide:**  
`ConvertToBase64("Hello world!")`  
Annab vastuseks "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Kirjeldus:**  
Funktsiooni ConvertToUTF8Hex teisendab stringi UTF8 Hex kodeeritud väärtus.

**Süntaks:**  
`str ConvertToUTF8Hex(str source)`

**Märkused:**  
See funktsioon vorming kasutab Azure Active Directory DN atribuut Vorming.

**Näide:**  
`ConvertToUTF8Hex("Hello world!")`  
Annab vastuseks 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Count

**Kirjeldus:**  
Funktsiooni Count tagastab elementide arv mitme väärtusega atribuut

**Süntaks:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Kirjeldus:**  
Funktsiooni CNum võtab stringi ja tagastab arvulise andmetüübi.

**Süntaks:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Kirjeldus:**  
Teisendab tekstistringi viide atribuut

**Süntaks:**  
`ref CRef(str value)`

**Näide:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Kirjeldus:**  
Funktsiooni CStr teisendab andmetüüp string.

**Süntaks:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- väärtus: saab arvulise väärtuse, viite atribuut või kahendmuutuja.

**Näide:**  
`CStr([dn])`  
Tagastada "cn = Joe, AV = contoso, AV = com"

----------
### <a name="dateadd"></a>DateAdd

**Kirjeldus:**  
Tagastab kuupäeva, mis sisaldab kuupäeva, mis on lisatud teatud ajavahemiku.

**Süntaks:**  
`dt DateAdd(str interval, num value, dt date)`

- intervall: avaldis, mis ajavahemiku lisamiseks on String. Stringi peab olema üks järgmistest väärtustest:
 - aaaa aasta
 - k kvartali
 - m kuu
 - aasta päeva y
 - d päeva
 - w Weekday
 - WW nädal
 - h tund
 - n minuti
 - s teise
- väärtus: arvu üksused, mille soovite lisada. See võib olla (ei saanud kuupäevade tulevikus) positiivne või negatiivne (ei saanud varem kuupäevad).
- kuupäev: kuupäev ja kellaaeg, mis tähistab kuupäeva, millele on lisatud intervalli.

**Näide:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Liidab 3 kuud ja tagastab soovitud kuupäeva ja kellaaja, mis tähistab "2001-04-01".

----------
### <a name="datefromnum"></a>DateFromNum

**Kirjeldus:**  
DateFromNum funktsioon teisendab väärtuse reklaami kuupäeva vormingus kuupäeva ja kellaaja tüüp.

**Süntaks:**  
`dt DateFromNum(num value)`

**Näide:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Annab vastuseks kuupäeva ja kellaaja tähistav 2012-01-01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Kirjeldus:**  
Funktsioon DNComponent tagastab väärtuse määratud DN komponendi läheb vasakult.

**Süntaks:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: viide atribuudi tõlgendamine
- ComponentNumber: Komponent DN tagastamiseks

**Näide:**  
`DNComponent([dn],1)`  
Kui dn on "cn = Joe ou =...,", tagastab ta Joe

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Kirjeldus:**  
Funktsioon DNComponentRev tagastab väärtuse määratud DN komponendi läheb paremale (end).

**Süntaks:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: viide atribuudi tõlgendamine
- ComponentNumber - komponendi DN tagastamiseks
- Suvandid: Näiteks Põhiliselt – kõik komponendid ignoreerida "AV ="

**Näide:**  
Kui dn on "cn = Joe ou = Atlanta, ou = GA, ou = US, AV = contoso, AV = com" siis  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Mõlemad tulemuseks US.

----------
### <a name="error"></a>Tõrge

**Kirjeldus:**  
Integraalse veafunktsiooni kasutatakse kohandatud tõrge tagastamiseks.

**Süntaks:**  
`void Error(str ErrorMessage)`

**Näide:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Kui atribuut account_name puudub, põhjustada tõrke objekti.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Kirjeldus:**  
Funktsiooni EscapeDNComponent võtab üks osa DN ja pääseb see, et seda ei saa esitada LDAP.

**Süntaks:**  
`str EscapeDNComponent(str value)`

**Näide:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Saab kontrollida, kas objekti saab luua ka Lightweight directory isegi juhul, kui atribuuti displayName on märgid, mis tuleb tuleb seda vältida LDAP sisse.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Kirjeldus:**  
Funktsioon FormatDateTime kasutatakse vormindamine soovitud kuupäeva ja kellaaja stringi määratud vormingu abil

**Süntaks:**  
`str FormatDateTime(dt value, str format)`

- väärtus: väärtus kuupäev ja kellaaeg vormingus
- vorming: string, mis tähistab teisendamiseks vorming.

**Märkused:**  
Võimalikud väärtused vorming, leiate siit: [Kasutaja määratletud kuupäeva/kellaaja vormingud (funktsioon Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Näide:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007 / 12 / 25" tulemusi.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Võib põhjustada "20140905081453.0Z"

----------
### <a name="guid"></a>GUID

**Kirjeldus:**  
Funktsiooni GUID genereeritud juhusliku uue GUID

**Süntaks:**  
`str GUID()`

----------
### <a name="iif"></a>FUNKTSIOONI IIF

**Kirjeldus:**  
IIF-funktsiooni, mis tagastab ühe võimalikud väärtused määratud tingimuse alusel.

**Süntaks:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- tingimus: mis tahes väärtus või avaldis, mida saab hinnata väärtustega true või false.
- valueIfTrue: kui tingimus annab tulemiks true, tagastatud väärtus.
- valueIfFalse: kui tingimus annab väärtuseks false, tagastatud väärtus.

**Näide:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Kui kasutaja on Interneti, tagastab pseudonüüm "t-", lisatakse see veel alguses kasutaja tagastab kasutaja alias on.

----------
### <a name="instr"></a>InStr

**Kirjeldus:**  
Funktsioon InStr leiab esimese esinemise alamstringi stringis

**Süntaks:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringikontroll: otsida stringi
- stringivaste: leida string
- Alustamine: alates asukoha leidmiseks on alamstringi
- võrrelge: vbTextCompare või vbBinaryCompare

**Märkused:**  
Tagastab asukohta, kus on alamstringi leiti või 0, kui ei leitud.

**Näide:**  
`InStr("The quick brown fox","quick")`  
Evalues 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Hindab 7

----------
### <a name="instrrev"></a>InStrRev

**Kirjeldus:**  
Funktsioon InStrRev otsib viimase esinemiskorra alamstringi string

**Süntaks:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringikontroll: otsida stringi
- stringivaste: leida string
- Alustamine: alates asukoha leidmiseks on alamstringi
- võrrelge: vbTextCompare või vbBinaryCompare

**Märkused:**  
Tagastab asukohta, kus on alamstringi leiti või 0, kui ei leitud.

**Näide:**  
`InStrRev("abbcdbbbef","bb")`  
Annab vastuseks 7

----------
### <a name="isbitset"></a>IsBitSet

**Kirjeldus:**  
Funktsiooni IsBitSet katsete kui natuke on määratud või ei

**Süntaks:**  
`bool IsBitSet(num value, num flag)`

- väärtus: arvulise väärtuse, mis on evaluated.flag: arvulise väärtuse, mis on hinnatav bit

**Näide:**  
`IsBitSet(&HF,4)`  
Annab vastuseks väärtuse True, kuna "4" bit on määratud kuueteistkümnendarvu väärtuse "F"

----------
### <a name="isdate"></a>IsDate

**Kirjeldus:**  
Kui avaldis võib olla hindab tüübina kuupäev ja kellaaeg, siis funktsioon IsDate väärtuseks True.

**Süntaks:**  
`bool IsDate(var Expression)`

**Märkused:**  
Kui CDate() võib olla eduka kasutada.

----------
### <a name="isempty"></a>IsEmpty

**Kirjeldus:**  
Kui atribuut on olemas või MV, kuid annab tulemuseks tühja stringi, siis funktsioon IsEmpty annab tulemuseks True.

**Süntaks:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Kirjeldus:**  
Kui stringi võib teisendada GUID, siis funktsiooni IsGuid hinnata väärtusega true.

**Süntaks:**  
`bool IsGuid(str GUID)`

**Märkused:**  
Pärast nende mustrite ühte stringi on määratletud GUID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx või {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Kui CGuid() võib olla eduka kasutada.

**Näide:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Kui soovitud StrAttribute on GUID vormingus, tagastavad on binaarkuju, muidu tagastab Null.

----------
### <a name="isnull"></a>IsNull

**Kirjeldus:**  
Kui avaldise tulem on null, tagastab väärtuse true funktsioon IsNull.

**Süntaks:**  
`bool IsNull(var Expression)`

**Märkused:**  
Atribuut puudumisel väljendub atribuudile, Null.

**Näide:**  
`IsNull([displayName])`  
Annab vastuseks väärtuse True, kui atribuut pole olemas või MV.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Kirjeldus:**  
Kui avaldis on null või tühi string, siis IsNullOrEmpty, tagastab funktsioon true.

**Süntaks:**  
`bool IsNullOrEmpty(var Expression)`

**Märkused:**  
Atribuudi, see oleks väärtuseks True, kui atribuut puudub või on olemas, kuid on tühi string.  
Selle funktsiooni pöördväärtus nimega IsPresent.

**Näide:**  
`IsNullOrEmpty([displayName])`  
Annab vastuseks väärtuse True, kui atribuut puudub või on tühi string või MV.

----------
### <a name="isnumeric"></a>IsNumeric

**Kirjeldus:**  
Funktsioon IsNumeric Tagastab kahendväärtuse, mis näitab, kas arv tüübiks saate hinnatav avaldis.

**Süntaks:**  
`bool IsNumeric(var Expression)`

**Märkused:**  
Kui CNum() võib olla eduka avaldist sõeluda kasutada.

----------
### <a name="isstring"></a>IsString

**Kirjeldus:**  
Kui stringi tüüpi saate hinnatav avaldis, siis funktsiooni IsString tulemiks True.

**Süntaks:**  
`bool IsString(var expression)`

**Märkused:**  
Kui CStr() võib olla eduka avaldist sõeluda kasutada.

----------
### <a name="ispresent"></a>IsPresent

**Kirjeldus:**  
Kui avaldis on string, mis ei ole tühi ja pole tühi, tagastab väärtuse true IsPresent funktsioon.

**Süntaks:**  
`bool IsPresent(var expression)`

**Märkused:**  
Selle funktsiooni pöördväärtus nimega IsNullOrEmpty.

**Näide:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Üksuse

**Kirjeldus:**  
Mitme väärtusega string/atribuut, tagastab funktsioon üksuse ühe üksuse.

**Süntaks:**  
`var Item(mvstr attribute, num index)`

- atribuut: mitme väärtusega atribuut
- registri: index üksuse mitme väärtusega string.

**Märkused:**  
Üksuse funktsioon on kasulik koos sisaldab funktsiooni, kuna üksuse mitme väärtusega atribuut, tagastab funktsioon viimase registri.

Kui indeks on vahemikust väljas, põhjustab tõrge.

**Näide:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Tagastab peamine meiliaadress.

----------
### <a name="itemornull"></a>ItemOrNull

**Kirjeldus:**  
Mitme väärtusega string/atribuut, tagastab funktsioon ItemOrNull ühe üksuse.

**Süntaks:**  
`var ItemOrNull(mvstr attribute, num index)`

- atribuut: mitme väärtusega atribuut
- registri: index üksuse mitme väärtusega string.

**Märkused:**  
Funktsiooni ItemOrNull on kasulik koos sisaldab funktsiooni, kuna viimasel funktsioon tagastab registri üksuse mitme väärtusega atribuut.

Kui indeks on vahemikust väljas, tagastab nullväärtust.

----------
### <a name="join"></a>Liitumine

**Kirjeldus:**  
Funktsiooni Liitu võtab mitme väärtusega stringi ja tagastab ühe väärtusega stringi koos määratud iga üksuse vahele järgmine eraldaja.

**Süntaks:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- atribuut: mitme väärtusega atribuut, mis sisaldavad stringide ühendatakse.
- eraldaja: iga stringi, tagastatud stringi alamstringide eraldamiseks kasutatav. Kui välja jäetud, tühikumärki ("") kasutatakse. Kui eraldaja on nullpikkusega string ("") või mitte midagi, kõigi loendiüksuste on liitsõnumeid koos pole eraldajaid.

**Märkused**  
Liitumine ja tükeldatud funktsioonid on võrdse. Funktsiooni Liitu võtab stringide massiivi ja ühendab need eraldaja string, kasutades ühe stringi tagastamiseks. Funktsiooni tükeldatud võtab stringi ja vahel see operaator veebisaidil eraldaja, massiivi stringide tagastamiseks. Siiski on oluline erinevus, et Liitu saate mis tahes eraldaja string stringid concatenate, tükeldatud saab eraldada ainult stringide kasutamine üksikmärki eraldaja.

**Näide:**  
`Join([proxyAddresses],",")`  
Tagastada:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>Funktsioon

**Kirjeldus:**  
Funktsioon LCase teisendab kõik märkide stringi madalama juhtumi.

**Süntaks:**  
`str LCase(str value)`

**Näide:**  
`LCase("TeSt")`  
Annab vastuseks "test".

----------
### <a name="left"></a>Vasakule

**Kirjeldus:**  
Funktsioon Left tagastab määratud arvu märke stringi vasakult.

**Süntaks:**  
`str Left(str string, num NumChars)`

- stringi: kaudu märkide tagastamine stringi
- NumChars: arvu märkide arvu tuvastamise tulu stringi algusest (vasakul)

**Märkused:**  
String, mis sisaldab esimest numChars märkide stringi:

- Kui numChars = 0, tagastab tühja stringi.
- Kui numChars < 0, tagastab Sisestuskeel string.
- Kui string on null, tagastab tühja stringi.

Kui string sisaldab vähem märke, kui arv on määratud numChars, tagastatakse string, mis on identne string (mis on, mis sisaldab kõik märgid parameetri 1).

**Näide:**  
`Left("John Doe", 3)`  
Tagastab "Joh".

----------
### <a name="len"></a>Funktsioon LEN

**Kirjeldus:**  
Funktsioon Len tagastab märkide arvu tekstistringis oleva.

**Süntaks:**  
`num Len(str value)`

**Näide:**  
`Len("John Doe")`  
Annab vastuseks 8

----------
### <a name="ltrim"></a>Funktsioonid LTrim

**Kirjeldus:**  
Funktsioonid LTrim funktsiooni eemaldab Algustühikute tühja stringi.

**Süntaks:**  
`str LTrim(str value)`

**Näide:**  
`LTrim(" Test ")`  
Annab vastuseks "Test"

----------
### <a name="mid"></a>Mid

**Kirjeldus:**  
Funktsioon Mid tagastab määratud kohast stringis määratud arvu märke.

**Süntaks:**  
`str Mid(str string, num start, num NumChars)`

- stringi: kaudu märkide tagastamine stringi
- Alustamine: tuvastamise algus arvu paigutuse stringi tagastamiseks märke
- NumChars: arvu märkide arvu tuvastamise tulu asukoht stringis

**Märkused:**  
Saatja numChars märki, alates positsioonist alustamiseks stringi.  
String, mis sisaldab numChars märkide stringi algusest asukoht:

- Kui numChars = 0, tagastab tühja stringi.
- Kui numChars < 0, tagastab Sisestuskeel stringi.
- Kui Alustamine > pikkust, stringi tagastamiseks Sisestuskeel string.
- Kui alustada < = 0, Sisestuskeel stringi.
- Kui string on null, tagastab tühja stringi.

Kui ei ole numChar märkide stringi algusest asukohta, nii palju allesjäänud märkide võimalikult tagastatakse.

**Näide:**  
`Mid("John Doe", 3, 5)`  
Tagastab "hn ära".

`Mid("John Doe", 6, 999)`  
Annab vastuseks "Mägi"

----------
### <a name="now"></a>Nüüd

**Kirjeldus:**  
Funktsioon Now tagastab soovitud kuupäeva ja kellaaja määramine praeguse kuupäeva ja kellaaja, vastavalt teie arvuti süsteemikuupäeva ja -kellaaja.

**Süntaks:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Kirjeldus:**  
Funktsiooni NumFromDate tagastab kuupäeva reklaami kuupäeva vormingus.

**Süntaks:**  
`num NumFromDate(dt value)`

**Näide:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Annab vastuseks 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Kirjeldus:**  
PadLeft funktsioon vasak-padjad on esitatud täidise märgi pikkuse piiritlemine string.

**Süntaks:**  
`str PadLeft(str string, num length, str padCharacter)`

- string: string valimisklahvistikku.
- pikkus: täisarvu soovitud stringi pikkus.
- padCharacter: string koosneb üksikmärki märgina valimisklahvistiku kasutamiseks

**Märkused:**

- Kui stringi pikkus on väiksem kui pikkus, siis padCharacter korduvalt lisatakse (vasakul) stringi pikkus kuni võrdne pikkus alguseni.
- PadCharacter võib olla tühikuid, kuid ei saa see olla tühiväärtus.
- Kui stringi pikkus on võrdne või suurem kui pikkus, tagastatakse stringi ei muutunud.
- Kui stringi pikkus, mis on suurem kui või võrdne pikkus, tagastatakse stringina identne stringi.
- Kui stringi pikkus on väiksem kui pikkus, siis soovitud pikkuses uue stringi tagastatakse tegeliku koos mõne padCharacter sisaldav string.
- Kui string on null, tagastab funktsioon tühja stringi.

**Näide:**  
`PadLeft("User", 10, "0")`  
Tagastab "000000User".

----------
### <a name="padright"></a>PadRight

**Kirjeldus:**  
PadRight funktsioon paremale-padjad on esitatud täidise märgi pikkuse piiritlemine stringi.

**Süntaks:**  
`str PadRight(str string, num length, str padCharacter)`

- string: string valimisklahvistikku.
- pikkus: täisarvu soovitud stringi pikkus.
- padCharacter: string koosneb üksikmärki märgina valimisklahvistiku kasutamiseks

**Märkused:**

- Kui stringi pikkus on väiksem kui pikkus, siis padCharacter korduvalt lisatakse lõppu (paremal) stringi seni, kuni see on võrdne pikkus pikkus.
- padCharacter võib olla tühikuid, kuid ei saa see olla tühiväärtus.
- Kui stringi pikkus on võrdne või suurem kui pikkus, tagastatakse stringi ei muutunud.
- Kui stringi pikkus, mis on suurem kui või võrdne pikkus, tagastatakse identne stringi string.
- Kui stringi pikkus on väiksem kui pikkus, siis soovitud pikkuses uue stringi tagastatakse tegeliku koos mõne padCharacter sisaldav string.
- Kui string on null, tagastab funktsioon tühja stringi.

**Näide:**  
`PadRight("User", 10, "0")`  
Tagastab "User000000".

----------
### <a name="pcase"></a>PCase

**Kirjeldus:**  
Funktsiooni PCase teisendab iga sõna tühikuga eraldatud tekstistringi esimese märgi läbivate suurtähtedega ja kõigi muude klaviatuurilt puuduvate märkide teisendatakse madalama juhtumi.

**Süntaks:**  
`String PCase(string)`

**Märkused:**

- See funktsioon ei paku praegu proper kest sõna, mis on täiesti suurtähed, nt lühend teisendada.

**Näide:**  
`PCase("TEsT")`  
Tagastab "Test".

`PCase(LCase("TEST"))`  
Annab vastuseks "Test"

----------
### <a name="randomnum"></a>RandomNum

**Kirjeldus:**  
Funktsiooni RandomNum annab vastuseks juhusliku arvu määratud intervalli vahel.

**Süntaks:**  
`num RandomNum(num start, num end)`

- Alustamine: arvu tuvastamise alampiir juhusliku väärtuse loomiseks
- Lõpeta: tuvastamise ülempiiri väärtuse juhusliku arvu loomiseks

**Näide:**  
`Random(100,999)`  
Saate tagastada 734.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Kirjeldus:**  
Funktsiooni RemoveDuplicates võtab mitme väärtusega string ja veenduge, et iga väärtuse on kordumatu.

**Süntaks:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Näide:**  
`RemoveDuplicates([proxyAddresses])`  
Tagastab desinfitseeritakse Kasutajaatribuut atribuut, kust on eemaldatud kõik dubleeritud väärtused.

----------
### <a name="replace"></a>Asendamine

**Kirjeldus:**  
Funktsioon Replace asendab kõigi esinemiskordade teise stringi.

**Süntaks:**  
`str Replace(str string, str OldValue, str NewValue)`

- stringi: stringi väärtuste asendamine.
- OldValue: String otsida ja asendada.
- Uusväärtus: String Asenda.

**Märkused:**  
Funktsioon tuvastab järgmised teisiti hüüdnimede:

- \n – uus rida
- \r – tagasijooksu
- \t – menüü

**Näide:**  
`Replace([address],"\r\n",", ")`  
Asendab CRLF koma ja tühikut ja võib põhjustada "One Microsoft Way, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Kirjeldus:**  
Funktsiooni ReplaceChars asendab stringi ReplacePattern leitud märkide kõik juhud.

**Süntaks:**  
`str ReplaceChars(str string, str ReplacePattern)`

- stringi: tekstistringi märkide asendamiseks.
- ReplacePattern: string, mis sisaldab märke asendada sõnastiku.

Vorming on {source1}: {target1}, {source2}: {target2}, {sourceN}, {targetN} kui allikas on märk otsimiseks ja asendamiseks stringi suunata.

**Märkused:**

- Funktsiooni võtab iga esinemiskorra määratletud andmeallikate ja asendab need eesmärgid.
- Allika peab olema täpselt ühe (unicode) märgi.
- Allika ei saa olla tühi või rohkem kui ühe märgi (sõelumine tõrge).
- Sihtkohas võib olla mitu märki, näiteks ö:oe, β:ss.
- Sihtkohas võib olla tühi, mis näitab, et märgi kõrvaldamisel.
- Allika on tõstutundlikud ja peab olema täpne vaste.
- (Koma) ja: (koolon) on reserveeritud märgid ja ei saa asendada selle funktsiooni.
- Tühikute ja muude valge märkide stringi ReplacePattern ignoreeritakse.

**Näide:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Annab vastuseks Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Annab vastuseks "ONeil" ühe rist on määratletud eemaldada.

----------
### <a name="right"></a>Paremale

**Kirjeldus:**  
Funktsioon Right tagastab määratud arvu märke stringi paremal (end).

**Süntaks:**  
`str Right(str string, num NumChars)`

- stringi: kaudu märkide tagastamine stringi
- NumChars: arvu märkide arvu tuvastamise lõppu (paremal) stringi tagastamiseks.

**Märkused:**  
Tagastatakse NumChars märkide stringi Viimane positsiooni.

String, mis sisaldab viimase numChars märkide stringi:

- Kui numChars = 0, tagastab tühja stringi.
- Kui numChars < 0, tagastab Sisestuskeel string.
- Kui string on null, tagastab tühja stringi.

Kui string sisaldab vähem märke, kui arv on määratud NumChars, tagastatakse identne stringi string.

**Näide:**  
`Right("John Doe", 3)`  
Tagastab "Mägi".

----------
### <a name="rtrim"></a>RTrim

**Kirjeldus:**  
Funktsiooni RTrim eemaldab lõputühikud tühja stringi.

**Süntaks:**  
`str RTrim(str value)`

**Näide:**  
`RTrim(" Test ")`  
Tagastab "Test".

----------
### <a name="split"></a>Tükeldatud

**Kirjeldus:**  
Funktsiooni tükeldatud suunab eraldaja eraldatud tekstistring, mis muudab mitme väärtusega stringi.

**Süntaks:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- väärtus: string eraldi märgiga eraldaja.
- eraldaja: ühe märgi eraldajana kasutada.
- piiratud: väärtused, mis võib tagastada arvu ülempiir.

**Näide:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Tagastab 2 elementidega mitme väärtusega stringi abiks Kasutajaatribuut atribuuti.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Kirjeldus:**  
Funktsioon StringFromGuid võtab kahendarvu GUID ja teisendab selle stringi

**Süntaks:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Kirjeldus:**  
Funktsiooni StringFromSid teisendab turvalisuse ID string, mis sisaldab baitide massiivis.

**Süntaks:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Üleminek

**Kirjeldus:**  
Funktsioon Switch kasutatakse hinnatud asjaolude ühe väärtuse tagastamiseks.

**Süntaks:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr: Variant avaldis, mida soovite hinnata.
- väärtus: tagastatakse, kui vastav avaldis on tõene väärtus.

**Märkused:**  
Lüliti funktsioon argumendi loendi koosneb paari avaldiste ja väärtused. Avaldiste hinnatakse vasakult paremale ja tagastatakse väärtus, mis on seotud esimese avaldis väärtuseks True. Kui osade pole õigesti paaris, käitusaja tõrketeade.

Kui Avaldis1 on True, tagastab vahetamise väärtus1. Kui avaldis-1 on False, aga avaldis-2 on True, tagastab aktiveerimine väärtuse 2 jne.

Üleminek tagastab midagi kui:

- Avaldiste pole True.
- Esimene tõene avaldis on vastav väärtus, mis on tühi.

Üleminek hindab kõigi avaldiste puhul, kuigi see tagastab ainult üks neist. Seetõttu jälgige soovimatu pool mõju. Näiteks kui mis tahes avaldis hindamisest jagamise tõrge nulliga, ilmneb tõrge.

Väärtus võib olla ka funktsiooni viga, mis tuleks tagastada kohandatud.

**Näide:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Tagastab mõned suurte linnade soovitud keel, muidu tagastab tõrke.

----------
### <a name="trim"></a>Funktsiooni TRIM

**Kirjeldus:**  
Funktsiooni Trim eemaldab- ja lõputühikud tühja stringi eel.

**Süntaks:**  
`str Trim(str value)`  

**Näide:**  
`Trim(" Test ")`  
Tagastab "Test".

`Trim([proxyAddresses])`  
Eemaldab ja lõputühikud Kasutajaatribuut atribuut iga väärtuse.

----------
### <a name="ucase"></a>UCase

**Kirjeldus:**  
Funktsioon UCase teisendab kõik märkide stringi suurtähed.

**Süntaks:**  
`str UCase(str string)`

**Näide:**  
`UCase("TeSt")`  
Annab vastuseks "TEST".

----------
### <a name="word"></a>Wordi

**Kirjeldus:**  
Wordi funktsioon tagastab sisaldas stringist parameetrite eraldajad kasutamine ja Wordi arvu tagastamiseks kirjeldav sõna.

**Süntaks:**  
`str Word(str string, num WordNumber, str delimiters)`

- stringi: stringi tagastamiseks sõna.
- WordNumber: arvu kindlaks teha, millised Wordi arv peaks tagastama.
- eraldajad: string, mis tähistab delimiter(s), mida tuleks kasutada sõnade tuvastamiseks

**Märkused:**  
Iga märgistring, eraldatud üks eraldajaid märkide stringi on tuvastatud sõnad:

- Kui arv < 1, tagastab tühja stringi.
- Kui string on tühi, tagastab funktsioon tühja stringi.

Kui string sisaldab vähem kui sõnade arv või ei sisalda stringi kõik sõnad, mis on tähistatud eraldajaid, tagastatakse tühi string.

**Näide:**  
`Word("The quick brown fox",3," ")`  
Annab vastuseks "pruun"

`Word("This,string!has&many separators",3,",!&#")`  
Tagastab "on"

## <a name="additional-resources"></a>Lisaressursid

* [Deklaratiivseid ettevalmistamise avaldiste mõistmine](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure'i AD ühenduse sünkroonimine: Kohandamise sünkroonimise suvandid](active-directory-aadconnectsync-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
