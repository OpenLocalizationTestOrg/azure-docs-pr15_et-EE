<properties
    pageTitle="Pidev integratsioon VS meeskonnatöö teenuste Azure'i ressursirühm projektide abil | Microsoft Azure'i"
    description="Kirjeldab, kuidas häälestada pidev Visual Studio Team Services integreerimise Azure'i ressursirühm juurutamise projektide Visual Studio abil."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Pidev integratsioon Visual Studio meeskonnatöö teenuste kasutamine Azure ressursirühm juurutamise projektid

Juurutada mõni Azure Mall, peate läbima eri tööülesannete täitmiseks: Azure'i Koosta Test, kopeerimine (nimetatakse ka "Lavastus"), ja juurutada malli.  On kaks võimalust selleks Visual Studio meeskonnatöö teenused (VS meeskonnatöö teenused). Allpool on mõlema meetodi pakuvad sama tulemuse, et valida üks, mis sobib kõige paremini teie töövoog.

-   Lisada ühe etapi definitsiooni Koosta, mis töötab PowerShelli skripti, mis sisaldub Azure'i ressursirühm juurutamise projekt (Deploy AzureResourceGroup.ps1). Skript kopeerib esemeid ja seejärel kasutab malli.
-   Saate lisada mitu VS meeskonnatöö teenuste koostada juhised leiate iga etapi ülesannet, üks.

Selles artiklis näitab, kuidas kasutada esimene variant (kasutage Koosta määratlus PowerShelli skripti käivitamiseks). Üks eeliseid see suvand on skript arendajate Visual Studio kasutab sama skript, mis kasutab VS meeskonnatöö teenused. See toiming eeldab, et teil on juba märgitud VS meeskonnatöö teenustesse Visual Studio juurutamise projekti.

## <a name="copy-artifacts-to-azure"></a>Azure'i esemeid kopeerimine 

Stsenaarium, olenemata sellest, kui teil on mis tahes esemeid, mida on vaja malli juurutuse pead Azure'i ressursihaldur juurdepääsu andmine. Neid esemeid võib sisaldada näiteks failid.

-   Pesastatud Mallid
-   Konfiguratsiooni skripte ja DSC skriptide
-   Binaarfailid

