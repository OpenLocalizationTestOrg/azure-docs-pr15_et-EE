<properties
    pageTitle="Kuidas luua ja juurutada pilveteenus | Microsoft Azure'i"
    description="Saate teada, kuidas luua ja juurutada pilveteenus Azure kiiresti luua meetodi abil."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Kuidas luua ja juurutada pilveteenus

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-create-deploy-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-create-deploy.md)

Azure'i klassikaline portaali pakub kahte võimalusi, kuidas luua ja juurutada pilveteenus: **Kiire loomine** ja **Kohandatud loomine**.

See teema selgitab, kuidas kiiresti luua meetodit abil saate luua uue pilveteenuses ja seejärel kasutage üles laadida ja juurutada Azure pilveteenuse pakett **üles laadida** . Kui seda meetodit kasutada Azure klassikaline portaali teeb saadaval mugav linke, kui lähete kõik nõuete täitmiseks. Kui olete valmis kasutama oma pilveteenuses loomist, saate teha nii kaudu, **Luua kohandatud**samal ajal.

> [AZURE.NOTE] Kui kavatsete avaldada oma pilveteenuses kaudu Visual Studio meeskonnatöö teenused (VSTS), kasutage kiiresti luua, ja seejärel häälestamine VSTS avaldamise **Kiirkäivituse** või armatuurlaua. Lisateabe saamiseks lugege teemat [Pidev kohaletoimetamise Azure abil Visual Studio meeskonnatöö teenused][TFSTutorialForCloudService], või vaadake **Kiirkäivituse** lehe spikker.

## <a name="concepts"></a>Mõisted
Kolm osa on vaja juurutada pilveteenus Azure taotluse:

- **Teenuse määratlus**  
  Pilveteenuse teenuse definitsioonifail (.csdef) määratleb teenuse mudeli, sh rollide arvu.

- **Teenuse konfigureerimine**  
  Pilveteenuse teenuse konfiguratsioonifail (.cscfg) pakub konfiguratsioonisätted pilve teenus ja üksikute rollid, sh rolli eksemplaride arv.

- **Pakett**  
  Pakett (.cspkg) sisaldab rakenduse koodi ja konfiguratsioone ja teenuse määratluse faili.
  
