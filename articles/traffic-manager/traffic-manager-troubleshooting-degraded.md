<properties
    pageTitle="Tõrkeotsing: halvenenud olek Azure'i liikluse haldur"
    description="Tõrkeotsing liikluse haldur profiilid, kui see on kuvatud halvenenud olek."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Tõrkeotsing: halvenenud olekus Azure'i liikluse haldur

Selles artiklis kirjeldatakse tõrkeotsing Azure'i liikluse haldur profiili, mis on kuvatud halvenenud olek. Selle stsenaariumi korral on soovitatav, et olete konfigureerinud liikluse haldur profiili osutab osa oma cloudapp.net majutatud teenused. Kui märgite ruudu liikluse teie haldur seisundi, näete, et olek on halvenenud.

![halvenenud olekus](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Kui te lähete selle profiili vahekaarti lõpp-punktid, kuvatakse üks või mitu ühenduseta olek lõpp-punktid.

![ühenduseta](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Mõistmine liikluse haldur sondid

- Liikluse haldur peab olema ONLINE ainult siis, kui selle juures saab vastuse HTTP 200 tagasi juures tee lõpp. Muud-200 vastuse on tõrge.
- 30 x suunamine nurjub, isegi siis, kui ümber URL tagastab 200.
- Jaoks HTTPs sondid, serdi tõrkeid ignoreeritakse.
- Dokumendi sisu juures tee pole oluline, kui 200 tagastatakse. Mõned staatiliseks sisuks nagu URL-i katsetamine "/ favicon.ico" on levinud meetod. Dünaamiline sisu, nt ASP-lehed, võib alati tagastada 200, isegi siis, kui rakendus on terve.
- Hea tava on seatud juures tee midagi, mis on piisavalt loogika määratlemiseks, et sait on üles või alla. Eelmises näites, määrates tee "/ favicon.ico", kuvatakse ainult selle w3wp.exe testimine ei reageeri. Selle juures võib viidata, et oma veebirakenduse on terve. Parem variant oleks seatud tee midagi nagu "/ Probe.aspx" mis on saidi seisundi määratlemiseks loogika. Näiteks võite kasutada jõudluse hinnale, et CPU kasutuse või nurjunud taotluste arv. Või võib proovite andmebaasi ressursid või veenduge, et veebirakenduse töötab seansi olek.
- Kui kõik lõpp-punktid profiili on halvenenud, siis liikluse haldur kohtleb kõik nimega terve lõpp-punktid ja marsruudib liikluse kõik lõpp-punktid. Selline käitumine tagab, et probleemide katsetamine abil tulemuseks täielik katkestuste oma teenuse.

## <a name="troubleshooting"></a>Tõrkeotsing

Tõrkeotsing juures tõrge, peate tööriista, mis näitab HTTP olekukoodi saatja juures URL-i kaudu. On palju tööriistu saadaval, mis näitab teile töötlemata HTTP vastus.

* [Viiuldaja](http://www.telerik.com/fiddler)
* [Curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Samuti saate kasutada võrgu vahekaarti F12 silumine tööriistade Internet Exploreris HTTP vastuste vaatamiseks.

Selles näites tahame vastuse meie juures URL-i jaoks: http://watestsdp2008r2.cloudapp.net:80/jaotuse. PowerShelli järgmine näide illustreerib probleemi.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Näide väljund:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Pange tähele, et me ei saanud suunata vastust. Nagu eelnevalt, mis tahes StatusCode kui käsitletakse 200 tõrke. Liikluse haldur lõpp-punkti olek muutub ühenduseta. Probleemi lahendamiseks kontrollige veebisaidi konfiguratsiooni tagamaks, et proper StatusCode tagastatud juures tee. Taaskonfigureerimine tee, mis tagastab 200 osutamiseks liikluse haldur juures.

Kui teie juures kasutab HTTPS-protokolli, peate keelata serdikontrolli SSL/TLS tõrgete vältimiseks oma test. PowerShelli järgmistest serdi valideerimine PowerShelli seansi keelamiseks tehke järgmist.

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Järgmised sammud

[Liikluse haldur liikluse marsruutimise meetodite kohta](traffic-manager-routing-methods.md)

[Mis on liikluse haldur](traffic-manager-overview.md)

[Pilveteenused](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure'i Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Toimingute kohta liikluse Manager (REST API teatmematerjalid)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure'i liikluse Manager cmdlet-käsud][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
