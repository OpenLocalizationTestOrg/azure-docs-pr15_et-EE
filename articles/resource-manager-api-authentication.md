<properties 
   pageTitle="Active Directory autentimise ja ressursihaldur | Microsoft Azure'i"
   description="Arendaja juhend autentimise Azure'i ressursihaldur API ja Active Directory integreerimise rakendus muude Azure'i tellimused."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Kuidas kasutada Azure Active Directory ja ressursihaldur haldamiseks kliendi ressursid

## <a name="introduction"></a>Sissejuhatus

Kui olete tarkvaraarendaja, kes peab haldava kliendi Azure ressursse rakenduse loomine, näitab selles teemas kuidas Azure'i ressursihaldur API-de autentimiseks ja muude tellimuste ressursside pääseda. 

Rakenduse pääsete juurde ressursihaldur API mitmel viisil.

1. **Kasutaja + rakenduse access**: rakendused, et juurdepääs ressursid sisselogitud kasutaja nimel. Seda moodust töötab rakendusi nagu veebirakenduste ja käsurea tööriistu, mis käsitlevad ainult "interaktiivse" Azure vahendite haldamine.
1. **Ainult rakenduse access**: rakendused, mis töötavad daemon teenuste ja ajastatud tööd. Rakenduse identiteedi antakse otsest juurdepääsu ressursside. Seda moodust töötab rakendused, millel on vaja Azure pikaajalise "ühenduseta juurdepääsu".

Selles teemas antakse üksikasjalikke juhiseid, et luua rakendus, mis töötab nii viiside autoriseerimine. See näitab, kuidas REST API-ga või C# iga toimingu sooritamiseks. Täieliku ASP.net-i MVC rakendus on saadaval veebisaidil [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Selles teemas tähist töötab web app, mida saate proovida [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense)juures. 

## <a name="what-the-web-app-does"></a>Mida tähendab web app

Veebirakendus:

1. Märgid sisse Azure'i rakendust.
2. Küsib kasutaja, et ressursihaldur web appi juurdepääsu anda.
3. Saab kasutaja + rakenduse juurdepääsu luba juurdepääs ressursihaldur.
4. Luba (alates samm 3) kasutab ressursihaldur helistada ja rakenduse teenuse põhilise tellimus, mis annab rakenduse pikaajalise juurdepääsu tellimuse rolli määramine.
5. Saab ainult rakenduse sümboolne.
6. Ressursside kaudu ressursihaldur tellimuse haldamiseks kasutab luba (alates juhis 5).

Siin on veebirakenduse voo lõpuni.

