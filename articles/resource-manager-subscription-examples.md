<properties
   pageTitle="Stsenaariumid ja tellimuse juhtimise näited | Microsoft Azure'i"
   description="Antakse ülevaade sellest, kuidas rakendada Azure tellimuse juhtimise Levinumad stsenaariumid."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Azure'i enterprise tellingud rakendamise näited

Selles teemas antakse ülevaade mõne kuidas ettevõte saab rakendada ka [Azure enterprise tellingud](resource-manager-subscription-governance.md)soovitused. Väljamõeldud ettevõte nimega Contoso kasutab illustreerimiseks head tavad levinud stsenaariumi. 

## <a name="background"></a>Tausta

Contoso on kogu maailmas ettevõte, mis pakub pakkumise ahelas lahenduste klientide kõige "Tarkvara kui veel teenus" mudel pakkimisel mudeliga juurutatud kohapealse.  Tekivad kogu maailmas koos märkimisväärset arengut andmekeskuste India, Ameerika Ühendriigid ja Kanada tarkvara. 

Ettevõtte ISV osa on jagatud mitme iseseisva äri üksused, mis toodete haldamine märgatavat äri. Ettevõtte on oma arendajate, toote ja arhitektid. 

Selle ettevõtte tehnoloogia Services (ETS) business üksus pakub tsentraliseeritud see võimalus ja haldab mitme andmekeskuste, kus business ühikud majutada oma rakendusi. Koos hallata selle andmekeskuste, ETS ettevõte pakub ja haldab tsentraliseeritud koostöö (nt e-posti ja veebisaidid) ja telefonivõrgu/teenused. Ka hallata klientide suunatud töökoormus väiksem business üksused, kellel ei ole töötajate jaoks. 

Selles teemas kasutatakse järgmist personas:

- Dave on ETS Azure'i administraator.
- Alice on Contoso's Director arengu pakkumise ahelas äri üksus. 

Contoso tuleb koostada line business rakendus ja klientide suunatud rakendus. See on otsustanud Azure rakendusi käivitada. Dave loeb [asemel tellimuse juhtimise](resource-manager-subscription-governance.md) teema ja on nüüd valmis soovituste rakendamiseks. 

## <a name="scenario-1-line-of-business-application"></a>Stsenaarium 1: ärivaldkonna rakendus

Contoso on hoone source code süsteem (BitBucket) kasutavad arendajad kogu maailmas.  Rakendus kasutab taristu teenus (IaaS) majutusteenuse ja koosneb web serverid ja andmebaasiserveri. Arendajate juurdepääsu serverid nende arengu keskkonnas, kuid need ei pea juurdepääsu Azure serverid. Contoso ETS soovib luba rakenduse omanik ja meeskonna rakenduse haldamine. Contoso on ettevõtte võrgus ajal saadaval on ainult rakenduse. Dave peab tellimuse selle rakenduse häälestamine. Tellimuse ka majutada muu arendaja seotud tarkvara tulevikus.  

### <a name="naming-standards--resource-groups"></a>Nime andmise standardid ja ressursside rühmad

Dave loob tellimuse toetamiseks arendaja tööriistu, mis on levinud üle kõik üksused. Ta peab looma tähendusega nimed tellimust ja ressursside rühmade (rakendus ja võrkude) jaoks. Ta loob järgmised tellimust ja ressursside rühmad:

| Üksuse | Nimi | Kirjeldus |
| ---- | ---- | ----------- |
| Tellimuse | Contoso ETS DeveloperTools tootmise | Toetab levinud Arendaja tööriistad |
| Ressursirühm | rgBitBucket | Sisaldab rakenduse veebiserverisse ja andmebaasiserveriga |
| Ressursirühm | rgCoreNetworks | Sisaldab virtuaalne võrkude ja ühenduse lüüs saidilt |


### <a name="role-based-access-control"></a>Rollipõhine juurdepääsu reguleerimine

Pärast oma tellimuse loomist Dave soovib tagada juurdepääs vastav meeskondadel ja rakenduse omanikud oma ressursse. Dave tuvastab, et iga meeskond on erinevad nõuded. Ta kasutab rühmad, mida Azure Active Directory Contoso's kohapealse Active Directory (AD) kaudu sünkroonitud ja pakub õige pääsutaseme meeskonnad. 

