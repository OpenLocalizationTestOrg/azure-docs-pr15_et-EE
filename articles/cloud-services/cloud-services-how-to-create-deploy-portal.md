<properties
    pageTitle="Kuidas luua ja juurutada pilveteenus | Microsoft Azure'i"
    description="Saate teada, kuidas luua ja juurutada pilveteenus Azure portaalis."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Kuidas luua ja juurutada pilveteenus

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-create-deploy-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-create-deploy.md)

Azure'i portaalis on kaks võimalust, saate luua ja juurutada pilveteenus: *Kiire loomine* ja *Kohandatud loomine*.

Selles artiklis selgitatakse, kuidas kiiresti luua meetodi abil luua uue pilveteenus ja seejärel kasutage üles laadida ja juurutada Azure pilveteenuse pakett **üles laadida** . Kui seda meetodit kasutada Azure portaali teeb saadaval mugav linke, kui lähete kõik nõuete täitmiseks. Kui olete valmis kasutama oma pilveteenuses loomist, saate teha nii kaudu, luua kohandatud samal ajal.

> [AZURE.NOTE] Kui kavatsete avaldada oma pilveteenuses kaudu Visual Studio meeskonnatöö teenused (VSTS), kasutage kiiresti luua, ja seejärel häälestamine VSTS avaldamise Azure'i Kiirjuhend või armatuurlaua. Lisateabe saamiseks lugege teemat [Pidev kohaletoimetamise Azure abil Visual Studio meeskonnatöö teenused][TFSTutorialForCloudService], või vaadake **Kiirkäivituse** lehe spikker.

## <a name="concepts"></a>Mõisted
Juurutamiseks pilveteenus Azure registreerimiseks vajalikud kolmest osast:

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

- Kui soovite kasutada mõnda pilveteenusesse, mis kasutab turvasoklite kiht (SSL) andmete krüptimine, [konfigureerida rakenduse](cloud-services-configure-ssl-certificate-portal.md#modify) jaoks SSL-i.

- Kui soovite konfigureerida kaugtöölaua ühendusi rolli aknad, kaugtöölaua [konfigureerimine rollid](cloud-services-role-enable-remote-desktop.md) . Seda saab teha ainult klassikaline portaalis.

- Kui soovite konfigureerida oma pilveteenuses jälgimine Paljusõnaline, võimaldavad Azure'i diagnostika pilveteenusesse. *Minimaalne jälgimine* (vaikimisi jälgida taset) kasutab jõudluse hinnale kogutud host opsüsteemide rolli eksemplaride (virtuaalmasinates). *Verbose jälgimise* kogutakse täiendavaid mõõdikute põhjal jõudlusandmeid rolli aknad lähemalt analüüsida probleemid, mis ilmnevad rakenduse töötlemise ajal lubamiseks sees. Azure'i diagnostika lubamise kohta leiate teemast [Azure lubamine diagnostika](cloud-services-dotnet-diagnostics.md).

Luua pilveteenus juurutuste web rollid või töötaja rollid, peate [teenuse paketi loomine](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Enne alustamist

- Kui teil pole installitud Azure'i SDK, **Installige Azure'i SDK** [Azure'i allalaadimiste lehe](https://azure.microsoft.com/downloads/)avamiseks klõpsake ja SDK, milles soovite töötada oma keele alla laadida. (Peate selleks hiljem võimalus.)

- Kui esinemisvõimaluse rolli sert, luua serdid. Pilveteenustega vajavad privaatvõti pfx-fail. [Saate üles laadida serdid Azure]() ajal luua ja juurutada pilveteenusesse.

- Kui kavatsete juurutada pilveteenusesse osaleja rühma, osaleja rühma loomine. Mõne osaleja rühma abil saate oma pilveteenuses ja muude Azure'i teenuste juurutamine piirkonnas samasse asukohta. Saate luua osaleja rühma **võrkude** alal Azure klassikaline portaalis lehel **osaleja rühmad** .


## <a name="create-and-deploy"></a>Loomine ja juurutamine

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake **uus > Virtuaalmasinates**, ja seejärel kerige allapoole ja klõpsake **Pilveteenuses**.

    ![Avaldada oma pilveteenuses](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Teabeleht, kus on kuvatud allservas nuppu **Loo**. 
4. Sisestage uue **Pilveteenuses** labale **DNS-i nimi**väärtus.
5. Luua uue **Ressursirühma** või valida mõne olemasoleva.
6. Valige **asukoht**.
7. Klõpsake **pakett**. See avab **paketi üleslaadimine** tera. Täitke nõutavad väljad.  

     Kui mõni teie rollid sisaldavad ühte eksemplari, veenduge, et **juurutada isegi juhul, kui üks või mitu rollid sisaldavad ühekordsest** on valitud.

8. Veenduge, et valitud oleks **Start juurutamise** .
9. Klõpsake nuppu **OK** suletakse **paketi üleslaadimine** tera.
10. Kui teil on, mis tahes serdid lisada, klõpsake nuppu **Loo**.

    ![Avaldada oma pilveteenuses](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Laadige üles sert

Kui teie juurutamise pakett on [konfigureeritud kasutama serdid](cloud-services-configure-ssl-certificate-portal.md#modify), saate laadida serdi kohe.

1. Valige **serdid**, ja enne **Lisa serdid** , valige SSL-i serdi pfx-faili ja seejärel andke **parooli** serti,
2. Klõpsake nuppu **Manusta sert**ja klõpsake **OK** enne **Lisa serdid** .
3. Klõpsake nuppu **Loo** enne **Pilveteenuses** . Kui juurutamise on **valmis** olek, jätkake järgmiste juhiste juurde.

    ![Avaldada oma pilveteenuses](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Lõpule viidud juurutamise kontrollimine

1. Klõpsake cloud eksemplari.

    Olek peaks kuva, et teenus ei **tööta**.

2. Klõpsake jaotises **Essentialsi**, **Saidi URL-i** oma pilveteenuses avamiseks veebibrauseris.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Järgmised sammud

* [Üldine konfigureerimine oma pilveteenuses](cloud-services-how-to-configure-portal.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name-portal.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage-portal.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate-portal.md)konfigureerimine.