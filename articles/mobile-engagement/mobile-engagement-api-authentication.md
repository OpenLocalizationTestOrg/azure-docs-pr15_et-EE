<properties 
    pageTitle="Mobiilse kaasamine REST API-de autentimiseks"
    description="Kirjeldab, kuidas autentimiseks Azure Mobile kaasamine REST API-d" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Mobiilse kaasamine REST API-de autentimiseks

## <a name="overview"></a>Ülevaade

Selles dokumendis kirjeldatakse, kuidas saada kehtiv AAD OAuthi Turbeloa autentimiseks Mobile kaasamine REST API-d. 

Eeldatakse, et teil on kehtiv Azure'i tellimus ja olete loonud Mobile kaasamine rakendus, kasutades ühte meie [Arendaja õpetused](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Autentimine

Microsoft Azure Active Directory vastavalt OAuthi luba kasutatakse autentimist. 

Selleks, et autentimine API taotluse, on autoriseerimine päise tuleb lisada iga taotlus, mis on järgmisel kujul:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory sõned aeguvad 1 tund.

On mitu võimalust, et leida märgiks. Kuna selle API-d ei ole üldiselt nõutud pilveteenus, mida soovite kasutada mõnda API võti. Mõne API võti Azure terminoloogia nimetatakse teenuse põhisumma parool. Järgmistes juhistes kirjeldatakse üks võimalus käsitsi häälestada.

### <a name="one-time-setup-using-script"></a>Ühekordse setup (abil skript)

Peaks järgige alltoodud juhiseid, et teha PowerShelli skripti, mis võtab minimaalne aega setup, kuid kasutab kõige lubatud vaikeväärtuste abil häälestamise määramine. Soovi korral saate ka [käsitsi häälestus](mobile-engagement-api-authentication-manual.md) teha seda otse portaalis Azure juhiste ja peenem konfiguratsiooni. 

1. Azure'i PowerShelli uusima versiooni hankimine [siia](http://aka.ms/webpi-azps). Lisateavet allalaadimine juhiseid, näete seda [linki](../powershell-install-configure.md).  

2. Kui Azure PowerShelli on installitud, saate kasutada järgmisi käske tagamaks, et teil on installitud **Azure mooduli** :

    lisamine. Veenduge, et Azure'i PowerShelli moodul on saadaval loendis Saadaolevad moodulid. 
    
        Get-Module –ListAvailable 

    ![Saadaval Azure moodulid][1]
        
    b. Kui te ei leia Azure PowerShelli mooduli ülaltoodud loendis ja seejärel käivitage järgmine:
        
        Import-Module Azure 
        
3. PowerShelli järgmise käsu ning esitada oma kasutajanime ja parooli Azure'i kontosse sisselogimine Azure'i ressursihaldur: 
        
        Login-AzureRmAccount

4. Kui teil on mitu tellimust, siis käivitage järgmine:

    lisamine. Kõigi tellimuste loendi hankimine ja kopeerige SubscriptionId tellimuse, mida soovite kasutada. Veenduge, et see tellimus on sama, mis on lingid mobiilirakenduse, mida te ei kavatse funktsiooni API-de abil suhelda. 

        Get-AzureRmSubscription

    b. Käivitage järgmine käsk pakkudes selle SubscriptionId konfigureerimiseks kasutada tellimus.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skripti teksti kopeerimiseks kohaliku arvuti ja salvestage see PowerShelli cmdlet-käsu (nt `APIAuth.ps1`) ja seda `.\APIAuth.ps1`. 
    
6. Skript küsib teilt sisend **principalName**. Sisestage sobiv nimi, et soovite abil saate luua rakenduse Active Directory (nt APIAuth). 

7. Pärast skripti on lõpule jõudnud, kuvatakse järgmine neli väärtusi, mida on vaja autentimiseks programmiliselt AD seega veenduge, et need kopeerida. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**ja **salajane**.

    Kasutate TenantId nimega `{TENANT_ID}`, nagu ApplicationId `{CLIENT_ID}` ja salajane nimega `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Teie turbepoliitika vaikimisi võib blokeerida teid PowerShelli skriptide. Sel juhul saate konfigureerida ajutiselt teie täitmise poliitika lubamiseks skripti täitmise abil järgmine käsk:

        > Set-ExecutionPolicy RemoteSigned

8. Siin on, kuidas näeks kogumi PS cmdlet-käsud. 

    ![][3]

9. Märkige ruut Azure'i haldusportaal AD uus rakendus on loodud andsite nimetatakse **principalName** jaotises **Kuva rakenduste minu ettevõte kuulub**skripti nimi.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Saamiseks kehtiv luba

1. Kõne API järgmiste parameetritega ja veenduge, et asendada rentniku\_ID, kliendi\_ID ja kliendi\_salajane:

    - **URL-i taotleda** *https://login.microsoftonline.com/ {rentniku\_ID} / oauth2/luba*
    - **HTTP-sisutüüp päise** nimega *rakenduse/x-www-form-urlencoded*
    - **HTTP taotluse keha** *anda\_tüüp = klient\_identimisteabe & client_id = {kliendi\_ID} & client_secret = {kliendi\_salajane} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Taotluse näide on järgmine:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Siin on näide vastus:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    Selles näites sisaldab URL-i kodeerimine postituse parameetrite `resource` väärtus on tegelikult `https://management.core.windows.net/`. Olge ettevaatlik, et ka URL-i kodeerida `{CLIENT_SECRET}` kuna see võib sisaldada erimärke.

    > [AZURE.NOTE] Testimiseks, saate mõne HTTP kliendi tööriista nagu [viiuldaja](http://www.telerik.com/fiddler) või [Chrome'i postiljon laiend](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Nüüd API kõne ajal järgmised autoriseerimine taotluse päis.

        Authorization: Bearer {ACCESS_TOKEN}

    Kui teil tekib 401 olekukoodi, tagastatakse, märkige ruut vastuse keha, see võib öelda luba on aegunud. Sel juhul avada uue loa.

##<a name="using-the-apis"></a>Funktsiooni API-de kasutamine

Nüüd, kui teil on kehtiv luba, olete valmis API kõnede tegemiseks.

1. Iga API taotluse, peate eelmises jaotises kehtiv, järelejäänud luba, mida te saada edasi.

2. Peate mõned parameetrid kutsesse URI, mille abil tuvastatakse rakenduse lisandmoodul. Taotluse URI, näeb välja umbes järgmine

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Saada parameetrid, oma rakenduse nimi ja seejärel klõpsake käsku Armatuurlaua ja te soovite kuvatakse 3 parameetrite Järgmine leht.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** teie ressursirühma nimi saab olema **MobileEngagement** juhul, kui olete loonud uue. 

    ![Mobile kaasamine API URI parameetrid][2]

>[AZURE.NOTE] <br/>
>1. Ignoreeri API Root aadress, nagu see oli eelmise API-de jaoks.<br/>
>2. Kui lõite rakenduse Azure'i klassikaline portaalis siis peate kasutama ressurssi nimi, mis on erinevas ise rakenduse nimi. Kui lõite rakenduse Azure'i portaalis tuleks kasutada rakenduse nimi ise (ei pole vahet teha rakenduse ressursinimi ja rakendused, mis on loodud uue portaali rakenduse nimi).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



