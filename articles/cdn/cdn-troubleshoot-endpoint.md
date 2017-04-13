<properties
    pageTitle="Azure'i CDN lõpp-punktid esitus 404 olek tõrkeotsing | Microsoft Azure'i"
    description="Tõrkeotsing: 404 vastuse koodid Azure'i CDN lõpp-punktid."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>CDN lõpp-punktid esitus käsitsi 404 olekud tõrkeotsing

See artikkel aitab teil [CDN lõpp-punktid](cdn-create-new-endpoint.md) 404 vead esitus koos probleemide tõrkeotsing.

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võite pöörduda [Azure'i MSDN-i](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. [Azure'i tugiteabe veebisait](https://azure.microsoft.com/support/options/) ja klõpsake **Toe saamiseks**.

## <a name="symptom"></a>Sümptom

Olete loonud CDN profiili ja lõpp, kuid sisu ei toimi on CDN saadaval.  CDN URL-i kaudu sisule juurde pääseda kasutajatele saadetakse HTTP 404 olek koode. 

## <a name="cause"></a>Põhjus

On mitu võimalikku põhjust, sh:

- Faili origin ei näe soovitud CDN-ID
- Lõpp-punkti on valesti konfigureeritud, põhjustades CDN vaadata vales kohas
- Selle hosti lükake need tagasi host päis on CDN kaudu
- Lõpp-punkti ei ole olnud aega kogu selle CDN levitamine

## <a name="troubleshooting-steps"></a>Tõrkeotsingu juhiste

> [AZURE.IMPORTANT] Pärast on CDN lõpp-punkti loomiseks, seda kohe on kättesaadavad kasutamiseks, nagu võtab aega registreerimiseks kajastuma on CDN kaudu.  <b>Azure'i CDN kaudu Akamai</b> profiilid, lõpetab paljundamine tavaliselt ühe minuti jooksul.  <b>Azure'i CDN Verizon</b> profiilid paljundamine 90 minuti jooksul tavaliselt täielik, kuid mõnel juhul võib võtta ka rohkem aega.  Kui toimingute selles dokumendis ja kostub 404 vastuseid, kaaluge mõne tunni pärast vaadata enne uuesti avades tugi Piletite ootamine.

### <a name="check-the-origin-file"></a>Märkige ruut origin fail

Esmalt meil tuleks kontrollida selle soovime vahemällu salvestatud fail on saadaval meie origin ja avalikult kättesaadav.  Kiireim viis selleks on avada brauseris tolli-era-või inkognito seanss ja liikuge soovitud failile.  Lihtsalt tippige või kleepige URL väljale aadress ja vaadake, kui mis tulemuseks faili ootate.  Selle näite puhul ma olen Azure Storage konto, kättesaadav faili `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Nagu näete, läbib edukalt test.

![Edu!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Kuigi see on kiireim ja lihtsaim viis kinnitamaks, et fail on avalikult kättesaadav, mõned võrgu konfiguratsiooni ettevõttes võib aidata teil illusiooni, et selle faili on avalikult kättesaadavaks, kui see on tegelikult nähtav ainult kasutajatele võrgu (kui see on majutatud Azure).  Kui teil on välises brauseris, kust saate testida, nt mobiilsideseadme kaudu, mis pole ühendatud teie ettevõtte võrku või virtuaalse masina Azure, mida oleks parim.

### <a name="check-the-origin-settings"></a>Märkige ruut origin sätted

Nüüd kus oleme kinnitamist fail on avalikult kättesaadavaks internetis meil peaks kinnitage meie origin sätted.  [Azure portaali](https://portal.azure.com), sirvige profiili CDN-ID ja klõpsake nuppu lõpp-punkti te kasutate tõrkeotsingut.  Klõpsake tulemuseks **lõpp-punkti** labale päritolu.  

![Lõpp-punkti abaluuga origin on esile tõstetud](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Origin** tera kuvatakse. 

![Origin blade](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Origin tüüp ja hostname (hostinimi)

Kontrollige **Origin tüüp** on õige ning veenduge, et **Origin hostname (hostinimi)**.  Minu näiteks `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, on see hostname osa URL-i `cdndocdemo.blob.core.windows.net`.  Nagu pildil näha, see on õige.  Azure Storage, Web App ja pilveteenuses päritolu, väli **Origin hostname** on rippmenüüst, nii, et me ei pea muretsema õigekirja see õigesti.  Kui kasutate kohandatud origin, on *täiesti kriitiline* teie hostname on õigesti kirjutatud!

#### <a name="http-and-https-ports"></a>HTTP ja HTTPS-pordid

Teine asi, et vaadata siin on teie **HTTP** ja **HTTPS-pordid**.  Enamikul juhtudel 80 ja 443 on õiged ja vajate muudatusi.  Juhul, kui origin server on kuulamine porti, mis tuleb oleksid esindatud.  Kui te pole kindel, lihtsalt vaadata origin faili URL-i.  HTTP ja HTTPS-i spetsifikatsioonide määrata pordid 80 ja 443 vaikesätteid. Minu URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, port pole määratud, siis eeldatakse on vaikimisi 443 ja minu sätted on õiged.  

Kuid öelda URL-i origin failiga, saate kontrollida varem on `http://www.contoso.com:8080/file.txt`.  Märkus selle `:8080` hostname lõigu lõppu.  Pordi kasutama brauserit aru `8080` ühenduse juures veebiserver `www.contoso.com`, seega peate Sisestage väljale **HTTP port** 8080.  See on oluline märkida pordi sätteid nende mõjutavad ainult mis port päritolu teabe toomiseks kasutab lõpp-punkti.

> [AZURE.NOTE] **Azure'i CDN kaudu Akamai** lõpp-punktid ei võimalda täielik TCP port vahemikus päritolu.  Origin pordid, mis ei ole lubatud loendi leiate teemast [Azure CDN Akamai lubatud Origin pordid kaudu](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Lõpp-punkti sätete kontrollimine

Tagasi **lõpp-punkti** tera, klõpsake nuppu **Konfigureeri** .

![Lõpp-punkti blade konfigureerimine nupp on esile tõstetud](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Kuvatakse selle lõpp-punkti **konfigureerimine** tera.

![Blade konfigureerimine](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokollid

**Protokollid**, veenduge, et kliendid, mida kasutavad protokolli.  Kliendi poolt kasutatud sama protokolli saab kasutada juurdepääsu päritolu, nii et see on oluline on õigesti konfigureeritud eelmises jaotises origin pordid.  Lõpp-punkti kuulab ainult HTTP ja HTTPS vaikeport (80 ja 443), olenemata origin pordid.

Vaatame tagasi hüpoteetilise näites koos `http://www.contoso.com:8080/file.txt`.  Kui teile meelde, Contoso määratud `8080` nimega oma HTTP port, kuid ka Oletame, nad määratud `44300` nende HTTPS Port.  Kui nende loodud lõpp nimega `contoso`, oleks nende CDN lõpp-punkti hostname `contoso.azureedge.net`.  Taotluse `http://contoso.azureedge.net/file.txt` on HTTP-päring, et lõpp-punkti tuleks kasutada HTTP port 8080 toomiseks päritolu.  Turvaline taotluse https, `https://contoso.azureedge.net/file.txt`, põhjustaks lõpp-punkti kasutada HTTPS Port 44300 kui retriving faili päritolu.

#### <a name="origin-host-header"></a>Origin host päis

**Origin host päis** on saadetud origin iga taotlusega hosti päise väärtus.  Enamikul juhtudel see peaks olema sama mis **Origin hostname** oleme kinnitanud varasemas versioonis.  Sellel väljal on vale väärtus üldiselt ei põhjusta 404 olekud, kuid võib põhjustada muude 4xx olekuid, olenevalt sellest, mis ootab päritolu.

#### <a name="origin-path"></a>Origin tee

Lõpetuseks meil peaks kontrollima meie **Origin tee**.  Vaikimisi on see tühi.  Saab kasutada ainult selle välja, kui soovite soovite kättesaadavaks on CDN origin majutatud ressursid ulatust piirata.  

Näiteks klõpsake minu lõpp-punkti, ma kalendriüksusi kõik ressursid minu salvestusruumi konto, et see oleks kättesaadav, nii saab tühjaks **Origin tee** .  See tähendab, et taotluse `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` minu lõpp-ühenduse, on tulemuseks `cdndocdemo.core.windows.net` mis taotleb `/publicblob/lorem.txt`.  Samuti taotluse `https://cdndocdemo.azureedge.net/donotcache/status.png` , on tulemuseks lõpp-punkti paluda `/donotcache/status.png` päritolu kaudu.

Kuid mida teha, kui ma ei soovi iga tee minu päritolu on CDN kasutamiseks?  Öelda tahtsin jätke selle `publicblob` tee.  Kui minu **Origin tee** väljale sisestada */publicblob* , mis põhjustavad lõpp-punkti lisamiseks */publicblob* enne iga nõude päritolu.  See tähendab, et taotlus `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` võtab nüüd tegelikult taotluse osa URL-i, `/publicblob/lorem.txt`, ja lisada `/publicblob` alguseni. Selle tulemuseks taotluse `/publicblob/publicblob/lorem.txt` päritolu kaudu.  Kui selle tee ei lahendanud tegeliku faili, tagastavad päritolu 404 olek.  Tegelikult oleks õige URL-i toomiseks lorem.txt selles näites `https://cdndocdemo.azureedge.net/lorem.txt`.  Teate, et me ei sisalda */publicblob* tee, kuna see taotluse osa URL-i on `/lorem.txt` ja lisab lõpp-punkti `/publicblob`, mis `/publicblob/lorem.txt` kutse edasi päritolu.
