<properties 
    pageTitle="SSL-i konfigureerimine mõnda pilveteenusesse (klassikaline) | Microsoft Azure'i" 
    description="Siit saate teada, HTTPS-i lõpp-punkti web rolli määramine ja kuidas tagada rakenduse SSL-serdi üleslaadimine." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Rakenduse Azure SSL-i konfigureerimine

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-configure-ssl-certificate-portal.md)
- [Azure'i klassikaline portaal](cloud-services-configure-ssl-certificate.md)

Turvaline turvasoklite kiht (SSL) krüptimine on kinnitamise andmed internetis kõige sagedamini kasutatav meetod. Ülesande käsitletakse HTTPS-i lõpp-punkti web rolli määramine ja kuidas tagada rakenduse SSL-serdi üleslaadimine.

> [AZURE.NOTE] Selles ülesandes toiminguid rakendamine Azure'i pilveteenustega; Rakenduse teenuste, lugege [artiklit](../app-service-web/web-sites-configure-ssl-certificate.md) .

Selles ülesandes kasutab tootmise juurutamine. Lavastus juurutamise kasutamise kohta teavet selle teema lõpus.

Lugege [seda](cloud-services-how-to-create-deploy.md) artiklit esmalt, kui te pole veel loonud mõnda pilveteenusesse.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Samm 1: Saada SSL-sert

Rakenduse konfigureerimiseks SSL-i, peate esmalt saada allkirjastatud, sertimiskeskuse (CA) väljastatud serti, usaldusväärse kolmanda osapoole, kes probleemid serdid selleks SSL-sert. Kui te ei ole veel üks, peate hankima SSL-sertide müüvast ettevõttest.

Serdi peab vastama järgmistele nõuetele SSL-sertide Azure.

-   Serdi peab sisaldama privaatvõti.
-   Serdi peab olema loodud võtme Exchange'i, eksporditav isikuandmete vahetus (pfx) faili.
-   Selle serdi subjekti nimi peab vastama domeeni kasutada pilveteenusesse. Sertimiskeskus (CA) cloudapp.net domeeni ei saa SSL-serdi hangib. Viimistlemine kohandatud domeeninime kasutada, kui teie teenuse. Kui te CA serdi päring, serdi subjekti nimi peab vastama kohandatud domeeni nimi, mida kasutatakse teie taotlus. Näiteks kui teie domeeni nimi on **contoso.com** oleks serdi taotlemiseks oma ca jaoks * **. contoso.com** või * *www.contoso.com**.
-   Serdi tuleb kasutada vähemalt 2048-bitine krüptimine.

Katsetamiseks, saate [luua](cloud-services-certs-create.md) ja kasutada iseallkirjastatud sert. Iseallkirjastatud serdi pole autenditud CA kaudu ja saate kasutada domeeni cloudapp.net veebisaidi URL-i. Näiteks järgmise toimingu kasutatakse iseallkirjastatud serdi, kus on kasutusel serdi levinud nimi (CN) **sslexample.cloudapp.net**.

Järgmiseks peate lisama serdi teabe teenuse määratlus ja teenuse konfiguratsiooni faile.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Samm 2: Muuta teenuse määratlus ja konfiguratsiooni failid

Rakenduse peab olema konfigureeritud kasutama serti ja HTTPS-i lõpp-punkti tuleb lisada. Selle tulemusena teenuse määratlus ja teenuse konfiguratsiooni faile on vaja uuendada.

1.  Teie arenduskeskkond avage teenuse definitsioonifail (CSDEF), **WebRole** jaotise **serdid** jaotise lisamine ja serdi (ja vahe serdid) kohta järgmine teave:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    Jaotise **serdid** määratleb nimi meie sert, selle asukoht ja nimi, kus see asub pood.
    
    Õiguste (`permisionLevel` atribuut) saate määrata ühe järgmistest väärtustest:

  	| Luba väärtus  | Kirjeldus |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Vaikimisi)** Kõik rolli protsessid pääsevad privaatvõti. |
  	| laiendatud          | Ainult administraatoriõigustes protsesside pääsevad privaatvõti.|

