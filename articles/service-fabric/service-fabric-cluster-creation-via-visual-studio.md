<properties
   pageTitle="Teenuse struktuuri kobar, kasutades Visual Studio häälestamine | Microsoft Azure'i"
   description="Kirjeldab, kuidas häälestada teenuse struktuuri kobar Azure'i ressursirühm projektis Visual Studio abil loodud Azure'i ressursihaldur malli abil"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Visual Studio abil teenuse struktuuri kobar häälestamine
Selles artiklis kirjeldatakse, kuidas häälestada mõne Azure teenuse struktuuri kobar Visual Studio ja Azure ressursihaldur malli abil. Visual Studio Azure'i ressursi rühmaprojekti kasutame malli loomine. Kui mall on loodud, seda saab töötama otse Azure'i Visual Studio. Seda saab kasutada käsikirjaga või osana pidev integratsioon (CI) poole.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Teenuse struktuuri kobar malli abil mõnda Azure ressursi rühma projekti loomine
Alustamiseks avage Visual Studio ja Azure ressursi rühma projekti (see on saadaval **Cloud** kausta):

![Uue projekti dialoogiboks Azure ressursirühm projekti valitud][1]

Saate luua uue Visual Studio lahendus selle projekti või lisada olemasolevat lahendust.

>[AZURE.NOTE] Kui te ei näe Azure ressursi rühma projekti Cloud sõlme all, ei ole Azure'i SDK installitud. Käivitada Web platvormi Installer ([installige see nüüd](http://www.microsoft.com/web/downloads/platform.aspx) kui teil pole veel), ja seejärel otsige "Azure SDK jaoks .NET" ja installige versioon, mis ühildub rakendusega Visual Studio versiooni.

Pärast seda, kui vajutate nuppu OK, Visual Studio palub teil valige loodava ressursihaldur mall:

![Valige Azure malli dialoogiboksis valitud teenuse struktuuri kobar malli abil][2]

Valige teenuse struktuuri kobar Mall ja vajuta uuesti nuppu OK. Nüüd on loodud projekti ja ressursihaldur malli.

## <a name="prepare-the-template-for-deployment"></a>Juurutamise malli ettevalmistamine
Enne malli juurutatakse klaster loomiseks, peate sisestama nõutav malli parameetrite väärtused. Järgmiste parameetrite väärtused on lugeda selle `ServiceFabricCluster.parameters.json` fail, mis on selle `Templates` ressursi rühma projekti kausta. Avage fail ja sisestage järgmised väärtused:

|Parameetri nimi           |Kirjeldus|
|-----------------------  |--------------------------|
|adminUserName            |Administraatori konto nimi teenuse struktuuri masinad (sõlmed).|
|certificateThumbprint    |Sert, mida tagab klaster sõrmejälje.|
|sourceVaultResourceId    |*Ressursi ID* võtme Vault sert, mida tagab klaster talletuskoht.|
|certificateUrlValue      |Kobar turbeserti URL-i.|

Visual Studio teenuse struktuuri ressursihaldur mall loob turvaline klaster, mis on kaitstud sert. Selle serdi on tähistatud kolm viimast malli parameetrid (`certificateThumbprint`, `sourceVaultValue`, ja `certificateUrlValue`), ja see on **Azure klahvi Vault**olemas. Lisateavet kobar turbeserti, vt [teenuse struktuuri klaster turvalisus stsenaariumid](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikkel loomise kohta.

## <a name="optional-change-the-cluster-name"></a>Valikuline: kobar nime muutmine
Iga teenuse struktuuri kobar on nimi. Azure'i loomisel struktuuri kobar kobar nimi määratleb (koos Azure piirkond) klaster nimi süsteemi (DNS). Näiteks kui teie nime klaster `myBigCluster`, ja uue kobar majutavad ressursirühm asukoht (Azure'i piirkond) on Ida-USA, klaster DNS-i nimi on see `myBigCluster.eastus.cloudapp.azure.com`.

Vaikimisi kobar nimi luuakse automaatselt ja tehtud kordumatu manustamise juhusliku järelliide "kobar" eesliite. See on väga lihtne malli **pidev integratsioon** (CI) süsteemi osana. Kui soovite kasutada klaster teatud nimi, mis tähenduslik, määrake väärtus on `clusterName` muutuv ressursihaldur malli faili (`ServiceFabricCluster.json`) teie valitud nimi. See on määratletud seda faili esimest muutuja.

