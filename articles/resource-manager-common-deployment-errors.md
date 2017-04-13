<properties
   pageTitle="Levinud Azure juurutamise tõrkeotsing | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse levinud vigade lahendamine ressursid juurutamisel Azure'i Azure ressursihaldur abil."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="juurutamise viga, azure juurutuse juurutada Azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Levinud Azure juurutamise tõrkeotsing Azure'i ressursihaldur

Selles teemas kirjeldatakse, kuidas saate lahendada mõned levinud Azure juurutamise tõrked võivad ilmneda. Kui vajate rohkem teavet, mis läks valesti, juurutamise, esmalt teemast [vaate juurutamise toimingud](resource-manager-troubleshoot-deployments-portal.md) ja seejärel naasta sellest artiklist leiate teavet tõrke. Selles teemas jaotiste loendis kuvatakse tõrkekood.

## <a name="invalid-template"></a>Vigaste Mall

See tõrge võib olla mitu erinevat tüüpi vigade tulemus. 

### <a name="syntax-error"></a>Süntaks

Kui kuvatakse tõrketeade, mis näitab valideerimine nurjus mall, peate süntaks probleem malli. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

See tõrge on lihtne teha, kuna malli avaldiste võib olla keerukas. Näiteks järgmine nimi ülesande salvestusruumi konto sisaldab sulgudes kogumit, kolm funktsiooni, kolm komplekti sulgude, üks kogum ülakomadega ja ühe atribuudi:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Kui esitate kattuvad süntaks malli toodab väärtus, mis on erinev, kui plaanite.

Kui saate seda tüüpi tõrkeid, hoolikalt üle avaldise süntaksisse. Kaaluge JSON-redaktor, nt [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) või [Visual Studio kood](resource-manager-vs-code.md), mis teid hoiatada süntaksivigu. 

### <a name="incorrect-segment-lengths"></a>Vale lõigu pikkusega

Mõne muu sobimatu malli tõrge ilmneb juhul, kui ressursi nimi pole õiges vormingus.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Juurkausta taseme ressursi peab olema väiksem ühe lõigu nimi kui ressursside kirjas. Iga segmendi sõltub on kaldkriips. Järgmises näites tüüp on kahe ja nimi on ühe lõigu, nii et see on **sobiv nimi**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Kuid järgmises näites on **sobiv nimi** , sest see on sama palju segmente tüübiks.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Lapse ressursid, tüüp ja nimi on sama palju segmente. See arv segmente mõttekas, kuna täielik nimi ja tüüp lapsele sisaldab ema nimi ja tüüp. Seetõttu täielik nimi on veel üks vähem kui täielik lõigu. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Kuidas lõigud õige võib olla keeruline ressursihaldur tüüpi, mis on rakendatud üle ressursi pakkujad. Näiteks veebisaidi ressursi Lukusta rakendamine nõuab koos nelja segmente tüüp. Seetõttu nimi on kolm:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopeeri index ei tohiks

