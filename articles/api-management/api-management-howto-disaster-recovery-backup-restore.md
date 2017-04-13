<properties 
    pageTitle="Rakendada avariitaastet teenuse varundamise ja taastamise Azure'i API Management | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada varundamine ja taastamine sooritada avariitaastet Azure'i API haldus." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Rakendada avariitaastet teenuse varundamise ja taastamise Azure'i API haldamine

Valides avaldamine ja hallata oma API Azure'i API halduse kaudu võtate ära palju tõrketaluvust ja taristu võimalusi, et teil oleks muidu kujundada, rakendada ja hallata. Azure'i platvormi leevendab suur osa võimalike tõrgete maksumus murdosa.

Kättesaadavus taastada mõjutavate piirkond, kus teie API juhtimise teenus on majutatud teil peaks olema valmis taastamiseks teises regioonis teenust igal ajal. Sõltuvalt teie kättesaadavus eesmärgid ja taastamise aja eesmärk võite reserveerida varukoopia teenuse ühes või mitmes piirkonnas ja proovige säilitada oma konfiguratsiooni ja sisu sünkroonis aktiivne teenus. Teenuse varundamine ja taastamine pakub vajalikud koosteüksuse rakendamiseks katastroofi taastamise strateegia kavandamine.

Sellest juhendist näitab, kuidas autentida Azure'i ressursihaldur taotlusi, ja varundamine ja taastamine oma API halduse teenuse eksemplari.

>[AZURE.NOTE] Varundus ja taaste eksemplari API halduse teenuse avariitaastet protsessi saab kasutada ka imitatsiooniga API halduse teenuse eksemplari stsenaariumid nagu lavastus.
>
>Pange tähele, et iga varukoopia lõpeb 7 päeva pärast. Kui proovite varukoopia taastamiseks pärast aegumise 7-päevase tähtaja möödumist, taastamine ei õnnestu on `Cannot restore: backup expired` sõnum.

## <a name="authenticating-azure-resource-manager-requests"></a>Azure'i autentiva ressursihaldur taotleb