## <a name="optional-add-public-application-ports"></a>Valikuline: lisage avaliku rakenduse pordid
Võite ka muuta avaliku rakenduse Porte klaster enne juurutamist. Mall avaneb vaikimisi vaid kahe avaliku TCP-pordid (80 ja 8081). Kui teil on vaja veel rakendusi, Azure'i laadi koormusetasakaalustusteenuse määratlemise mallis muuta. Määratluse on talletatud peamised mallifail (`ServiceFabricCluster.json`). Faili avamine ja otsida `loadBalancedAppPort`. Iga pordi on seostatud kolme esemeid:

1. Malli muutuja, mis määratleb pordi TCP pordi väärtus:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. *Juures* määratleva tihti ja kaua Azure koormuse koormusetasakaalustusteenuse proovib kasutada kindla teenuse struktuuri sõlme enne probleemse teise. Laadi koormusetasakaalustusteenuse ressursi sondid kuuluvad. Siin on esimene vaikeport rakenduse juures definitsiooni:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. *Koormust tasakaalustavad reegel* , mis ühendab pordi juures, mis võimaldab laadi tasakaalustamiseks kogu kogumi teenuse struktuuri kobar sõlmed:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Kui rakendusi, mida soovite juurutada klaster on vaja rohkem pordid, saate need lisada loomise täiendavad juures ja koormust tasakaalustavad seisundireeglite määratluste. Ressursihaldur Mallid Azure'i laadimine koormusetasakaalustusteenuse töötamise kohta leiate lisateavet teemast [on sisemine laadi koormusetasakaalustusteenuse malli abil loomise alustamiseks](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Malli Visual Studio abil
Kui olete kõik nõutavad parameetrite väärtused on`ServiceFabricCluster.param.dev.json` faili, olete valmis malli ja luua klaster teenuse struktuuri. Paremklõpsake rühma ressursi projekti Visual Studio Solution Exploreris ja valige **Deploy | Uue juurutamise...**. Vajaduse korral kuvatakse Visual Studio dialoogiboksis **Deploy ressursirühma** palub teil autentida Azure:

![Ressursirühm dialoogiboks juurutamine][3]

Dialoogiboksis saate valida klaster olemasoleva ressursihaldur ressursirühm ja pakub teile võimalust luua uue. Tavaliselt mõttekas kasutada eraldi ressursirühm teenuse struktuuri kobar.

Pärast klõpsamist nuppu Deploy, Visual Studio palub teil kinnitada, et malli parameetrite väärtused. Klõpsake nuppu **Salvesta** . Üks parameeter ei ole nõutud väärtuse: klaster haldus konto parooli. Peate sisestama parooli väärtuse, kui Visual Studio küsib teilt ühte.

>[AZURE.NOTE] Alustades Azure'i SDK 2.9, Visual Studio toetab lugemise paroolide **Azure'i klahvi** hoidlast käigus juurutamist. Dialoogiboksis malli parameetrid teate, et selle `adminPassword` parameetri tekstiväljal on vähe "võti" ikoon paremal. See ikoon võimaldab teil valida mõne olemasoleva võtme vault salajane klaster haldus parooli. Lihtsalt veenduge, et esimene luba Azure'i ressursihaldur Accessi malli juurutamiseks oma võtme vault Täpsemad juurdepääsu poliitika. 

Saate juurutamise käigus Visual Studio väljundi aknas edenemist jälgida. Kui malli juurutamise on lõpule jõudnud, uue klaster on kasutamiseks valmis!

>[AZURE.NOTE] Kui kunagi kasutati PowerShelli haldamine Azure'i nüüd kasutatavast seadmest, peate teha veidi parimate.
>1. Luba PowerShelli skripti käivitades funktsiooni [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) käsk. Arengu seadmed "piiramatu" poliitika on tavaliselt aktsepteeritav.
>2. Otsustage, kas lubada Azure PowerShelli käskude diagnostika andmete kogumine ja käivitada [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) või [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) vastavalt vajadusele. See aitab vältida mittevajalike viipasid malli juurutamisel.

Kui seal on vigu, [Azure portaali](https://portal.azure.com/) ja avage i juurutatud ressursirühma. **Kõik sätted**, klõpsake käsku **juurutuste** enne sätted. Nurjunud-ressursirühm juurutamise jätab üksikasjalik diagnostikateave seal.

>[AZURE.NOTE] Teenuse struktuuri kogumite nõua teatud sõlmed olevat säilitada kättesaadavus ja säilitamine riigi - edaspidi "säilitades kvoorumi." Seega pole turvaliste sulgeda kõik masinad klaster, kui teie tehtud esmalt [oleku täielik varukoopia](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Järgmised sammud
- [Lisateavet teenuse struktuuri kobar Azure'i portaalis häälestamise kohta](service-fabric-cluster-creation-via-portal.md)
- [Saate teada, kuidas hallata ja juurutada teenuse struktuuri rakendused Visual Studio abil](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