Dave määrab tellimuse järgmisi rolle: 

| Roll | Määratud | Kirjeldus |
| ---- | ----------- | ----------- |
| [Omanik](./active-directory/role-based-access-built-in-roles.md#owner) | Contoso kasutaja ID hallatavate AD | See ID juhitakse lihtsalt aega (JIT) Accessi kaudu Contoso tema identiteedi haldustööriista ja tagab, et tellimuse omanik juurdepääs on täielik auditeerida. |
| [Turvalisus haldur](./active-directory/role-based-access-built-in-roles.md#security-manager) | Turva- ja juhtimise osakonna | Selle rolli võimaldab kasutajatel vaadata Azure'i turbekeskus ja ressursside olekut. |
| [Võrgu kaasautor](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Võrgumeeskonna | Selle rolli võimaldab Contoso's võrgu meeskonnatöö saidilt VPN- ja virtuaalne võrkude haldamiseks. |
| *Kohandatud roll* | Rakenduse omanik | Dave loob rolli, mis määrab ressursirühma ressursside muuta. Lisateabe saamiseks lugege teemat [Azure RBAC kohandatud rollid](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Poliitika

Dave on haldamise ressursid tellimuse järgmistele nõuetele.

- Kuna arengu vahendite toetab arendajad kogu maailmas, ta ei taha kasutajate loomine mis tahes piirkonna ressursid blokeerida. Siiski peab ta teadma, kus luuakse ressursse. 
- Ta on seotud kulud. Seetõttu soovib registreerumine virtuaalmasinates loomise rakendus omanikud takistada.  
- Kuna see rakendus arendajad palju üksustes, ta soovib sildistamine iga ressursi business üksus ja rakenduse omanik. Nende siltide abil saate ETS arve vastav meeskondadel.

Ta loob [ressursihaldur poliitikate](resource-manager-policy.md)järgmist: 

| Väli | Efekti | Kirjeldus |
| ----- | ------ | ----------- |
| asukoht | Auditilogi | Auditilogi ressursse, mis tahes ala loomine |
| tüüp | keelamiseks | Keelamiseks G-sarja virtuaalmasinates loomine |
| Sildid | keelamiseks | Nõua rakenduse omanik silt |
| Sildid | keelamiseks | Nõua maksumus keskele silt |
| Sildid | lisamine | Sildi nime **äriüksusega** ja sildi väärtus **ETS** lisada kõik ressursid |


### <a name="resource-tags"></a>Ressursi Sildid

Dave mõistab, et ta peab olema arvele tuvastamiseks maksumus center BitBucket rakendamiseks kindlale teabele. Lisaks Dave soovib leida ressursse, mis ETS kuulub. 

Ta lisab järgmised [Sildid](resource-group-using-tags.md) ressursi rühmade ja ressursse. 
 
| Sildi nimi | Sildi väärtus |
| -------- | --------- |
| ApplicationOwner | Selle rakenduse haldava isiku nimi. |
| CostCenter | Rühma, mis on Azure'i maksab maksumus keskele. |
| Äriüksusega | **ETS** (selle tellimusega seostatud business üksus) |

### <a name="core-network"></a>Core võrgu

Contoso ETS teabe turvalisus ja riski haldamine meeskonnatöö vaatab läbi Dave'i pakutud leping rakenduse Azure liikumiseks. Need soovite tagada, et rakendus on avatud Interneti-ühendus.  Dave on ka edaspidi teisaldatakse Azure'i arendaja rakendused. Nende rakenduste jaoks on vaja avalikku kasutajaliidest.  Nende nõuete täitmiseks ta pakub virtuaalse sise- ja välisandmeallikatest võrgu ja juurdepääsu piiramise võrgu turberühma.

Ta loob järgmistest allikatest:

| Ressursi tüüp | Nimi | Kirjeldus |
| ------------- | ---- | ----------- |
| Virtuaalse võrgu | vnInternal | Rakendusega BitBucket kasutanud ja on ExpressRoute Contoso oma ettevõtte võrku ühendatud.  Mõne alamvõrgu (sbBitBucket) pakub taotluse teatud IP-aadress ruumi. |
| Virtuaalse võrgu | vnExternal | Tulevaste rakendusi, mis nõuavad avalikkusele suunatud lõpp-punktide jaoks saadaval. |
| Võrgu turberühm | nsgBitBucket | Tagab, et see töökoormus rünnak pind on minimeeritud, lubades ühendused ainult pordi 443 jaoks alamvõrgu kui rakendus asub (sbBitBucket). |

### <a name="resource-locks"></a>Ressursi lukud 

Dave tuvastab, et virtuaalse sise võrgu kaudu Contoso oma ettevõtte võrku ühenduvust tuleb kaitsta väänik skripti või juhuslikku kustutamist. 

Ta loob [Ressursi Lukusta](resource-group-lock-resources.md)järgmist:

| Lukusta tüüp | Ressurss | Kirjeldus |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Ei saa kasutajad kustutamine virtuaalse võrgu või alamvõrku, kuid ei takista uue alamvõrku lisamine. |

### <a name="azure-automation"></a>Azure'i automatiseerimine 

Dave on midagi selle rakenduse automatiseerimiseks. Kuigi ta loodud konto Azure automatiseerimine, ta ei esialgu kasutada seda. 

### <a name="azure-security-center"></a>Azure'i turbekeskus 

Contoso see Teenusehaldus peab kiiresti tuvastada ja ohtude käsitlemiseks. Samuti saate aru, milliseid probleeme on olemas.  

Nende nõuete täitmiseks Dave võimaldab [Azure'i turbekeskus](./security-center/security-center-intro.md)ja pääsete juurde turvalisus halduri roll. 

## <a name="scenario-2-customer-facing-app"></a>Stsenaarium 2: kliendi-ühendusega rakendus

Ettevõtte juhtimine pakkumise ahelas business üksuse tuvastas kaasamiseks Contoso's klientidega, kasutades kliendikaarti erinevaid võimalusi. Alice'i meeskond peab selle teenuserakenduse loomine ja otsustab, et Azure'i suurendab nad suudavad selle vajadust täita. Alice töötab Dave ETS konfigureerida kaks tellimuste töötades välja ja selle rakenduse kaudu.

### <a name="azure-subscriptions"></a>Azure'i tellimused 

Dave Azure'i ettevõtteportaali sisse logib ja näeb ahelas osakond on juba olemas.  Selle projekti on esimene arengu projekti pakkumise ahelas meeskond Azure, tuvastab Dave vajadust Alice'i arendusmeeskonnale uus konto.  Ta loob oma meeskonna jaoks "Teadus- ja arendustegevus" konto ja määrab juurdepääsu Alice. Alice portaali konto kaudu logib ja loob kaks tellimused: korraldada arengu serverid ja üks hoidke tootmise serverites.  Ta järgib varem loodud nime standardid loomisel järgmistele tellimustele: 

| Tellimuse kasutamine | Nimi |
| ---------------- | ---- |
| Arengu | SupplyChain ResearchDevelopment LoyaltyCard arengu |
| Tootmise | SupplyChain toimingute LoyaltyCard tootmise |

### <a name="policies"></a>Poliitika

Dave ja Alice arutada rakendus ja tuvastada, et selle rakenduse toimib ainult Põhja-Ameerika piirkonna kliendid.  Alice ja tema meeskond kavandamine Azure rakenduse teenuse keskkonna ja Azure SQL-i abil saate luua rakenduse. Need tuleb luua virtuaalmasinates arendamise käigus.  Alice soovib tagada, et tema arendajad on vajalikud ressursid uurida ja uurida probleeme ilma ETS tõmbamine. 

**Arengu tellimuse**need järgmised poliitika loomine:

| Väli | Efekti | Kirjeldus |
| ----- | ------ | ----------- |
| asukoht | Auditilogi | Auditilogi ressursse, mis tahes ala loomine. |

Piira need kasutajad saavad luua arendatakse sku tüüp ja nad ei nõua siltide ressursi rühmade või ressursid.

**Tootmise tellimus**, nad loovad järgmised poliitikad:

| Väli | Efekti | Kirjeldus |
| ----- | ------ | ----------- |
| asukoht | keelamiseks | Keelamiseks ressursse väljaspool USA andmekeskuste loomine. |
| Sildid | keelamiseks | Nõua rakenduse omanik silt |
| Sildid | keelamiseks | Nõua osakonna silt. | 
| Sildid | lisamine | Iga ressursirühma, mis näitab tootmiskeskkonda sildi lisada. |

Need piirata tüüpi kasutajad saavad luua valmistamisel SKU-ga.

### <a name="resource-tags"></a>Ressursi Sildid 

Dave mõistab, et ta peab olema kindla teabe õige business rühmad arveldus- ja omaniku tuvastamiseks. Ta määratleb ressursi sildid ressursi rühmad ja ressursse. 
 
Sildi nimi | Sildi väärtus |
| -------- | --------- |
| ApplicationOwner | Selle rakenduse haldava isiku nimi. |
| Osakonna | Rühma, mis on Azure'i maksab maksumus keskele. |
| EnvironmentType | **Tootmise** (Isegi juhul, kui tellimus sisaldab **tootmise** nimi, sh selle sildiga tuvastamist lihtne vaadates portaalis või arve ressursse.) |

### <a name="core-networks"></a>Core võrkude

Contoso ETS teabe turvalisus ja risk management meeskonna vaatab läbi Dave'i pakutud leping rakenduse Azure liikumiseks. Need soovite tagada, et kliendikaart rakendus on õigesti eraldatud ja kaitstud DMZ võrku.  Selle nõude täita Dave ja Alice luua virtuaalse välisvõrgus ja võrgu turberühma eristamiseks kliendikaart rakenduse Contoso ettevõtte võrgus.  

**Arengu tellimuse**need loomiseks tehke järgmist.

| Ressursi tüüp | Nimi | Kirjeldus |
| ------------- | ---- | ----------- |
| Virtuaalse võrgu | vnInternal | Contoso kliendikaart arenduskeskkond pakutakse ja kaudu ExpressRoute Contoso oma ettevõtte võrku ühendatud. |

**Tootmise tellimuse**need loomiseks tehke järgmist.

| Ressursi tüüp | Nimi | Kirjeldus |
| ------------- | ---- | ----------- |
| Virtuaalse võrgu | vnExternal | Kliendikaart rakendust hostib ja on ühendatud otse Contoso's ExpressRoute. Kood on lükata oma lähtekoodi süsteemi kaudu teenustele PaaS. |
| Võrgu turberühm | nsgBitBucket | Tagab, et see töökoormus rünnak pind on minimeeritud, lubades ainult rakenduses seotud side TCP 443 kohta.  Contoso uurib ka kaitseks Web rakenduse tulemüüri kaudu. |  

### <a name="resource-locks"></a>Ressursi lukud 

Dave ja Alice anna ja te otsustate lisada ressursi lukud mõned peamised ressursid keskkonnas, et vältida juhuslikku kustutamist mõne Hairahtunut kood tõuketeatised ajal. 

Need järgmised Lukusta loomiseks tehke järgmist.

| Lukusta tüüp | Ressurss | Kirjeldus |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Et takistada inimestel virtuaalse võrgu või alamvõrku kustutamist. Funktsiooni lukustamine ei takista uue alamvõrku lisamine. |

### <a name="azure-automation"></a>Azure'i automatiseerimine 

Alice ja tema arendusmeeskonnale on olulisel tegevusraamatud haldamise keskkond, mis on selle rakenduse jaoks. Funktsiooni tegevusraamatud luba sõlmed ja muud DevOps tööülesannete lisamine ja kustutamine. 

Nende tegevusraamatud kasutamiseks need võimaldavad [automatiseerimine](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure'i turbekeskus 

Contoso see Teenusehaldus peab kiiresti tuvastada ja ohtude käsitlemiseks. Samuti saate aru, milliseid probleeme on olemas.  

Nende nõuete täitmiseks Dave võimaldab Azure'i turbekeskus. Ta tagab Azure'i turbekeskus jälgib ressursse, ja saate tutvuda meeskondadel DevOps ja turve. 

## <a name="next-steps"></a>Järgmised sammud

- Ressursihaldur mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide loomise head tavad](resource-manager-template-best-practices.md).

*Selles teemas kaasa [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