**InvalidTemplate** tõrge ilmneb siis, kui olete rakendanud **Kopeeri** elemendi Mall, mis ei toeta – see element osa. Kopeeri elemendi saab rakendada ainult ressursi tüüp. Kopeeri ei saa rakendada atribuudi sees ressursi tüüp. Näiteks Kopeeri rakendamine virtuaalse masina, kuid ei saa te seda rakendada OS ketast virtuaalse masina jaoks. Mõnel juhul saate teisendada lapse ressursi ema ressursile Kopeeri tsükkel loomiseks. Kopeeri kasutamise kohta leiate lisateavet teemast [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Parameeter ei sobi

Malli saate määrata lubatud väärtused parameeter, kui annate väärtus, mis ei ole need väärtused, kuvatakse teade, mis on sarnane järgmine tõrketeade:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Kontrollige malli lubatud väärtusi, ja ühe käigus juurutamist.

## <a name="not-found-or-resource-not-found"></a>Ei leitud (või ressursside ei leitud)

Kui malli on ressurss, mida ei saa lahendada nime, saate tõrketeate, mis on sarnane:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Kui proovite juurutamine puuduva ressursi malli, kontrollige, kas soovite lisada sõltuvus. Ressursihaldur optimeerib juurutus, luues ressursid samal ajal, kui võimalik. Kui üks ressurss tuleb kasutada mõne muu ressursi pärast, peate kasutama **ei leia dependsOn** elemendi malli loomiseks sõltuvus muu ressursiga. Näiteks web appi juurutamisel kuuluma rakenduse teenusleping. Kui te pole määranud, et veebirakenduse sõltub rakenduse teenusleping, loob ressursihaldur korraga nii ressursid. Saate tõrketeate selle kohta, rakenduse teenuse leping ressursi ei leita, kuna seda pole veel web Appile atribuudi katsel. Selle vea, seades sõltuvus web Appis vältimine

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Näete ka selle vea, kui ressurss on olemas eri ressursirühm, kui see on juurutatud. Sel juhul saada täielik nimi ressursi [ResourceIdkasutamisel funktsiooni](resource-group-template-functions.md#resourceid) abil.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Kui proovite kasutada funktsiooni [viide](resource-group-template-functions.md#referenc) või [listKeys](resource-group-template-functions.md#listkeys) ressursiga, mida ei saa lahendada, kuvatakse järgmine tõrketeade:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Otsige avaldis, mis sisaldab funktsiooni **viide** . Kontrollige, kas parameetrite väärtused on õiged.

## <a name="storage-account-already-exists-or-already-taken"></a>Salvestusruumi konto on juba olemas (või juba tehtud)

Salvestusruumi konto korral peab võimaldama ressurss, mis on Azure üle kordumatu nimi. Kui annate kordumatu nimi, kuvatakse tõrge, näiteks:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Saate luua, ühendades oma nimetamistava [uniqueString](resource-group-template-functions.md#uniquestring) funktsiooni tulemiga kordumatu nimi.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Kui juurutada salvestusruumi konto sama nimega salvestusruumi konto teie tellimus, kuid pakuvad teise asukohta, saate tõrketeate, mis näitab salvestusruumi konto on juba olemas mõnes muus asukohas. Kustutage olemasolev salvestusruumi konto või sisestage olemasoleva salvestusruumi konto samasse asukohta.

## <a name="account-name-invalid"></a>Kas konto nime sobimatu

Näete **AccountNameInvalid** tõrge, kui proovite anda salvestusruumi konto nimi, mis sisaldab keelatud märke. Salvestusruumi kontonimed peab olema vahel 3 ja 24 märki ja arvude ja väiketähed tähed ainult.

## <a name="no-registered-provider-found"></a>Ei leitud registreeritud pakkuja

Ressursi juurutamisel võidakse kuvada tõrkekood ja sõnumi:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Kolm põhjustest, kuvatakse järgmine tõrketeade:

1. Pole toetatud ressursi asukoht
2. API versioon ei toeta ressursi
3. Ressursi pakkuja pole registreeritud tellimuse

Kuvatakse tõrketeade peaks teile soovitatud toetatud asukohad ja API versioonid. Saate muuta oma mall ühte soovitatud väärtused. Enamik teenusepakkujaid on registreeritud Azure portaali või kasutate käsurea liides järgi automaatselt, kuid mitte kõigis. Kui te pole ressursile pakkuja enne kasutanud, peate selle pakkuja registreerimiseks. Võite avastada rohkem teada PowerShelli või Azure CLI ressursi pakkujad.

### <a name="powershell"></a>PowerShelli

Teie olek kuvamiseks kasutage **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Pakkuja registreerimiseks kasutage **Register-AzureRmResourceProvider** ja soovite registreerida ressursi pakkuja nimi.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Kindlat tüüpi ressursi toetatud asukohad saamiseks kasutage:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Kindlat tüüpi ressursi toetatud API versioonide saamiseks kasutage:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure'i CLI

Kui soovite näha, kas pakkuja on registreeritud, kasutada selle `azure provider list` käsk.

    azure provider list

Registreerida ressursi pakkuja, kasutage funktsiooni `azure provider register` käsk ja määrake *nimeruum* registreerida.

    azure provider register Microsoft.Cdn

Ressursi pakkuja toetatud asukohad ja API versioonide vaatamiseks kasutada:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Toiming pole lubatud

Teil võib olla probleeme, kui juurutamise ületab piirmäära, mis võiks olla ressursirühm, tellimused, kontod ja muud otsinguulatuste. Näiteks teie tellimus võib olla konfigureeritud valdkond piirkonna arv piirata. Kui proovite juurutamine virtuaalse masina koos rohkem tuuma kui lubatud summa, kuvatakse tõrge, kvoodi ületamise.
Täielikult kvoodi leiate teemast [Azure tellimuse ja teenuste piirangud, kvootide ja piiranguid](azure-subscription-service-limits.md).

Oma tellimuse kvootide poolide uurida, võite kasutada funktsiooni `azure vm list-usage` käsk CLI Azure. Järgmine näide illustreerib, et core kvoodi tasuta prooviversiooni konto on 4:

    azure vm list-usage

Mis annab:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Malli, mis loob neli protsessorituuma Lääne USA piirkonna juurutamisel kuvatakse juurutamise tõrketeade, mis näeb välja umbes:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Või PowerShelli, saate kasutada cmdlet-käsu **Get-AzureRmVMUsage** .

    Get-AzureRmVMUsage

Mis annab:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

Sellisel juhul peaksite portaalis ja faili tugi probleemi oma piirkond, kuhu soovite juurutada piirmäära tõsta.

> [AZURE.NOTE] Pidage meeles, et ressursi rühmade, kvoodi on iga üksiku piirkonna, mitte kogu tellimuse jaoks. Kui teil on vaja juurutada 30 poolide Lääne USA, peate küsima 30 ressursihaldur poolide Lääne US. Kui teil on vaja juurutada 30 südamikud mis tahes regioonid, millele teil on juurdepääs, küsige 30 ressursihaldur tuuma kõikides piirkondades.

## <a name="invalid-content-link"></a>Sobimatu sisu link

Kui kuvatakse tõrketeade:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Tõenäoliselt proovisite linkida pesastatud Mall, mis pole saadaval. Kontrollige selle URL-i pesastatud malli. Kui mall on olemas salvestusruumi konto, veenduge, et URI on juurdepääsetav. Võimalik, et peate läbima SAS luba. Lisateabe saamiseks lugege teemat [lingitud mallide Azure'i ressursihaldur kasutamine](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Autoriseerimine nurjus.

Te saate juurutamisel tõrketeate, kuna konto või teenuse põhisumma juurutamine ressursside katsel ei pääse nende toimingute teostamiseks. Azure Active Directory võimaldab või mis ressurssidele arvutustäpsuse hea määral juurdepääsu oma administraatoril määrata, millised identiteedid. Näiteks kui teie konto on määratud roll lugeja, te ei saa luua ressursid. Sel juhul näete teadet autoriseerimine nurjus.

Rollipõhine juurdepääsu reguleerimine kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).

Lisaks Rollipõhine juurdepääsu reguleerimine teie juurutamise toimingud võib olla piiratud poliitikate tellimust. Poliitika kaudu administraator saate jõustada põhimõtted tellimuse juurutatud kõik ressursid. Näiteks administraator saab nõuda, et teatud sildi väärtus ressursi tüüp. Kui teil pole täita nõuete poliitika, saate tõrketeate ajal. Poliitika kohta leiate lisateavet teemast [Kasutamine poliitika haldamine ressursid ja reguleerida juurdepääsu](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Tõrkeotsingu virtuaalmasinates

| Tõrge | Artiklid |
| -------- | ----------- |
| Kohandatud skriptitõrkeid laiend | [Windows VM laiend tõrked](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />või<br />[Linux VM laiend tõrked](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Ettevalmistamise tõrgete OS pilt | [Uue Windows VM tõrked](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />või<br />[Uue Linux VM tõrked](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Eraldatud tõrked | [Windows VM eraldatud tõrked](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />või<br />[Linux VM eraldatud tõrked](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Kui proovite ühendust Secure Shell (SSH) tõrked | [Secure Linux VM Shell ühendused](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Ühenduse VM töötava rakenduse tõrked | [Rakendus töötab Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />või<br />[Rakendus töötab Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Kaugtöölaua ühendus tõrked | [Kaugtöölaua Windows VM ühendused](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Ühenduse tõrkeid lahendatud ümbersuunamine | [Uue Azure sõlme virtuaalse masina ümberkorraldamine](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Pilveteenuse teenuse tõrked | [Pilveteenuse juurutamise probleemide](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Muude teenuste tõrkeotsing

Järgmises tabelis pole tõrkeotsingu teemades Azure täieliku loendi. Selle asemel kujundab juurutamine või konfigureerida ressursid seotud probleemid. Kui vajate abi tõrkeotsingu käitusaja ressursi, leiate Azure'i teenuse dokumentatsioonist.

| Teenus | Artikkel |
| -------- | -------- |
| Automatiseerimine | [Tõrkeotsingu näpunäited levinud vigade Azure automatiseerimine](./automation/automation-troubleshooting-automation-errors.md) |
| Azure'i virnas | [Microsoft Azure'i Virnlintdiagrammil tõrkeotsing](./azure-stack/azure-stack-troubleshooting.md) |
| Azure'i virnas | [Veebirakenduste ja Azure virnas](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Andmete Factory | [Andmete Factory probleemide tõrkeotsing](./data-factory/data-factory-troubleshoot.md) |
| Teenuse struktuuri | [Levinud probleemide teenust Azure teenuse struktuuri juurutamisel](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Saidi taastamine | [Jälgimine ja kaitse virtuaalmasinates ja füüsilise servereid tõrkeotsing](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Salvestusruumi | [Jälgimine, diagnoosimine ja tõrkeotsing Microsoft Azure'i Tabelimäluga](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [StorSimple seadme juurutamise probleemide tõrkeotsing](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-andmebaas | [Azure'i SQL-andmebaasiga ühenduse tõrkeotsing](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL-i andmebaas | [Tõrkeotsingu Azure SQL-andmebaas](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Mõista, kui on valmis juurutamine

Kui kõigi teenusepakkujate tulu juurutamise edukalt aruanded Azure ressursihaldur edu juurutamine. Siiski see sõnum ei tähenda, et teie ressursirühm on "aktiivne ja kasutajate jaoks valmis." Näiteks juurutamine võib tekkida vajadus allalaadimine täienduste, oodake-malli ressursid või keerukate skriptide installida. Ressursihaldur ei tea, kui need toimingud lõpule viia, kuna need ei ole tegevused, mida pakkuja jälitab. Sellisel juhul võib olla veidi aega enne oma vahendite tegelike kasutamiseks valmis. Selle tulemusena peaks eeldavad, et juurutamise oleku õnnestub juurutamise saab kasutada pisut aega.

Saate takistada Azure juurutamise edu, aga aruandlus, luues kohandatud skript oma kohandatud mall. Skripti oskab jälgimine süsteemi hõlmava valmisoleku tagamiseks kogu juurutamise ja tagastab edukalt ainult siis, kui kasutaja saab interaktiivselt kasutada kogu juurutamise. Kui soovite tagada, et teie laiend on viimase käivitada, **ei leia dependsOn** atribuuti kasutada malli.

## <a name="next-steps"></a>Järgmised sammud

- Auditi toimingute kohta leiate teemast [auditilogi toimingute abil ressursihaldur](resource-group-audit.md).
- Toimingute määratlemiseks juurutamisel tõrgete kohta leiate teemast [vaate juurutamise toimingud](resource-manager-troubleshoot-deployments-portal.md).