2.  Teenuse määratlus failis element **InputEndpoint** lubamiseks HTTPS-i **lõpp-punktid** jaotise lisamiseks tehke järgmist.

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Lisage teenuse definitsioonifail **Köitmine** elemendi **saitide** jaotisse. Selles jaotises lisab HTTPS-sidumise vastendamiseks lõpp-punkti saidile:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Teenuse määratluse faili kõik vajalikud muudatused on lõpule viidud, kuid peate endiselt teenuse konfiguratsioonifail serdi teabe lisamine.

4.  Teenuse konfiguratsiooni faili (CSCFG) ServiceConfiguration.Cloud.cscfg, **rolli** jaotisse, asendades valimi sõrmejälje väärtus, et teie serdi allpool näidatud jaotise **serdid** lisamiseks tehke järgmist.

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Eelmises näites kasutatakse **sha1** algoritmi sõrmejälje. Määrake sobiv väärtus teie serdi sõrmejälje algoritmile.)

Nüüd, kui teenuse määratlus ja teenuse konfigureerimine failide värskendatud paketti juurutamise Azure'i üleslaadimiseks. Kui kasutate **cspack**, ärge kasutage **/generateConfigurationFile** lipu, nagu mis kirjutab enda lisatud serdi teabe üle.

## <a name="step-3-upload-a-certificate"></a>Samm 3: Laadige üles sert

Teie juurutamise pakett on värskendatud kasutada serti ja HTTPS-i lõpp-punkti on lisatud. Nüüd saate üles laadida paketi ja serdi Azure'i Azure klassikaline portaalis.

1. [Azure'i klassikaline portaali][]sisse logida. 
2. Klõpsake vasakul navigeerimispaanil **Pilveteenustega** .
3. Klõpsake soovitud pilveteenuses.
4. Klõpsake vahekaarti **serdid** .

    ![Klõpsake vahekaarti Serdid](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klõpsake nuppu **üles laadida** .

    ![Laadi üles](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. **Faili** **parooli**, sisestage seejärel klõpsake nuppu **valmis** (märge).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Samm 4: Ühenduse rolli eksemplari HTTPS-i abil

Nüüd, kui teie juurutusega on tööks Azure, saate luua ühenduse HTTPS-i abil.

1.  Azure'i klassikaline portaalis Valige juurutamise ja seejärel klõpsake linki **Saidi URL-i**all.

    ![Saidi URL-i][2]

2.  Teie veebibrauseris, muuta kasutada **https** asemel **http**link ja seejärel külastage lehte.

    >[AZURE.NOTE] Kui kasutate iseallkirjastatud serdi, HTTPS-i lõpp, mis on seotud iseallkirjastatud serdi sirvimisel võidakse kuvada serdi viga brauseris. Usaldusväärne sertimiskeskus allkirjastatud serdiga kõrvaldab selle probleemi; Vahepeal, võite ignoreerida viga. (Teine võimalus on kasutaja usaldusväärne sert asutuse serdi poe iseallkirjastatud serdi lisamine.)

    ![SSL-i näites veebisait][3]

Kui soovite kasutada SSL-i asemel tootmise juurutamise lavastus juurutamiseks, peate esmalt URL-i kasutatakse lavastus juurutamiseks. Juurutada ilma sert või serdi teavet, sh oma pilveteenuses lavastus keskkonnas. Kui juurutada, saate määrata GUID-põhiste URL, mis on Azure klassikaline portaali **Saidi URL-i** väljal loetletud. Serdi loomine levinud nimega (CN) võrdne GUID-põhiste URL (nt **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Azure'i klassikaline portaali abil saate lisada serdi oma etapiviisilist pilveteenusesse. Seejärel lisage serdi teavet failide CSDEF ja CSCFG, pakkima rakenduse ja värskendage etapiviisilist juurutamise uue paketi kasutamiseks.

## <a name="next-steps"></a>Järgmised sammud

* [Üldine konfigureerimine oma pilveteenusesse](cloud-services-how-to-configure.md).
* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage.md).


  [Azure'i klassikaline portaal]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