### <a name="nested-templates-and-configuration-scripts"></a>Pesastatud Mallid ja skriptide konfigureerimine
Kui kasutate Visual Studio mallid (või Visual Studio Koodilõigud ehitatud) PowerShelli skripti, mitte ainult etapid esemeid, seda ka parameterizes URI erinevate juurutuste ressursid. Skripti siis kopeerib esemeid turvaline container Azure, loob SaS luba, et container jaoks, ja seejärel vastav teave malli kasutamise kohta. Vaadake [loomine malli juurutamise](https://msdn.microsoft.com/library/azure/dn790564.aspx) pesastatud mallide kohta.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Pidev juurutamise VS meeskonnatöö teenuste häälestamine

Helistamiseks VS meeskonnatöö teenuste PowerShelli skripti, peate värskendama definitsiooni koostamine. Lühidalt, on kirjeldatud juhiseid. 

1.  Koosta definitsiooni redigeerimine
1.  Saate häälestada Azure autoriseerimine VS meeskonnatöö teenused.
1.  Lisage Azure PowerShelli koostamine samm PowerShelli skripti Azure ressursirühm juurutamise projekti viitav.
1.  Määrake töötamiseks sisseehitatud VS meeskonnatöö teenuste projekti *- ArtifactsStagingDirectory* parameeter.

### <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse

Järgmised toimingud juhendab teid juhiseid, mis on vajalikud pideva VS meeskonnatöö teenuste konfigureerimine 

1.  VS meeskonnatöö teenuste Koosta definitsiooni redigeerimine ja lisage Azure PowerShelli koostamine samm. Valige Koosta määratluse kategoorias **koostada määratlused** ja seejärel valige **Redigeeri** linki.

    ![][0]

1.  Lisada uue **Azure PowerShelli** Koosta sammu Koosta määratluse ja seejärel valige **lisa koostada samm...** nupp.

    ![][1]

1.  Valige **Deploy tööülesande** kategooria, valige **Azure PowerShelli** toiming ja valige oma nuppu **Lisa** .

    ![][2]

1.  Valige **Azure PowerShelli** Koosta etappi ja seejärel täitke selle väärtused.

    1.  Kui teil on juba Azure teenuse lõpp-punkti VS meeskonnatöö teenuste loendisse, valige rippmenüüst loendi **Azure tellimuse** tellimuse ja siis järgmise jaotise juurde. 

        Kui teil pole Azure teenuse lõpp-punkti VS meeskonnatöö teenused, peate seda lisada. Käesoleva suunab teid läbi vastava protsessi. Kui teie Azure'i konto kasutab Microsofti kontoga (nt Hotmail), peate saamiseks teenuse põhilise autentimise toimige järgmiselt.

    1.  Valige **Halda** lingi kõrval **Azure tellimuse** rippmenüüst loendiboksi.

        ![][3]

    1. Valige rippmenüüst loendi **Uue teenuse lõpp-punkti** **Azure** .

        ![][4]

    1.  **Lisage Azure tellimuse** dialoogiboksis suvand **Teenuse põhilise** .

        ![][5]

    1.  Dialoogiboksi **Lisamine Azure tellimuse** Azure tellimuse teabe lisada. Teil on vaja järgmisi üksusi:
        -   Tellimuse Id
        -   Tellimuse nimi
        -   Teenuse põhilise Id
        -   Teenuse põhilise võti
        -   Rentniku Id

    1.  Teie valitud nimi lisada välja **tellimuse** nimi. See väärtus kuvatakse hiljem **Azure tellimuse** ripploendit VS meeskonnatöö teenused. 

    1.  Kui te ei tea oma Azure tellimuse ID-d, saate ühte järgmistest käskudest hankimine.
        
        PowerShelli skriptide, kasutage:

        `Get-AzureRmSubscription`

        Azure'i CLI, kasutage:

        `azure account show`
    

    1.  Teenuse põhilise ID saamiseks teenuse põhilise võti ja rentniku ID, järgige juhiseid artiklis [loomine Active Directory rakenduste ja teenuste põhisumma kasutades portaalis](resource-group-create-service-principal-portal.md) või [teenuse põhilise Azure'i ressursihaldur autentimist](resource-group-authenticate-service-principal.md).

    1.  Dialoogiboksi **Lisamine Azure tellimuse** teenuse põhilise ID, teenuse põhilise võti ja rentniku ID väärtused lisada, ja seejärel klõpsake nuppu **OK** .

        Nüüd on teil lubatud teenuse põhilise Azure PowerShelli skripti abil.

1.  Koosta määratlust redigeerida ja valige **Azure PowerShelli** Koosta etappi. Valige rippmenüüst loendi **Azure tellimuse** tellimuse. (Kui tellimus pole kuvatud, valige nupp **Värskenda** järgmine link **Halda** .) 

    ![][8]

1.  Sisestage Deploy-AzureResourceGroup.ps1 PowerShelli skripti tee. Selleks **Skripti tee** kõrval kolmikpunkti (…) nuppu, liikuge Deploy-AzureResourceGroup.ps1 PowerShelli skripti projekti **skriptide** kausta, valige see ja seejärel klõpsake nuppu **OK** . 

    ![][9]

1. Kui olete valinud skripti, värskendage tee skripti nii, et see käivitatakse Build.StagingDirectory (sama kataloogi *ArtifactsLocation* on seatud). Saate seda teha, lisades "$(Build.StagingDirectory)/"skripti tee alguseni.

    ![][10]

1.  Sisestage väljale **Skripti argumendid** (üks rida) järgmised parameetrid. Skripti käivitamisel Visual Studios, näete, kuidas VS kasutab parameetrid **väljundi** aknas. Saate kasutada seda lähtepunktina häälestuse oma Koosta etapis parameetrite väärtused.

  	| Parameetri | Kirjeldus|
  	|---|---|
  	| -ResourceGroupLocation           | Geograafilise asukoha väärtus, kus ressursirühma asub, nt **eastus** või **"Ida-USA"**. (Lisada ülakomadega, kui nimes on tühik.) Lisateavet leiate [Azure'i regioonid](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Ressursirühm, kasutatakse selle juurutamiseks nimi.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | See parameeter esitus, kui määrab, et esemeid on vaja Azure üles laadida: Kohalik süsteem. Ainult peate määrama selle parameetri kui juurutamise malli jaoks on vaja täiendavat esemeid soovitud etapp, kasutades PowerShelli skripti (nt konfiguratsiooni skriptide või pesastatud mallid).                                                                                                                                                                 |
  	| -StorageAccountName              | Salvestusruumi konto, mida kasutatakse etapi esemeid selle juurutamiseks nimi. See parameeter on nõutav ainult juhul, kui kopeerite esemeid Azure. Selle konto salvestusruumi pole luuakse automaatselt juurutamise teel, peab see juba olemas.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Salvestusruumi kontoga seotud ressursside rühma nimi. See parameeter on nõutav ainult juhul, kui kopeerite esemeid Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | Azure'i ressursirühm juurutamise projekti malli faili tee. Paindlikkuse suurendamiseks kasutada see parameeter, mis on sõltuv PowerShelli skripti asemel absoluutne tee asukoha tee.|
  	| -TemplateParametersFile          | Azure'i ressursirühm juurutamise projekti parameetrid faili tee. Paindlikkuse suurendamiseks kasutada see parameeter, mis on sõltuv PowerShelli skripti asemel absoluutne tee asukoha tee.|
  	| -ArtifactStagingDirectory        | Selle parameetri abil soovitud PowerShelli skripti tea kaust, kuhu projekti kahendarvu faile kopeerida. See väärtus alistab kasutatavaid PowerShelli skripti vaikeväärtus. VS meeskonnatöö teenuseid kasutada, seadke väärtus: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Siin on script argumendid näide (rea loetavuse ei tööta).

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Kui olete lõpetanud, **Skripti argumendid** välja näeb välja järgmine.

    ![][11]

1.  Pärast seda, kui olete lisanud kõik nõutavad üksused Azure PowerShelli koostada samm, valige **järjekorra** koostamine nuppu projekti koostamiseks. **Koostage** ekraanil kuvatakse PowerShelli skripti väljund.

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet Azure ressursihaldur ja Azure ressursirühma [Azure'i ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md) .


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
