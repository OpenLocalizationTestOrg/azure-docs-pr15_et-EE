<properties
    pageTitle="Azure'i API halduse KKK | Microsoft Azure'i"
    description="Siit saate teada, vastused levinud küsimustele, mustrite ja Azure API Management parimaid tavasid."
    services="api-management"
    documentationCenter=""
    authors="miaojiang"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="mijiang"/>

# <a name="azure-api-management-faqs"></a>Azure'i API halduse KKK

Saada vastused levinud küsimustele, mustrite ja heade tavade Azure'i API haldus.

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

-   [Kuidas Microsoft Azure'i API haldus meeskond on küsimusi esitada?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)
-   [Mida see tähendab, kui funktsioon eelvaade?](#what-does-it-mean-when-a-feature-is-in-preview)
-   [Kuidas ma secure API Management gateway ja minu tagaandmebaas teenuste vahelise ühenduse?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
-   [Kuidas mu API halduse eksemplari uue eksemplari kopeerida?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
-   [Saate hallata oma API halduse eksemplari programmiliselt?](#can-i-manage-my-api-management-instance-programmatically)
-   [Kuidas lisada kasutaja administraatorite rühma?](#how-do-i-add-a-user-to-the-administrators-group)
-   [Miks on poliitika, mida soovin lisada rühmapoliitika redaktoris saadaval?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
-   [Kuidas kasutada API versioonimise API haldus?](#how-do-i-use-api-versioning-in-api-management)
-   [Kuidas häälestada mitme keskkonnas ühe API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
-   [Saate kasutada SOAP API haldus?](#can-i-use-soap-with-api-management)
-   [On API Management gateway IP aadress konstandi? Saate seda kasutada tulemüüri reeglid?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
-   [Saate konfigureerida OAuth 2.0 loa serveris turvalisuse AD FS-i?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
-   [Milliseid marsruutimise meetodit kasutada API halduse juurutuste mitme geograafiliste asukohtade?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
-   [Kasutada mõni Azure ressursihaldur Mall API halduse teenuse eksemplari loomine?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
-   [Saab kasutada iseallkirjastatud SSL-serdi tagasi end?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
-   [Miks saan tõrke autentimise klooni GIT hoidla katsel?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
-   [Kas API halduse töötab Azure'i ExpressRoute?](#does-api-management-work-with-azure-expressroute)
-   [Teenuse API haldus saate teisaldada ühe tellimuse teise?](#can-i-move-an-api-management-service-from-one-subscription-to-another)


### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Kuidas Microsoft Azure'i API haldus meeskond on küsimusi esitada?

Võtke meiega, kasutades ühte järgmistest suvanditest:

-   Postitada oma küsimusi meie [API halduse MSDN-i Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
-   Saada e-posti <apimgmt@microsoft.com>.
-   Saatke meile funktsiooni taotluse [Azure tagasiside Foorum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Mida see tähendab, kui funktsioon eelvaade?

Kui funktsioon on eelvaade, see tähendab, et saaksime olete aktiivselt tagasisidet selle kohta, kuidas see funktsioon töötab teie eest. Funktsiooni eelvaade on funktsionaalselt lõpule jõudnud, kuid on võimalik teeme serveris, mis on vastuseks klientide tagasiside muutmine. Soovitame, et ei sõltu funktsioon, mis on eelvaates tootmiskeskkonnas. Kui teil on eelvaate funktsioonide kohta tagasisidet, palun andke meile teada kontakti suvandid kaudu [kui ma palun Microsoft Azure'i API haldamine meeskonnatöö küsimus?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Kuidas ma secure API Management gateway ja minu tagaandmebaas teenuste vahelise ühenduse?

Teil on mitu võimalust turvamiseks API Management gateway ja tagaandmebaas teenuste vahelise ühenduse. Sa saad:

-   Kasutage HTTP elementaarset autentimist. Lisateavet leiate teemast [konfigureerimine API sätted](api-management-howto-create-apis.md#configure-api-settings).
- Kasutada SSL-i vastastikune autentimine, nagu on kirjeldatud, [Kuidas tagada tagaandmebaas services kliendi serdi autentimist Azure'i API Management abil](api-management-howto-mutual-certificates.md).
- Kasutage IP valge nimekirja tagaandmebaas teenust. Kui teil on Standard või Premium taseme API halduse eksemplari, lüüsi IP-aadress jääb samaks. Saate määrata oma nimekiri lubama IP-aadress. Azure'i portaalis armatuurlaual saate oma API halduse eksemplari IP-aadress.
- Azure virtuaalse võrguga ühenduse oma API halduse eksemplari. Lisateabe saamiseks vaadake, [Kuidas häälestada VPN-ühendused Azure'i API haldus](api-management-howto-setup-vpn.md).

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Kuidas mu API halduse eksemplari uue eksemplari kopeerida?

Teil on mitu võimalust, kui soovite näiteks API halduse kopeerimine uue eksemplari. Sa saad:

-   Kasutage varundamine ja taastamine API halduse funktsiooni. Lisateabe saamiseks vaadake, [kuidas rakendada avariitaastet abil teenuse varundamine ja taastamine Azure'i API Management](api-management-howto-disaster-recovery-backup-restore.md).
-   Luua oma varundamine ja taastamine funktsiooni abil [API haldamine REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). Salvestamine ja taastamine üksuste soovitud eksemplari REST API abil.
-   Laadige teenuse konfiguratsiooni abil Git ja laadige uue eksemplari. Lisateabe saamiseks vaadake, [Kuidas salvestada ja konfigureerida oma API halduse teenuse konfigureerimine Git abil](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Saate hallata oma API halduse eksemplari programmiliselt?

Jah, saate hallata API halduse programmiliselt abil:

-   [API halduse REST API-ga](https://msdn.microsoft.com/library/azure/dn776326.aspx).
-   [Microsoft Azure'i ApiManagement teenuse teegis SDK](http://aka.ms/apimsdk).
-   [Teenuse juurutus](https://msdn.microsoft.com/library/mt619282.aspx) - ja [Teenusehaldus](https://msdn.microsoft.com/library/mt613507.aspx) PowerShelli cmdlet-käsud.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Kuidas lisada kasutaja administraatorite rühma?

Siin on, kuidas lisada kasutaja administraatorite rühma.

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Minge ressursirühm, mis sisaldab API halduse eksemplar, mida soovite värskendada.
3. API Management, määrake kasutajale **Api halduse kaasautori** roll.

Nüüd saate äsja lisatud kaasautor Azure PowerShelli [cmdlet-käsud](https://msdn.microsoft.com/library/mt613507.aspx). Siit saate teada, kuidas administraatorina sisse logida:

1. Kasutage funktsiooni `Login-AzureRmAccount` cmdlet sisse logida.
2. Konteksti määramine tellimus, mis sisaldab teenuse abil `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Ühekordse sisselogimise URL-i hankimine, kasutades `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Juurdepääs halduskeskuse URL-i abil.


### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Miks on poliitika, mida soovin lisada rühmapoliitika redaktoris saadaval?

Kui poliitika, mille soovite lisada tuhmid või taustaga poliitika redaktoris, olla kindel, et teil on õige ulatuse poliitika. Iga poliitika lause on mõeldud kasutamiseks teatud otsinguulatuste ja poliitika jaotised. Poliitika jaotised ja otsinguulatuste poliitika läbi jaotisest poliitika kasutus [API teabehalduspoliitikate](https://msdn.microsoft.com/library/azure/dn894080.aspx).


### <a name="how-do-i-use-api-versioning-in-api-management"></a>Kuidas kasutada API versioonimise API haldus?

Teil on mitu võimalust kasutada API versioonimise API haldus:

-   API haldus, saate konfigureerida API-de tähistada erinevaid versioone. Näiteks võib teil on kaks erinevat API-d, MyAPIv1 ja MyAPIv2. Arendaja saate valida arendaja soovib kasutada versiooni.
-   Samuti saate konfigureerida oma API teenuse URL, mis ei sisalda versioon lõigu, näiteks https://my.api. Seejärel konfigureerida versioon lõigu iga toimingu [URL-i ümberkirjutamine](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) malli. Näiteks saate määrata toimingu [malli URL-i](api-management-howto-add-operations.md#url-template) ehk /resource ja [URL-i ümberkirjutamine](api-management-howto-add-operations.md#rewrite-url-template) malli nimega /v1/Resource. Saate muuta versioon lõigu väärtus iga toimingu jaoks eraldi.
-   Kui soovite hoida "vaikimisi" versiooni lõigu selle API teenuse URL-i valitud toimingute, seadnud poliitika, mis kasutab poliitika [määramine kirjutamata teenuse](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) tagaandmebaas taotluse tee.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Kuidas häälestada mitme keskkonnas ühe API?

Luua mitme keskkonnas, näiteks testimiskeskkonnas ja tootmiskeskkonna, ühe API, on teil kaks võimalust. Sa saad:

-   Host erinevate API samal rentnikukontol.
-   Majutada samas API-de kohta erinevate rentnikega.

### <a name="can-i-use-soap-with-api-management"></a>Saate kasutada SOAP API haldus?

[SOAP läbiv](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) tugi on nüüd saadaval. Administraatorid saavad importida oma SOAP teenuse WSDL ja Azure API Management loob SOAP esiosa. Arendaja portaali dokumentatsioon, testi konsooli, poliitika ja analytics on kättesaadavad SOAP teenuste jaoks.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>On API Management gateway IP aadress konstandi? Saate seda kasutada tulemüüri reeglid?

Standard- ja Premium astme, avaliku IP-aadress (VIP) rentniku API haldus on staatiline eluiga rentniku, mõned erandid. IP-aadress muutub sellisel:

-   Teenus on kustutatud ja seejärel uuesti luua.
-   Teenuse tellimus on peatatud (nt Sorgen) ja seejärel taastada.
-   Saate lisada või eemaldada Azure virtuaalse võrgu (saate kasutada virtuaalse võrgu ainult Premium taseme).

Mitme piirkonnaga juurutuste korral muutub piirkondliku aadressi kui piirkonnas on siis ja seejärel taastada (saate kasutada mitme piirkonna juurutamise ainult Premium taseme).

Premium taseme rentnikud konfigureeritud mitme piirkonna juurutamiseks on määratud üks avaliku IP-aadress müüginäitajaid piirkonna kohta.

Azure'i portaalis lehel rentniku saate avada oma IP-aadress (või aadressid, mitme piirkonna juurutamine).

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Saate konfigureerida OAuth 2.0 loa serveris turvalisuse AD FS-i?

Saate teada, kuidas konfigureerida OAuth 2.0 loa serveris turvalisuse Active Directory Federation Services (AD FS), lugege teemat [Abil ADFS-i API haldamine](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Milliseid marsruutimise meetodit kasutada API halduse juurutuste mitme geograafiliste asukohtade?

API halduse kasutab juurutuste mitme geograafiliste asukohtade [jõudluse liikluse marsruutimise meetod](../traffic-manager/traffic-manager-routing-methods.md#performance-traffic-routing-method) . Sissetulev liiklus suunatakse lähima API lüüsi. Kui ühe piirkonna läheb ühenduseta, suunatakse liiklust automaatselt järgmisele lähima lüüsi. Lugege lisateavet marsruutimise meetodite [liikluse haldur marsruutimine meetodite](../traffic-manager/traffic-manager-routing-methods.md)kohta.

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kasutada mõni Azure ressursihaldur Mall API halduse teenuse eksemplari loomine?

Jah. Leiate [Azure'i API halduse teenuse](http://aka.ms/apimtemplate) Kiirjuhend mallide.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Saab kasutada iseallkirjastatud SSL-serdi tagasi end?

Jah. Niimoodi kasutamiseks tagasi end iseallkirjastatud serdi turvasoklite kiht (SSL).

1. API halduse abil luua [Taustväärtus](https://msdn.microsoft.com/library/azure/dn935030.aspx) üksus.
2. **SkipCertificateChainValidation** atribuudi väärtuseks **true**.
3. Kui te ei soovi enam lubama iseallkirjastatud serdid, kirjutamata üksuse kustutada või **skipCertificateChainValidation** atribuudi väärtuseks **väär**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Miks saan tõrke autentimise klooni Git hoidla katsel?

Kui kasutate Git Mandaadihaldur või kui proovite klooni Git hoidla Visual Studio abil, võivad tekkida dialoogiboksi Windowsi identimisteabe teadaolev probleem. Dialoogiboksi piirab 127 märki parooli pikkus ja see kärbitakse Microsofti loodud parool. Me tegeleme lühendamine parool. Nüüd, kasutage Git Bash klooni oma Git hoidla.

### <a name="does-api-management-work-with-azure-expressroute"></a>Kas API halduse töötab Azure'i ExpressRoute?

Jah. Azure'i ExpressRoute töötab API haldus.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Teenuse API haldus saate teisaldada ühe tellimuse teise?

Jah. Lisateavet teemast [teisaldamine ressursid uue ressursirühma või tellimusele](../resource-group-move-resources.md).
