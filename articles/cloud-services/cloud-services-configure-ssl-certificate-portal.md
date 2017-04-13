<properties 
    pageTitle="SSL-i konfigureerimine pilveteenus | Microsoft Azure'i" 
    description="Siit saate teada, HTTPS-i lõpp-punkti web rolli määramine ja kuidas tagada rakenduse SSL-serdi üleslaadimine. Nendes näidetes kasutatakse Azure portaali." 
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

Turvaline turvasoklite kiht (SSL) krüptimine on kinnitamise andmete Internetis kõige sagedamini kasutatav meetod. Ülesande käsitletakse HTTPS-i lõpp-punkti web rolli määramine ja kuidas tagada rakenduse SSL-serdi üleslaadimine.

> [AZURE.NOTE] Selles ülesandes toiminguid rakendamine Azure'i pilveteenustega; Rakenduse teenused, leiate [selle](../app-service-web/web-sites-configure-ssl-certificate.md).

Selles ülesandes kasutab tootmise juurutamise; lavastus juurutamise kasutamise kohta teavet selle teema lõpus.

Lugemine [see](cloud-services-how-to-create-deploy-portal.md) esmalt kui te pole veel loonud mõnda pilveteenusesse.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Samm 1: Saada SSL-sert

Rakenduse konfigureerimiseks SSL-i, peate esmalt saada allkirjastatud, sertimiskeskuse (CA) väljastatud serti, usaldusväärse kolmanda osapoole, kes probleemid serdid selleks SSL-sert. Kui te ei ole veel üks, peate hankima SSL-sertide müüvast ettevõttest.

Serdi peab vastama järgmistele nõuetele SSL-sertide Azure.

-   Serdi peab sisaldama privaatvõti.
-   Serdi peab olema loodud võtme Exchange'i, eksporditav isikuandmete vahetus (pfx) faili.
-   Selle serdi subjekti nimi peab vastama domeeni kasutada pilveteenusesse. Sertimiskeskus (CA) cloudapp.net domeeni ei saa SSL-serdi hangib. Viimistlemine kohandatud domeeninime kasutada, kui teie teenuse. Kui CA serdi päring serdi subjekti nimi peab vastama kohandatud domeeni nimi, mida kasutatakse teie taotlus. Näiteks kui teie domeeni nimi on **contoso.com** oleks serdi taotlemiseks oma ca jaoks * **. contoso.com** või * *www.contoso.com**.
-   Serdi tuleb kasutada vähemalt 2048-bitine krüptimine.

Katsetamiseks, saate [luua](cloud-services-certs-create.md) ja kasutada iseallkirjastatud sert. Iseallkirjastatud serdi pole autenditud CA kaudu ja saate kasutada domeeni cloudapp.net veebisaidi URL-i. Näiteks alltoodud tööülesande kasutatakse iseallkirjastatud serdi, kus on kasutusel serdi levinud nimi (CN) **sslexample.cloudapp.net**.

Järgmiseks peate lisama serdi teabe teenuse määratlus ja teenuse konfiguratsiooni faile.

<a name="modify"> </a>
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
    
    Õiguste (`permisionLevel` atribuut) saab panna ühte järgmistest:

  	| Luba väärtus  | Kirjeldus |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Vaikimisi)** Kõik rolli protsessid pääsevad privaatvõti. |
  	| administraatoriõigustes          | Ainult administraatoriõigustes protsesside pääsevad privaatvõti.|

2.  Teenuse määratluse failis element **InputEndpoint** lubamiseks HTTPS-i **lõpp-punktid** jaotise lisamiseks tehke järgmist.

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Lisage teenuse definitsioonifail **Köitmine** elemendi **saidid** jaotisse. Sellega lisatakse HTTPS-sidumise vastendamiseks lõpp-punkti saidile:

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

    Kõik vajalikud muudatused teenuse määratluse faili on lõpule viidud, kuid peate endiselt teenuse konfiguratsioonifail serdi teabe lisamine.

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

(Ülaltoodud näites kasutatakse **sha1** algoritmi sõrmejälje. Määrake sobiv väärtus teie serdi sõrmejälje algoritmile.)

Nüüd, kui teenuse määratlus ja teenuse konfigureerimine failide värskendatud paketti juurutamise Azure'i üleslaadimiseks. Kui kasutate **cspack**, ärge kasutage **/generateConfigurationFile** lipu, nagu mis kirjutab äsja sisestatud teave serdi.

## <a name="step-3-upload-a-certificate"></a>Samm 3: Laadige üles sert

Ühenduse loomine portaali ja...

1. Valige oma pilveteenuses portaalis, valige **Pilveteenuses**. (Mis pole jaotises **kõik ressursid** .) 
    
    ![Avaldada oma pilveteenuses](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klõpsake nuppu **serdid**.

    ![Klõpsake ikooni serdid](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. **Faili** **parooli**, sisestage seejärel nuppu **üles laadida**.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Samm 4: Ühenduse rolli eksemplari HTTPS-i abil

Nüüd, kui teie juurutusega on tööks Azure, saate luua ühenduse HTTPS-i abil.
    
1.  Klõpsake **Saidi URL-i** veebibrauseris avada.

    ![Klõpsake saidi URL-i](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2.  Teie veebibrauseris, muuta kasutada **https** asemel **http**link ja seejärel külastage lehte.

    >[AZURE.NOTE] Kui kasutate iseallkirjastatud serdi, sirvimisel on seostatud iseallkirjastatud sert kuvatakse serdi viga brauseris HTTPS-i lõpp-punkt. Usaldusväärne sertimiskeskus allkirjastatud serdiga kõrvaldab selle probleemi; Vahepeal, võite ignoreerida viga. (Teine võimalus on kasutaja usaldusväärne sert asutuse serdi poe iseallkirjastatud serdi lisamine.)

    ![Saidi eelvaade](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

    >[AZURE.TIP] Kui soovite kasutada SSL-i asemel tootmise juurutamise lavastus juurutamiseks, peate esmalt URL, mida kasutatakse lavastus juurutamise määratlemiseks. Kui kasutusele võetud oma pilveteenuses, määratakse lavastus keskkonna URL-i **Juurutamise ID** GUID järgmises vormingus:`https://deployment-id.cloudapp.net/`  
      
    >Serdi loomine levinud nimega (CN) võrdne GUID-põhiste URL (nt **328187776e774ceda8fc57609d404462.cloudapp.net**). Portaali abil serdi lisamine oma etapiviisilise pilveteenuses. Seejärel lisage serdi teavet failide CSDEF ja CSCFG, pakkima rakenduse ja värskendage etapiviisilist juurutamise uue paketi kasutamiseks.

## <a name="next-steps"></a>Järgmised sammud

* [Üldine konfigureerimine oma pilveteenuses](cloud-services-how-to-configure-portal.md).
* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy-portal.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name-portal.md).
* [Halda oma pilveteenuses](cloud-services-how-to-manage-portal.md).