>[AZURE.IMPORTANT] Varundus ja taaste REST API kasutab Azure ressursihaldur ja on erinevate autentimise süsteem kui REST API-de haldamine oma API halduse üksused. Selle jaotise juhised on kirjeldatud, kuidas autentida Azure'i ressursihaldur taotlused. Lisateabe saamiseks leiate [Azure'i ressursihaldur autentimist taotlusi](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Kõik tööülesanded, mida teha, kasutades Azure ressursihaldur ressursid on kinnitatud Azure Active Directory täitke järgmised juhised.

-   Lisage Azure Active Directory rentniku rakendus.
-   Rakendus, mida lisasite õiguste määramine.
-   Saada luba kutsed Azure'i ressursihaldur autentimiseks.

Esimene samm on Azure Active Directory rakenduse loomiseks. Abil tellimus, mis sisaldab teie API halduse eksemplari [Azure klassikaline portaali](http://manage.windowsazure.com/) sisse logida ja liikuge oma vaikimisi Azure Active Directory **rakenduste** sakki.

>[AZURE.NOTE] Kui Azure Active Directory vaikekataloogi ei ole nähtav, oma konto, pöörduge administraatori Azure tellimuse andmiseks vajalikud õigused teie konto. Asukoha vaikimisi kataloogi kohta leiate teemast [otsimine vaikimisi kataloogi](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Azure Active Directory rakenduse loomine][api-management-add-aad-application]

Klõpsake nuppu **Lisa**, **ettevõttesiseselt areneb rakenduse lisamine**, ja valige **omakliendi rakendus**. Sisestage kirjeldav nimi ja klõpsake järgmise noolt. Sisestage kohatäite URL, näiteks `http://resources` **Ümber suunata URI**, nagu see on nõutav välja, kuid väärtust kasutatakse hiljem. Märkige ruut rakenduse salvestamiseks.

Kui rakendus on salvestatud, klõpsake nuppu **Konfigureeri**, liikuge kerides jaotiseni **muude rakenduste õigused** ja klõpsake käsku **Lisa rakendus**.

![Õiguste lisamine][api-management-aad-permissions-add]

Valige **Windows** **Azure'i teenuse juhtimise API** ja märkige ruut rakenduse lisamiseks.

![Õiguste lisamine][api-management-aad-permissions]

Klõpsake nuppu **Õigused delegeeritud** äsja lisatud **Windows** **Azure'i teenuse juhtimise API** rakenduse kõrval, märkige ruut **Juurdepääsu Azure Teenusehaldus (eelvaade)**ja klõpsake nuppu **Salvesta**.

![Õiguste lisamine][api-management-aad-delegated-permissions]

Enne API-d, et luua varukoopia ja taastada selle kasutada, on vaja leida märgiks. Järgmises näites kasutatakse [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) Nugeti pakett toomiseks luba.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Asendage `{tentand id}`, `{application id}`, ja `{redirect uri}` abil järgmised juhised.

Asendage `{tenant id}` Azure Active Directory taotluse äsja loodud rentniku id-ga. Pääsete ID-d, klõpsates nuppu **Kuva lõpp-punktid**.

![Lõpp-punktid][api-management-aad-default-directory]

![Lõpp-punktid][api-management-endpoint]

Asendage `{application id}` ja `{redirect uri}` **Kliendi Id** ja URL-i kaudu Azure Active Directory rakenduste **konfigureerimine** menüü jaotis **Ümber suunata URI-d** . 

![Ressursid][api-management-aad-resources]

Kui väärtused on määratud, näiteks koodi peaks tagastama märgiks sarnane järgmises näites.

![Turbeloa][api-management-arm-token]

Enne helistamiseks varundus ja taaste järgmistes lõikudes kirjeldatud toimingud, määrake ülejäänud kõne autoriseerimine taotluse päis.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>API halduse teenuse varundamine
Varundamise API juhtimine hooldustaotlus probleemi järgmised HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

kui:

* `subscriptionId`-sisaldavad proovite API halduse teenuse tellimuse id varundada
* `resourceGroupName`-stringi kujul "Api - vaike-{teenuse piirkond}" kui `service-region` tuvastab Azure piirkond, kus API halduse teenust teile üritavad varundus on majutatud, nt`North-Central-US`
* `serviceName`-API halduse teenuse nimi teete varukoopia määratud selle loomise ajal
* `api-version`-asendamine`2014-02-14`

Määrake taotluse kehasse target Azure'i salvestusruumikonto nimi, kiirklahv, bloobimälu container nime, ja varukoopia nimi:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Seadke soovitud `Content-Type` taotluse päis `application/json`.

Varundus on toiming, mis võib kuluda mitu minutit.  Kui taotluse õnnestus ja varundamist käivitati saate soovitud `202 Accepted` vastuse tähis koos mõne `Location` päis.  Veenduge, et saada taotluste URL on `Location` päise välja selgitada selle toimingu olekut. Varundamise ajal jätkab saada '202 aktsepteeritud' olek kood. Vastuse koodi, `200 OK` näitab eduka varukoopia toimingu sooritamist.

**Märkus**:

- **Container** koosolekukutse kehasse **peavad olema**määratud.
* Varundus on pooleli **peaks katse mis tahes haldamise toimingud** nagu SKU versiooni täiendamine või alandada domeeni nime muutmine jne. 
* Pärast selle loomist praegu taastada, **varundus on tagatud ainult 7 päeva** . 
* Kasutusanalüüsi loomiseks kasutatud **kasutusandmete** aruannete **on pole kaasatud** varukoopia. [Azure'i API haldamine REST API][] abil saate tuua perioodiliselt Kasutusanalüüsi aruannete hoidmiseks.
* Frequency, kellega teete teenuse varukoopiate mõjutab teie taastamine punkti eesmärk. Minimeerida soovitame rakendamise korrapäraste varukoopiate plaanimine kui ka läbimiseks nõudmisel varukoopiate pärast oluliste muudatuste tegemist API halduse teenust.
* Teenuse konfigureerimine (nt API-d, poliitikad, arendaja portaali ilme) varundamise ajal tehtud **muudatused** on protsess **võib olla kaasatud varundamine ja seetõttu lähevad kaotsi**.

## <a name="step2"> </a>API halduse teenuse taastamine
Taastamine on API juhtimine teenuse varem loodud varukoopiatest Veenduge järgmises HTTP-päringu:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

kui:

* `subscriptionId`-sisaldavad API halduse teenust taastate varukoopia sisse tellimuse id
* `resourceGroupName`-stringi kujul "Api - vaike-{teenuse piirkond}" kui `service-region` tuvastab Azure piirkond, kus on majutatud API halduse teenust taastate varukoopia sisse, nt`North-Central-US`
* `serviceName`-teenus on taastatud sisse määratud selle loomise ajal API haldamise nimi
* `api-version`-asendamine`2014-02-14`

Määrake taotluse kehaosa varufaili asukoht, st Azure'i salvestusruumikonto nimi, kiirklahv, bloobimälu container nimi ja varukoopia nimi.

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Seadke soovitud `Content-Type` taotluse päis `application/json`.

Taastamine on toiming, mis võib kuluda kuni 30 või enama minuti lõpuleviimiseks.  Kui taotluse õnnestus ja taastada protsess käivitati saate mõne `202 Accepted` vastuse tähis koos mõne `Location` päis.  Veenduge, et saada taotluste URL on `Location` päise välja selgitada selle toimingu olekut. Taastamine ajal saate jätkavad '202 aktsepteeritud' olekukoodi. Vastuse koodi, `200 OK` näitab taastetoimingu edukaks.

>[AZURE.IMPORTANT] **The SKU** teenus on taastatud rakendusse, **peab vastama** SKU taastatakse varundatud teenuse.
>
>Teenuse konfigureerimine (nt API-d, poliitikad, arendaja portaali ilme) taastetoimingu ajal tehtud **muudatused** on pooleli **võib kirjutatakse üle**.

## <a name="next-steps"></a>Järgmised sammud
Vaadake järgmist Microsoft ajaveebide jaoks kahte erinevat juhendavad tutvustused varundus ja taaste protsessi.

-   [Ise Azure API halduse kontod](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Täname teid Gisela selle artikli eest.
-   [Azure'i API haldus: Varundamine ja taastamine konfigureerimine](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Üksikasjalikult Stuart lähenemine ei sobi ametlik juhiseid, kuid see on väga huvitav.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure'i API halduse REST API-ga]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