![Ressursihaldur autentimise kulgemist](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Kasutaja, sisestage tellimus, mida soovite kasutada tellimuse id:

![Sisestage tellimuse id](./media/resource-manager-api-authentication/sample-ux-1.png)

Valige konto, mida soovite kasutada logimine.

![Valige konto](./media/resource-manager-api-authentication/sample-ux-2.png)

Sisestage oma kasutajanimi ja parool.

![mandaat](./media/resource-manager-api-authentication/sample-ux-3.png)

Andke oma Azure'i tellimused rakenduse juurdepääs:
 
![Juurdepääsu andmine](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Ühendatud tellimuste haldamine:

![Tellimuse ühenduse loomine](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Rakenduse registreerimine

Enne alustamist kodeerimise Registreerige oma veebirakenduse koos Azure Active Directory (AD). Rakenduse registreerimine loob keskse identiteedi oma rakenduse Azure AD. Tal põhiteavet rakenduse OAuthi kliendi ID, vastuse URL-id ja identimisteavet, mida teie rakendus kasutab autentimiseks ja Azure ressursihaldur API-de juurde. Rakenduse registreerimine kirjete ka delegeeritud õigusi, rakenduse peab kasutaja nimel Microsoft APIs kasutamisel. 

Kuna teie rakendus kasutab muude tellimuste, tuleb see konfigureerida mitme rentniku rakendus. Sisestage valideerimine läbida seostatud teie Active Directory domeeni. [Klassikaline portaali](https://manage.windowsazure.com)sisselogimine seostatud teie Active Directory domeenid kuvamiseks. Valige oma Active Directory ja seejärel valige **Domeenid**.

Järgmises näites kujutatakse registreerida rakenduse Azure PowerShelli abil. Peab teil olema selle käsu toimimise Azure PowerShelli uusim versioon (August 2016). 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
AD rakendus sisselogimiseks peate rakenduse id ja parooliga. Rakenduse id, mis on tagastatud eelmise käsu abil saate näha:

    $app.ApplicationId

Järgmises näites kujutatakse registreerida rakenduse Azure'i CLI abil. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Tulemused sisaldavad AppId, mida te ei vaja kui taotluse autentimine.

### <a name="optional-configuration---certificate-credential"></a>Valikuline konfiguratsioon - serdi mandaati

Azure AD toetab ka serdi identimisteabe rakenduste: luua iseallkirjastatud cert, hoidke privaatvõti ja avalik võti lisamine rakenduse registreerimise Azure AD. Autentimise rakenduse saadab small last Azure AD sisse loginud, kasutades oma privaatvõti ja Azure AD kinnitatakse signatuur, kasutades registreeritud avalik võti.

Rakenduse AD sertifikaadiga loomise kohta leiate artiklist [PowerShelli kasutamine Azure põhisumma ressursid juurdepääsu teenuse](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) või [Kasutada Azure CLI põhisumma ressursid juurdepääsu teenuse](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Rentniku id toomine tellimuse id

Luba, mida saab kasutada helistamiseks ressursihaldur taotlemiseks rakenduse vaja teada Azure AD rentniku, mis hostib Azure tellimuse rentniku ID-d. Tõenäoliselt kasutajate teate oma tellimuse ID-d, kuid need ei pruugi teada oma rentniku ID Active Directory jaoks. Paluge kasutajal kasutaja rentniku id saamiseks tellimuse ID. Sisestage selle tellimuse id, saates tellimuse kohta.

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Taotluse nurjub, kuna kasutajal on pole veel sisse loginud, kuid saate tuua rentniku id vastus. Tuua seda erandit vastuse päise väärtus id rentniku jaoks **WWW autentida**. Näete seda meetodit [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) rakendamine.

## <a name="get-user--app-access-token"></a>Saada kasutaja + rakenduse juurdepääsu luba

Rakenduse suunab Azure AD on OAuthi 2.0 lubada taotleda – kasutaja mandaat autentimiseks ja naasta tähise autoriseerimine ja kasutajale. Teie rakendus kasutab luba kood kuvatakse juurdepääsu Turbeloa ressursihaldur jaoks. [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) meetod loob luba taotluse.

Selles teemas kirjeldatakse REST API kutsed autentida. Helper teekide abil autentida koodi. Nende teekide kohta leiate lisateavet teemast [Azure Active Directory autentimine teekide](./active-directory/active-directory-authentication-libraries.md). Identiteedi haldamine rakenduses integreerimise kohta vaadake teemast [Azure Active Directory arendaja juhend](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth taotluse (OAuth 2.0)

Probleemi on avatud ID ühendamine/OAuth2.0 lubada taotleda Azure AD Autoriseerin lõpp-punkti:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Päringu päringustringi parameetrite, mis on saadaval selle päringu on kirjeldatud teemas [taotluse autoriseerimine näeb välja umbes järgmine](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Järgmises näites kirjeldatakse, kuidas taotleda OAuth2.0 luba.

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD autendib kasutaja ja vajadusel palub kasutajal anda õiguse rakendus. Luba kood naaseb vasta URL-i rakenduse. Sõltuvalt nõutud response_mode, Azure AD kas andmed saadetakse päringustringi või postituse andmetena.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth taotluse (avatud ID ühendust)

Kui mitte ainult soovivad juurdepääsu Azure ressursihaldur kasutaja nimel, aga ka võimaldab kasutajal rakenduse abil oma Azure AD kontoga sisse logida, välja on avatud ID ühenduse lubada taotleda. Avatud ID ühendust luua, koos rakenduse saab ka mõne id_token Azure AD, mis rakenduse abil saate kasutaja sisselogimine.

Päringu päringustringi parameetrite, mis on saadaval selle päringu on kirjeldatud teemas [Saatmistaotlus Logi sisse](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

Näide avatud ID ühenduse taotlus on:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD autendib kasutaja ja vajadusel palub kasutajal anda õiguse rakendus. Luba kood naaseb vasta URL-i rakenduse. Sõltuvalt nõutud response_mode, Azure AD kas andmed saadetakse päringustringi või postituse andmetena.

Avatud ID ühenduse vastuse näide on järgmine:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Turbeloa taotluse (OAuth2.0 kood anda voog)

Nüüd, kui teie taotlus on saanud luba kood Azure AD, on aega, et saada juurdepääsu Turbeloa Azure'i ressursihaldur.  Postitada mõni OAuth2.0 koodi anda Turbeloa taotlus Azure AD Turbeloa lõpp-punkti: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Päringu päringustringi parameetrite, mis on saadaval selle päringu on kirjeldatud teemas [autoriseerimine kood](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

Järgmises näites on kujutatud koodi anda luba parooli mandaadi taotluse:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Serdi identimisteabe töötades luua JSON Web Turbeloa (JWT) ja märk (RSA SHA256) abil oma rakenduse serdi mandaat privaatvõti. Luba selle nõude tüübid on näidatud [JWT Turbeloa taotluste](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Viite teemast [Active Directory Auth teegis (.NET) kood](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) kliendi kinnituse JWT sõned logima.

Vaadake teemat kliendi autentimine [spec avatud ID ühenduse](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) üksikasju. 

Järgmises näites on kujutatud koodi anda luba serdi mandaati taotluse:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Näide vastuse koodi anda luba jaoks: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Toime koodi anda loa vastuse

Eduka loa vastuse sisaldab (kasutaja + rakenduse) juurdepääsu luba Azure'i ressursihaldur. Teie rakendus kasutab seda juurdepääsu luba juurdepääs ressursihaldur kasutaja nimel. Accessi sõned välja Azure AD eluiga on üks tund. See on tõenäoline, et oma veebirakenduse vajab pikendamiseks (kasutaja + rakenduse) juurdepääsu luba. Kui seda on vaja uuendada juurdepääsu luba, kasutage rakenduse loa vastuse saadetavat värskendamise luba. Postituse Azure AD Turbeloa lõpp-punkti taotleda OAuth2.0 Turbeloa: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Parameetrite kasutamine koos värskendustaotlust saata on kirjeldatud [värskendamine juurdepääsu luba](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Järgmises näites kirjeldatakse, kuidas kasutada värskendamine Turbeloa.

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Kuigi värskendamise sõned saab kasutada saada uut Accessi sõned Azure'i ressursihaldur, nad ei sobi rakenduse ühenduseta juurdepääsu. Värskenda sõned eluiga on piiratud ja Värskenda sõned on seotud kasutaja. Kui kasutaja lahkub, lähevad kaotsi rakenduse abil Värskenda Luba juurdepääs. Seda moodust pole sobiv rakendusi, mis kasutavad meeskondadel hallata oma Azure ressursse.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Märkige ruut, kui kasutaja saab määrata juurdepääsu tellimusele

Nüüd teie taotlus on märgiks juurdepääsu Azure ressursihaldur kasutaja nimel. Järgmiseks on rakenduse ühenduse tellimus. Pärast ühenduse loomist rakenduse saate hallata neid tellimuste isegi siis, kui kasutaja ei esita (pikaajaline ühenduseta juurdepääs). 

Iga tellimuse ühenduse, helistage [ressursihaldur loendiõigused](https://msdn.microsoft.com/library/azure/dn906889.aspx) API määratlemaks, kas kasutajal on juurdepääs haldus õigused tellimuse.

ASP.net-i MVC valimi rakenduse [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) meetodit rakendatakse see kõne.

Järgmises näites on kujutatud kuidas taotleda kasutaja õiguste tellimus. 83cfe939-2402-4581-b761-4f59b0a041e4 on tellimuse ID-d.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Kasutaja õiguste hankimine tellimuse vastuse näide on järgmine:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Õiguste API tagastab mitu õigused. Iga õigus koosneb lubatud toiminguid (toimingud) ja toiminguid (notactions) pole lubatud. Kui toiming on olemas, mis tahes õiguste loendis lubatud toiminguid ei esine notactions loendis selle õiguse kasutajal on lubatud selle toimingu sooritamiseks. **Microsoft.Authorization/RoleAssignments/Write** on toiming, et mis annab juurdepääsu õiguste haldus. Rakenduse peab sõeluda õiguste tulemi selle toimingu string toimingute ja iga õiguste notactions regex vaste otsimiseks.

## <a name="get-app-only-access-token"></a>Juurdepääs ainult rakenduse Turbeloa

Nüüd teate, kui kasutajal määrata juurdepääsu Azure tellimuse. Järgmised toimingud on:

1. Määrake sobiv roll RBAC rakenduse identiteedi tellimust.
2. Kinnitage ülesande Accessi päringute rakenduse luba tellimust või ressursihaldur abil ainult rakenduse Luba juurdepääs.
1. Salvestada ühenduse rakenduste "ühendatud tellimuste" andmestruktuuri - püsib Tellimuse ID-d.

Vaatame lähemalt esimese sammu. Rakenduse identiteedi RBAC sobiv roll määramiseks peate läbi mõtlema

- Rakenduse identiteedi kasutaja Azure Active Directory objekti ID-d
- Teie rakendus nõuab tellimust RBAC rolli identifikaator

Kui rakenduse autendib Azure AD kasutaja, loob see teenus peamine eesmärk rakenduse selle Azure AD. Azure'i võimaldab RBAC rollide määratav teenuse põhisumma otsest juurdepääsu vastavate rakenduste Azure ressursid. See toiming on täpselt Soovime teha. Päringu Azure AD Graph API rakenduse sisse logitud kasutajale põhisumma teenuse identifikaator määratlemiseks kasutaja Azure AD.

Teil on ainult juurdepääsu sümboolse Azure'i ressursihaldur – peate uue juurdepääsu loa helistamiseks Azure AD Graph API. Iga taotlus Azure AD on õigus päringu nii, et ainult rakenduse juurdepääsu sümboolse piisab oma teenuse peamine eesmärk.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Juurdepääs ainult rakenduse Turbeloa Azure AD Graph API

Rakenduse autentimiseks ja Azure AD Graph API sümboolse saada, välja Kliendi mandaati anda OAuth2.0 meilivoo Turbeloa taotluse Azure AD Turbeloa lõpp-(**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/luba**).

ASP.net-i MVC valimi rakenduse [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) meetodit saab ainult rakenduse Accessi Turbeloa Graph API Active Directory Authentication Library .net-i abil.

Päringu päringustringi parameetrite, mis on saadaval selle päringu on kirjeldatud teemas [taotluse Accessi sümboolne](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Näide taotlus kliendi mandaati anda luba: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Kliendi mandaadi näide vastuse anda luba: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Saada kasutaja Azure AD ObjectId väärtuse rakenduse teenuse kasutajaks.

Nüüd kasutada ainult rakenduse juurdepääsu luba päringu [Azure AD Graphi teenuse põhisumma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API määratlemiseks rakenduse teenuse põhisumma kataloogis objekti ID-d.

ASP.net-i MVC valimi rakenduse [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) meetodit rakendatakse see kõne.

Järgmises näites on kujutatud kuidas taotleda mõne rakenduse teenuse kasutajaks. a0448380-C346-4f9f-b897-c18733de9394 on rakenduse kliendi ID-d.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Järgmises näites on kujutatud rakenduse teenuse taotluse vastuseks põhisumma 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure'i RBAC rolli identifikaator hankimine

Sobiv RBAC rolli määramine põhisumma teenust, peate läbi mõtlema Azure'i RBAC rolli ainuidentifikaator.

Õige RBAC rolli rakenduse:

- Kui rakenduse jälgib tellimusest ainult muudatusi tegemata, on vaja ainult lugemisõigused tellimust. **Lugeja** rolli määramine.
- Kui teie rakendus haldab Azure'i tellimus, muutmine, loomine ja kustutamine üksuste nõuab kaasautori õigused.
  - Kindlat tüüpi ressursside haldamise kohta leiate määrata ressursside kohased kaasautor rollid (virtuaalse masina kaasautor, virtuaalse võrgu kaasautor, salvestusruumi konto kaasautor, jne)
  - Hallata mis tahes ressursi tüüp, määrata **kaasautori** roll.

Kasutajad, seega valige vähim nõutav au Rollimäärang rakenduse numbrit.

Helistage [ressursihaldur rolli määratlus API](https://msdn.microsoft.com/library/azure/dn906879.aspx) loendi kõigi Azure'i RBAC rollid ja Otsi seejärel itereerima kursor tulemi soovitud roll määratluse nime järgi otsimine.

ASP.net-i MVC valimi rakenduse [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) meetodit rakendatakse see kõne.

Taotluse järgmises näites on kujutatud hankimine Azure'i RBAC rolli identifikaator. 09cbd307-aa71-4ACA-b346-5f253e6e3ebb on tellimuse ID-d.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Vastus on järgmises vormingus: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Kõne seda API-d pidevalt pole vaja. Kui olete kindlaks tuntud GUID rolli määratlus, saate koostada rolli määratlus id, kui:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Siin on tuntud GUID levinumaid sisseehitatud rollidest.

| Roll | GUID |
| ----- | ------ |
| Lugeja | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Kaasautor | b24988ac-6180-42a0-ab88-20f7382dd24c
| Virtuaalse masina kaasautor | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtuaalse võrgu kaasautor | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Salvestusruumi konto kaasautor | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Veebisaidi kaasautor | de139f84-1756-47ae-9be6-808fbbe84772
| Web leping kaasautor | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL serveriga kaasautor | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL-i DB kaasautor | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Rakenduse RBAC rolli määramine

Olete kõik peate määrama RBAC sobiv roll põhisumma teenust, kasutades funktsiooni [ressursihaldur loomine Rollimäärang](https://msdn.microsoft.com/library/azure/dn906887.aspx) API.

ASP.net-i MVC valimi rakenduse [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) meetodit rakendatakse see kõne.

Näide taotluse, et rakendus RBAC rolli määramine: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

Taotluse kasutatakse järgmised väärtused:

| GUID | Kirjeldus |
| ------ | --------- |
| 09cbd307-aa71-4ACA-b346-5f253e6e3ebb | tellimuse id
| c3097b31-7309-4c59-b4e3-770f8406bad2 | teenuse põhilise taotluse objekti ID-d
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | lugeja rolli ID-d
| 4f87261d-2816-465d-8311-70a27558df4c | uue Rollimäärang jaoks loonud uue guid

Vastus on järgmises vormingus: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Juurdepääs ainult rakenduse Turbeloa Azure'i ressursihaldur

Kinnitage see rakendus on soovitud tellimuse juurde pääseda, testi toimingu abil kuvatakse ainult rakenduse luba tellimust.

Kuvatakse ainult rakenduse juurdepääsu Turbeloa, järgige juhiseid jaotises [juurdepääs ainult rakenduse Turbeloa Azure AD Graph API](#app-azure-ad-graph), teine ressursi parameetri väärtus: 

    https://management.core.windows.net/

ASP.net-i MVC valimi rakenduse [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) meetodit saab Accessi rakenduse ainult Turbeloa Azure'i ressursihaldur Active Directory Authentication Library .net-i abil.

#### <a name="get-applications-permissions-on-subscription"></a>Tellimuse rakenduse õiguste hankimine

Kontrollimaks, et teie taotlus on soovitud Accessi Azure tellimuse, võite ka kutsuda [Ressursihaldur õiguste](https://msdn.microsoft.com/library/azure/dn906889.aspx) API. See lähenemine on sarnane, kuidas saate määrata, kas kasutajal on juurdepääsu juhtimine õiguste tellimuse. Sel ajal, helistage siiski nende õiguste API eelmises etapis vastuvõetud ainult rakenduse juurdepääsu luba.

ASP.net-i MVC valimi rakenduse [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) meetodit rakendatakse see kõne.

## <a name="manage-connected-subscriptions"></a>Ühendatud tellimuste haldamine

Kui RBAC sobiv roll on määratud rakenduse teenuse põhisumma tellimust, rakenduse saab edasi jälgimine/hallata abil ainult rakenduse sõned Azure'i ressursihaldur.

Kui tellimuse omanik eemaldab teie taotlus Rollimäärang klassikaline portaali või käsurea tööriistade abil, pole enam rakenduse tellimuse juurde pääseda. Sel juhul tuleks teatavad kasutajale ühenduse tellimus lahutada sellest väljaspool rakendus ja anda neile "parandamiseks" ühendus. "Paranda" oleks lihtsalt kustutatud ühenduseta Rollimäärang uuesti luua.

Ainult siis, kui te lubatud kasutaja tellimuste ühenduse rakenduse, peate lubama ühenduse katkestama tellimuste liiga kasutaja. Seisukohast on juurdepääs haldus, katkestada tähendab eemaldamine rolli määramine, mis on rakenduse teenuse kasutajaks on tellimust. Soovi korral võite rakenduse tellimuse riik liiga eemaldada. Ainult need kasutajad halduse pääsuõigusega tellimust on võimalik tellimuse katkestada.

ASP.net-i MVC valimi rakenduse [RevokeRoleFromServicePrincipalOnSubscription meetodit](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) rakendatakse see kõne.

See on õige - kasutajad saavad nüüd hõlpsasti ühendust luua ja hallata oma Azure'i tellimused oma rakendusega.

