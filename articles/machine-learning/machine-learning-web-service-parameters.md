<properties 
    pageTitle="Azure'i masina Õppekeskuse Web teenuse parameetrite kasutamine | Microsoft Azure'i" 
    description="Kuidas kasutada Azure seadme õ Web teenuse parameetrite muutmiseks mudelisse toimimist, kui veebiteenuse on juurdepääs." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Azure'i masina Õppekeskuse Web teenuse parameetrite kasutamine

Teenuse Azure seadme õ web luuakse katse, mis sisaldab moodulid konfigureeritav parameetritega avaldamise kaudu. Mõnel juhul soovite muuta mooduli käitumine veebiteenuse töötamise ajal. *Web teenuse parameetrite* abil, saate selle toimingu tegemiseks. 

[Andmete importimine] häälestamine on näide[ reader] mooduli nii, et kasutaja avaldatud veebiteenuse saate määrata muud andmeallikat, kui veebiteenuse on juurdepääs. Või konfigureerida [Andmete eksportimine] [ writer] mooduli nii, et eri sihtkoha saab määrata. Mõned näited sisaldavad määratud arvu bittide muutmise [Funktsiooni räsi] [ feature-hashing] mooduli või arvu soovitud funktsioonide [filtri-põhiste funktsioonide valik] [ filter-based-feature-selection] mooduli. 

Saate määrata Web teenuse parameetrid ja neid seostada ühe või mitme mooduli parameetrite oma katse ja saate määrata, kas need on nõutav või valikuline. Veebiteenuse kasutaja saab seejärel andke väärtused järgmiste parameetrite veebiteenuse helistamisel. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Kuidas määrata ja Web teenuse parameetrite kasutamine

Määratlege veebiteenuse parameeter välja parameetri moodul kõrval olevat ikooni ja valige "Seadmine veebiteenuse parameeter". See loob uue veebiteenuse parameetrit ja ühendub selle mooduli parameeter. Kui veebiteenus on kättesaadav, kasutaja saab määrata väärtuse jaoks veebiteenuse parameeter ja rakendatakse see parameeter mooduli.

Kui määratlete veebiteenuse parameeter, on mõni muu mooduli parameetri katse saadaval. Kui määratlete ühe mooduli parameeter on seostatud veebiteenuse parameeter, saate seda sama veebiteenuse parameeter moodul, kui parameetri eeldab, et sama tüüpi väärtuse. Näiteks kui veebiteenuse parameeter pole arvväärtus, siis seda saab kasutada ainult mooduli parameetrite, mida oodata arvulise väärtuse. Kui kasutaja määrab väärtuse jaoks veebiteenuse parameeter, seotakse kõik seotud mooduli parameetrid.

Saate määrata, kas vaikeväärtuse ette veebiteenuse parameeter. Kui te ei tee, on kasutaja veebiteenuse valikuline parameeter. Kui te ei esita vaikeväärtuse, siis kasutaja peab sisestage väärtus, kui veebiteenuse on juurdepääs.

Veebiteenuse dokumentatsiooni sisaldab teavet web teenuse kasutaja kohta, kuidas määrata veebiteenuse parameeter programmiliselt veebiteenuse kasutamisel.

>[AZURE.NOTE] Klassikaline veebiteenuse dokumentatsiooni on esitatud veebiteenuse **ARMATUURLAUA** masina õ Studios **API aitab lehe** lingi kaudu. Uue veebiteenuse dokumentatsiooni on esitatud lehtedel **tarbida** ja **Ärplema API** [Azure seadme õ veebiteenuste](https://services.azureml.net/Quickstart) portaali kaudu oma veebiteenuse.


##<a name="example"></a>Näide

Näiteks Oletame meil on [Andmete eksportimine] katse[ writer] moodul, mis saadab Azure'i bloobimälu. Me määratleda nimega "Bloobivahemälu tee" veebiteenuse parameeter, mis võimaldab web teenuse kasutaja muutmiseks tee soovitud bloobimälu kui teenus on kättesaadav.

1.  Seadme õ Studios, klõpsake [Andmete eksportimine] [ writer] mooduli selle valimiseks. Katse lõuend paremal paanil atribuudid kuvatakse selle atribuute.

2.  Tüübi talletusruumi määramiseks tehke järgmist.

    - Valige jaotises **Palun määra andmete sihtkohta**, "Azure'i bloobimälu".
    - Valige jaotises **Palun määrata autentimistüüp**, "Konto".
    - Azure'i bloobimälu kontoteabe sisestamine. 
    <p />

3.  Klõpsake paremas servas **tee alguses container parameetriga Bloobivahemälu**ikooni. See näeb välja umbes järgmine:

    ![Web teenuse Päringuparameetri ikoon][icon]

    Valige "Seadmine veebiteenuse parameeter".

    Kirje lisatakse jaotises **Web teenuse parameetrite** allosas nimega "Tee Bloobivahemälu algavad sõnadega container" paanil atribuudid. Mis on nüüd veebiteenuse parameeter on seostatud [Andmete eksportimine] [ writer] mooduli parameeter.

4.  Veebiteenuse parameeter ümbernimetamiseks klõpsake nime, sisestage "Bloobivahemälu tee" ja vajutage sisestusklahvi ( **Enter** ). 
 
5.  Vaikimisi ette veebiteenuse parameeter, klõpsake funktsiooni nimest paremal ikooni, valige "Osutab vaikeväärtus", sisestage väärtus (nt "container1/output1.csv") ja vajutage sisestusklahvi ( **Enter** ).

    ![Veebiteenuse parameeter][parameter]

6.  Klõpsake nuppu **Käivita**. 

7.  Klõpsake **Juurutada veebiteenuse** ja valige **Juurutamine veebiteenuse [Classic]** või **Juurutada veebiteenuse [uus]** veebiteenuse juurutamine.

Kasutaja veebiteenuse nüüd saate määrata uut sihtkohta [Andmete eksportimine] [ writer] mooduli veebiteenuse kasutamisel.

##<a name="more-information"></a>Lisateave

Vaadake üksikasjalikumat näiteks [Web teenuse parameetrite](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) kirjet [Masina õ ajaveebi](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Juurdepääs seadme Õppekeskuse veebiteenuse kohta leiate lisateavet teemast [peab olema avaldatud masina Õppekeskuse veebiteenus](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
