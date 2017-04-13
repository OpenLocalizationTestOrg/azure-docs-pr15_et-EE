
<properties
    pageTitle="Azure'i CLI ressursside haldamine | Microsoft Azure'i"
    description="Azure'i käsurea liides (CLI) abil Azure ressursid ja rühmade haldamine"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Azure'i CLI abil Azure ressursid ja ressursside rühmade haldamine


> [AZURE.SELECTOR]
- [Portaal](azure-portal/resource-group-portal.md) 
- [Azure'i CLI](xplat-cli-azure-resource-manager.md)
- [Azure'i PowerShelli](powershell-azure-resource-manager.md)
- [REST API-GA](resource-manager-rest-api.md)


Azure'i käsurea liides (Azure'i CLI) on üks mitmeid tööriistu, mille abil saate juurutada ja hallata ressursid koos ressursihaldur. Selles artiklis tutvustatakse levinud viisi Azure materjale ja ressursside rühmade haldamiseks ressursihaldur režiimis Azure'i CLI abil. Juurutamiseks ressursse CLI kasutamise kohta leiate artiklist [ressursihaldur mallide ja Azure CLI Deploy ressursid](resource-group-template-deploy-cli.md). Tausta Azure ressursid ja ressursihaldur kohta, leiate [Azure'i ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Hallata Azure CLI Azure ressursse, peate [installige Azure'i CLI](xplat-cli-install.md)ja [Azure sisse logida](xplat-cli-connect.md) , kasutades funktsiooni `azure login` käsk. Veenduge, et CLI on ressursihaldur režiimis (Käivita `azure config mode arm`). Kui olete valmis neid asju, olete valmis.



## <a name="get-resource-groups-and-resources"></a>Ressursi rühmad ja ressursid

### <a name="resource-groups"></a>Ressursi rühmad

Kõigi ressursside rühmad tellimuse ja nende asukohtade loendi saamiseks selle käsu käivitamine.

    azure group list
    

### <a name="resources"></a>Ressursid
 Loetle kõik ressursid rühma, üks nime *testRG*, nt käsu.

    azure resource list testRG

On üksikute ressurss rühmast, näiteks VM nimega *MyUbuntuVM*kuvamiseks kasutage käsk, näiteks järgmist.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Pange tähele **Microsoft.Compute/virtualMachines** parameeter. See parameeter näitab ressursi taotlemise kohta teabe tüüp.
    
>[AZURE.NOTE]**Azure'i ressursi** käskude peale **loend** käsu kasutamisel Määrake ressursi API versiooni koos parameetriga **– o** . Kui te pole kindel, kas API-versiooni, küsige mallifail ja otsige ressursi apiVersion väli. API versioonide sisse ressursihaldur kohta leiate lisateavet teemast [ressursihaldur pakkujate ning API versioonide ja skeemid](resource-manager-supported-services.md).

Ressursi üksikasjade kuvamisel sageli on kasulik kasutada funktsiooni `--json` parameeter. Selle parameetri muudab väljund lihtsam lugeda, kuna mõned väärtused on pesastatud struktuurid või saidikogumid. Järgmises näites näitab tulemite **kuvamine** käsu JSON dokumendina.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Soovi korral võite salvestada JSON andmete faili, kasutades funktsiooni &gt; märgi väljundi faili. Näiteks:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Sildid

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Ressursside haldamine


Käivitage ressursi nt salvestusruumi konto lisamiseks ressursirühma sarnane käsk:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Lisaks määramisele koos parameetriga **– o** ressursi API versiooni kasutada parameetrit **– p** edasi JSON-vormingus stringi mõni nõutav või täiendava atribuudid.
    
    
Mõne olemasoleva ressursi, nt virtuaalse masina ressursi kustutamiseks kasutage käsk, näiteks järgmist.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Olemasolevate ressursside teisaldamiseks teise ressursirühm või tellimuse käsu **azure ressursi teisaldada** . Järgmises näites on kujutatud teisaldamise Redis vahemälu uue ressursirühma. Sisestage parameetri **-i** ressursi ID-d komaga eraldatud loend on liikuda.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Ressursside juurdepääsu reguleerimine

Saate luua ja hallata poliitikate Azure ressursse juurdepääsu piiramiseks Azure'i CLI. Poliitika määratlused ja poliitika määramine ressursside kohta leiate [poliitika haldamine ressursid ja reguleerida juurdepääsu kasutada](resource-manager-policy.md).

Näiteks järgmised poliitika keelamiseks kõik kutsed kui asukoht ei ole Lääne USA või Põhja keskse meile määratlemine ja salvestage see poliitika määratluse faili policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Käivitage **rühmapoliitika määratluse loomine** käsk:

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
See käsk näitab väljund sarnaneb järgmisega.

    + Loomise poliitika määratluse MyPolicy andmed: PolicyName: MyPolicy andmed: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    andmed: PolicyType: kohandatud andmed: DisplayName: määratlemata andmed: kirjeldus: määratlemata andmed: PolicyRule: välja = asukohta, klõpsake = [westus northcentralus] efekti = keelamiseks

 Korraga soovite ulatuse määramiseks kasutage **PolicyDefinitionId** tagastatud eelmise käsu. Järgmises näites selle ulatus on tellimus, kuid saab ulatus ressursi rühmadele või üksikute ressursid.

    Azure'i poliitika ülesande loomine MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Saate hankida, muutmine või eemaldamine poliitika määratlused **poliitika määratluse kuvamine**, **poliitika määratlus seadmine**ja **poliitika määratluse kustutamine** käskude abil.

Samuti saate hankida, muutmine või eemaldamine poliitika ülesanded **poliitika ülesande kuvada**, **poliitika ülesande määramine**ja **poliitika ülesande kustutamine** käskude abil.


## <a name="export-a-resource-group-as-a-template"></a>Ekspordi ressursirühma mallina

Olemasoleva ressursirühma, saate vaadata ressursirühma ressursihaldur mall. Malli eksportimise pakub kahte eelised:

1. Kuna kõik taristu on määratletud malli, saate tulevaste juurutuste lahenduse hõlpsalt automatiseerida.

2. Te saate tutvuda malli süntaks vaadates JSON, mis tähistab teie lahendus.

Azure'i CLI kasutamisel saate eksportida Mall, mis tähistab ressursirühma hetkeseisu, või alla laadima malli, mida kasutati kindla juurutamiseks.

* **Malli ressursirühma eksportimine** – see on kasulik, kui teie tehtud muudatused ressursirühma ja tuua selle praeguses olekus JSON kujutis on vaja. Siiski loodud Mall sisaldab ainult minimaalne arv parameetrite ja ühtegi muutujat. Malli väärtused on raske koodiga. Enne juurutamist loodud malli, soovite teisendada mitu väärtust parameetrid nii, et saate kohandada juurutamise viibite.

    Eksportida ressursirühma malli kohalikku kausta, käivitage selle `azure group export` käsk, nagu on näidatud järgmises näites. (Asendada operatsioonisüsteemi keskkonna jaoks sobiv kohalikku kausta).

        azure group export testRG ~/azure/templates/

* **Kindla juurutamiseks malli allalaadimiseks** – see on kasulik, kui teil on vaja vaadata tegelik Mall, mida kasutati juurutamiseks ressursse. Mall sisaldab kõigi parameetrite ja muutujate algse juurutamise jaoks määratletud. Kui mõni teie ettevõtte muutnud ressursirühm väljaspool määratluse malli, ei selle malli tähistavad praeguses olekus ressursirühma.

    Kindla juurutamiseks kohalikku kausta kasutatava malli allalaadimiseks käivitada soovitud `azure group deployment template download` käsk. Näiteks:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Malli ekspordi on eelvaade ja kõik ressursi tüüpi toeta eksportimise malli. Kui proovite eksportida malli, võidakse kuvada tõrketeade, mis kinnitab, et ei eksporditud järgmised materjalid. Vajadusel käsitsi määratleda nende ressursside malli pärast allalaadimise kaudu.



## <a name="next-steps"></a>Järgmised sammud

* Üksikasjade juurutamise toimingute ja Azure CLI juurutamise tõrkeotsing, leiate teemast [vaate juurutamise toimingute Azure'i CLI](resource-manager-troubleshoot-deployments-cli.md).
* Kui soovite rakenduse või skripti ressursid juurdepääsu häälestamine CLI abil, vt [Kasutada Azure CLI põhisumma ressursid juurdepääsu teenuse](resource-group-authenticate-service-principal-cli.md).


