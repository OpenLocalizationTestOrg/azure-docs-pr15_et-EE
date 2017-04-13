<properties
     pageTitle="Azure'i CDN abil | Microsoft Azure'i"
     description="Selles teemas kirjeldatakse, kuidas lubada selle sisu kohaletoimetamise võrk (CDN) Azure. Õpetuse tutvustatakse uue CDN profiili ja lõpp-punkti loomine."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Azure'i CDN abil  

Selles teemas tutvustatakse uue profiili CDN-ID ja lõpp-punkti luues Azure'i CDN lubamine.

>[AZURE.IMPORTANT] Vaadake lühitutvustust CDN toimimise funktsioonide, samuti [CDN ülevaade](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>CDN uue profiili loomine

CDN profiil on kogumi CDN lõpp-punktid.  Iga profiil sisaldab ühe või mitme CDN lõpp-punktid.  Soovi korral võite kasutada mitut profiili CDN lõpp-punktide Interneti-domeeni, veebirakenduse või mõnda muude kriteeriumide järgi korraldada.

> [AZURE.NOTE] Vaikimisi on piiratud kaheksa CDN profiilid ühe Azure'i tellimus. Iga CDN profiili on piiratud 10 CDN lõpp-punktid.
>
> CDN hinnad on CDN profiili tasemel rakendatud. Kui soovite kasutada Azure CDN hinnakirjad astme kombinatsiooni, peate mitme CDN profiilid.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Uue CDN lõpp-punkti loomine

**Uue CDN lõpp-punkti loomiseks**

1. Liikuge [Azure portaali](https://portal.azure.com)CDN profiili.  Teil võib-olla on kinnitatud selle armatuurlaua eelmises etapis.  Kui te pole, leiate selle nuppu **Sirvi**ja seejärel **CDN profiilid**ja profiili plaanite lisada oma lõpp-punkt.

    CDN profiil tera kuvatakse.

    ![CDN profiil][cdn-profile-settings]

2. Klõpsake nuppu **Lisa lõpp-punkti** .

    ![Lõpp-punkti nupp Lisa][cdn-new-endpoint-button]

    **Lisa lõpp** tera kuvatakse.

    ![Lõpp-punkti blade lisamine][cdn-add-endpoint]

3. Sisestage selle CDN lõpp-punkti **nimi** .  Selle nime kasutatakse juurdepääsu oma domeeni vahemällu talletatud ressursside `<endpointname>.azureedge.net`.

4. Valige rippmenüüst **Origin tüüp** origin tüüp.  Valige konto Azure Storage **salvestusruumi** , **pilveteenus** Azure pilveteenuse teenuse, **Web App** mis tahes muud avalikult juurdepääsetava web serveri origin (Azure'i või mujal majutatud) Azure'i veebirakenduse või **kohandatud origin** .

    ![CDN origin tüüp](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. **Origin hostname** rippmenüü, valige või tippige domeeni origin.  Ripploendi loetleb kõik saadaolevad päritolu juhises 4 määratud tüüp.  Kui teie **Origin tippige**valitud *kohandatud origin* , te tipite Domeen oma kohandatud päritolu.

6. Sisestage tekstiväljale **Origin tee** ressursside soovite vahemälu, või jätke see väli tühjaks luba vahemälu mis tahes ressursside veebisaidil domeeni juhises 5 määratud tee.

7. **Origin host päis**, sisestage soovite saata iga taotlusega CDN host päis või jätke vaikimisi.

    > [AZURE.WARNING] Teatud tüüpi päritolu, näiteks Azure Storage ja veebirakenduste, nõua hosti päise domeeni päritolu vastavaks. Kui teil on origin, mis nõuab hosti päise erineb oma domeeni, tuleks jätta vaikeväärtus.

8. **Protocol (protokoll)** ja **Origin portide**, määrake protokollid ja kasutada oma ressursse teinud pordid.  Valitud peab olema vähemalt üks protocol (HTTP- või HTTPS).
    
    > [AZURE.NOTE] **Origin pordi** mõjutab ainult, mis port lõpp-punkti kasutab teabe toomiseks päritolu.  Lõpp-punkti ise on saadaval ainult Lõpeta kliendid vaikimisi HTTP ja HTTPS pordid (80 ja 443), olenemata **Origin pordi**.  
    >
    > **Azure'i CDN kaudu Akamai** lõpp-punktid ei võimalda täielik TCP port vahemikus päritolu.  Origin pordid, mis ei ole lubatud loendi leiate teemast [Azure CDN Akamai lubatud Origin pordid kaudu](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Sisu HTTPS-i kaudu juurdepääs CDN on järgmised piirangud.
    > 
    > - Kasutage SSL-sert, mida on CDN. Kolmanda osapoole serdid ei toetata.
    > - Kasutage domeeni CDN-ide (`<endpointname>.azureedge.net`) sisule juurdepääsu HTTPS. HTTPS-i tugiteenuste ei ole saadaval kohandatud domeeninime (CNAMEs), kuna selle CDN ei toeta praegu kohandatud serdid.

9. Klõpsake nuppu **Lisa** uus lõpp-punkti loomiseks.

10. Kui lõpp-punkti on loodud, kuvatakse see loendis profiili lõpp-punktid. Loendivaate näitab URL-i abil vahemällu talletatud sisu, samuti origin domeeni.

    ![CDN lõpp-punkti][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Lõpp-punkti kohe on kättesaadavad kasutamiseks, nagu võtab aega registreerimise levitada on CDN kaudu.  <b>Azure'i CDN kaudu Akamai</b> profiilid, tavaliselt on paljundamine ühe minuti jooksul lõpule.  <b>Azure'i CDN Verizon</b> profiilid paljundamine 90 minuti jooksul tavaliselt täielik, kuid mõnel juhul võib võtta ka rohkem aega.
    >    
    > Kasutajad, kes proovida kasutada CDN domeeninime enne lõpp-punkti konfigureerimine on olevatesse hüppab saavad HTTP 404 vastuse koodide.  Kui see on mitu tundi, sest loodud oma lõpp-punkti ja ei kao 404 vastustega, leiate [tõrkeotsingu CDN lõpp-punktid 404 olekud esitus](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Vt ka
- [Vahemällu toimimist taotlused koos päringus juhtimine](cdn-query-string.md)
- [Kuidas CDN sisu vastendamiseks kohandatud domeeni](cdn-map-content-to-custom-domain.md)
- [Azure'i CDN lõpp-punkti eelse laadi varad](cdn-preload-endpoint.md)
- [Likvideerite Azure CDN lõpp](cdn-purge-endpoint.md)
- [CDN lõpp-punktid esitus käsitsi 404 olekud tõrkeotsing](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