Lisateavet teavet paketi loomiseks ja need [siin](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Rakenduse ettevalmistamine
Pilveteenus juurutamiseks peate looma oma rakenduse koodi ja pilvepõhise teenuse konfiguratsioonifail (.cscfg) pilveteenuse pakett (.cspkg). Azure'i SDK tööriistade jaoks ettevalmistamiseks vajalik juurutamise need failid. Saate installida [Azure allalaadimiste](https://azure.microsoft.com/downloads/) lehe, kus soovite töötada oma rakenduse keeles SDK.

Kolm pilvepõhise teenuse funktsioonid nõuavad teisiti konfiguratsioone enne eksportimist pakett:

- Kui soovite kasutada mõnda pilveteenusesse, mis kasutab turvasoklite kiht (SSL) andmete krüptimine, [konfigureerida rakenduse](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) jaoks SSL-i.

- Kui soovite konfigureerida kaugtöölaua ühendusi rolli aknad, kaugtöölaua [konfigureerimine rollid](cloud-services-role-enable-remote-desktop.md) .

- Kui soovite konfigureerida oma pilveteenuses jälgimine Paljusõnaline, võimaldavad Azure'i diagnostika pilveteenusesse. *Minimaalne jälgimine* (vaikimisi jälgida taset) kasutab jõudluse hinnale kogutud host opsüsteemide rolli eksemplaride (virtuaalmasinates). "Paljusõnaline jälgimine * kogutakse täiendavaid mõõdikute põhjal jõudlusandmeid rolli aknad lähemalt analüüsida probleemid, mis ilmnevad rakenduse töötlemise ajal lubamiseks sees. Azure'i diagnostika lubamise kohta leiate teemast [Azure diagnostika lubamine](cloud-services-dotnet-diagnostics.md).

Luua pilveteenus juurutuste web rollid või töötaja rollid, peate [teenuse paketi loomine](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Enne alustamist

- Kui teil pole installitud Azure'i SDK, **Installige Azure'i SDK** [Azure'i allalaadimiste lehe](https://azure.microsoft.com/downloads/)avamiseks klõpsake ja SDK, milles soovite töötada oma keele alla laadida. (Peate selleks hiljem võimalus.)

- Kui esinemisvõimaluse rolli sert, luua serdid. Pilveteenustega vajavad privaatvõti pfx-fail. Saate [üles laadida serdid Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) loomisel ja juurutamine pilveteenusesse.

- Kui kavatsete juurutada pilveteenusesse osaleja rühma, osaleja rühma loomine. Mõne osaleja rühma abil saate oma pilveteenuses ja muude Azure'i teenuste juurutamine piirkonnas samasse asukohta. Saate luua osaleja rühma **võrkude** alal Azure klassikaline portaalis lehel **osaleja rühmad** .


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Kuidas: loomine abil kiiresti luua pilveteenus

1. [Azure'i klassikaline portaalis](http://manage.windowsazure.com/), klõpsake nuppu **Uus**>**arvutada**>**Pilveteenusesse**>**Kiiresti luua**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. Sisestage **URL-i**kasutamine avaliku URL-i juurdepääsuks oma pilveteenuses tootmise kasutuselevõttu alamdomeen nimi. URL-i vorming tootmise juurutuste on: http://*myURL*. cloudapp.net.

3. Valige **piirkonnas või osaleja**geograafilised piirkond või osaleja rühma juurutamiseks pilveteenuses, et. Osaleja rühma valimine, kui soovite oma pilveteenuses juurutama muude Azure'i teenuste piirkonna samasse asukohta.

4. Klõpsake **loomine pilveteenuses**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Saate jälgida olekut protsessi sõnumi alale akna allosas.

    Avaneb **Pilveteenustega** ala, kus kuvatakse uus pilveteenuses. Kui olekuks loodud, cloud teenuste loomiseks on lõpule viidud.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Kuidas: pilveteenus sertifikaat üleslaadimine

1. [Azure'i klassikaline portaalis](http://manage.windowsazure.com/), klõpsake **Pilveteenustega**, klõpsake pilveteenusesse nime ja klõpsake **serdid**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Klõpsake **serdi üleslaadimine** või **üles laadida**.

3. **Fail**, **otsige** valimiseks kasutage serdi (pfx-fail).

4. Sisestage tekstiväljale **parool**privaatvõti serti.

5. Klõpsake nuppu **OK** (märge).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Saate vaadata edenemise üleslaadimise sõnumiala allpool näidatud. Kui üleslaadimine on lõpule jõudnud, lisatakse tabelisse sert. Sõnumi alale, klõpsake nuppu OK teate sulgemiseks.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Kuidas: pilveteenus juurutamine

1. [Azure'i klassikaline portaalis](http://manage.windowsazure.com/), klõpsake **Pilveteenustega**, klõpsake pilveteenusesse nime ja klõpsake **armatuurlaua**.

2. Klõpsake **uue tootmise juurutamise üleslaadimine** või **üles laadida**.

3. **Juurutamise sildi**, sisestage nimi uue juurutamiseks – näiteks MyCloudServicev4.

3. **Paketi**, **sirvige** valimiseks kasutage teenuse paketifaili (.cspkg) kasutada.

4. **Konfiguratsiooni**, **sirvige** valimiseks kasutage teenuse konfigureerimine faili (.cscfg) kasutada.

5. Kui pilveteenusesse sisaldab ainult ühte eksemplari koos mis tahes rollid, märkige ruut **juurutamine isegi juhul, kui üks või mitu rollid sisaldavad ühte eksemplari** , juurutamise jätkamiseks lubamiseks.

    Azure'i saate tagada ainult 99,95% juurdepääs pilveteenusesse värskendused hoolduse ajal, kui iga rolli on vähemalt kaks korda. Vajadusel saate lisada täiendavad rolli aknad **skaala** lehel pärast juurutamist pilveteenusesse. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).

6. Klõpsake nuppu **OK** (märge) pilvepõhise teenuse kasutamise alustamiseks.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Saate jälgida olekut juurutamise sõnumi alale. Sõnumi peitmiseks klõpsake nuppu OK.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Lõpule viidud juurutamise kontrollimine

1. Valige **armatuurlaud**.

    Olek peaks kuva, et teenus ei **tööta**.

2. Klõpsake jaotises **kiire ülevaade**, avage oma pilveteenuses veebibrauseris saidi URL-i.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Järgmised sammud

* [Üldine konfigureerimine oma pilveteenusesse](cloud-services-how-to-configure.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate.md)konfigureerimine.