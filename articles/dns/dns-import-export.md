<properties
   pageTitle="Importimine ja eksportimine Azure'i DNS-i abil CLI domeeni tsooni faili | Microsoft Azure'i"
   description="Siit saate teada, kuidas importida ja eksportida Azure'i DNS-i DNS-i tsooni faili, kasutades Azure CLI"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importimine ja eksportimine DNS-i tsooni faili Azure'i CLI abil


Selles artiklis juhendab teid failide importimiseks ja eksportimiseks DNS-i tsooni kaudu Azure'i CLI Azure'i DNS-i kohta.

## <a name="introduction-to-dns-zone-migration"></a>DNS-i tsooni migreerimise tutvustus

DNS-i tsooni faili on teksti faili, mis sisaldab iga süsteemi (DNS) kirje tsooni üksikasjad. See järgib standardne vorming, mistõttu sobib suunamine DNS-i kirjeid DNS-i süsteemi vahel. Tsooni faili abil on lihtne, usaldusväärseid ja mugav viis kanda DNS-i tsooni või sealt välja Azure'i DNS-i.

Azure'i DNS-i toetab importimise ja eksportimise tsoonifailide Azure käsurea liides (CLI) abil. Azure'i CLI on mitu platvormi käsurea tööriista haldamise Azure'i teenuseid kasutada. See on saadaval Windowsi, Maci ja Linuxi platvormid [Azure allalaadimiste lehe](https://azure.microsoft.com/downloads/)kaudu. Mitu platvormi tugi on eriti oluline importimise ja eksportimise tsoonifailide, kuna kõige levinum nimi serveritarkvara [siduda](https://www.isc.org/downloads/bind/), tavaliselt jookseb Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Hankige oma DNS-i tsooni olemasolev fail

Enne DNS-i tsooni faili importimine Azure'i DNS-i, peate hankima tsooni faili koopia. Kui DNS-i tsooni praegu majutatakse sõltub selle faili allikat.

- Kui teie DNS-i tsooni hostib partneri teenuse (nt domeeniregistraatori, sihtotstarbeline DNS-hostiteenuse pakkuja või alternatiivne pilveteenuse pakkuja), selle teenuse peaks andma võimalus DNS-i tsooni faili alla laadida.

- Kui teie DNS-i tsooni on majutatud Windows DNS-i, on zone failide vaikekausta **%systemroot%\system32\dns**. DNS-i teenuse halduskonsooli vahekaardil **Üldine** näitab ka iga tsooni faili täielik tee.

- Kui teie DNS-i tsooni majutab abil siduda, iga tsooni tsooni faili asukoht on määratud siduda konfiguratsiooni faili **named.conf**.

**GoDaddy zone failidega töötamine**<BR>
Tsooni faile alla laadida GoDaddy on veidi mittestandardsed vorming. Peate probleemi lahendamiseks enne nende zone failide importimiseks Azure'i DNS-i. Täielikult kvalifitseeritud veerunimede DNS-i nimed RData iga DNS-i kirje on määratud, kuid neil pole soovitud lõpetamise "." See tähendab, et neid tõlgendab muude DNS-i süsteemide suhteline nimedega. Peate redigeerimiseks tsooni faili lisamiseks soovitud lõpetamise "." oma nimed, enne kui impordite need Azure'i DNS-i.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Azure'i DNS-i DNS-i tsooni faili importimine


Tsooni faili importimisel loob uue zone Azure'i DNS-i kui üks juba olemas. Kui tsooni juba olemas, peate olemasoleva kirje komplekti kirje komplekti tsooni faili ühendada.

### <a name="merge-behavior"></a>Ühenda käitumine

- Vaikimisi ühendatakse olemasolevate kui ka uute kirjete komplektid. Tühistage dubleeritakse on identne kirjeid ühendatud kirjekomplekti sees.

- Teise võimalusena, määrates selle `--force` suvand impordi protsessi asendab olemasoleva kirje komplekti uue kirje komplektid. Olemasoleva kirje komplektides, mida ei saa määrata imporditud tsoonifaili vastav kirje ei saa eemaldada.

- Kui kirje komplektid on ühendatud, kasutatakse live (TTL) olemasoleva kirje seatakse aega. Kui `--force` on kasutanud, kasutatakse uue kirjekomplekti TTL.

- Alusta sertimiskeskuse (SOA) parameetrite (välja arvatud `host`) on alati võetud imporditud tsooni faili, olenemata sellest, kas `--force` kasutatakse. Nime serveri kirje tsooni tipus seadmine alati võetakse TTL samamoodi imporditud tsooni faili.

- Imporditud CNAME-kirje ei asenda olemasolevaid CNAME-kirje sama nimega kui selle `--force` parameeter on määratud.

- Konflikti tekkimisel CNAME-kirje ja muu kirjega sama nime, kuid muud tüüpi (sõltumata sellest, mis on olemasolevad või uued) vahel, säilitatakse olemasoleva kirje. See ei sõltu kasutamine `--force`.

### <a name="additional-information-about-importing"></a>Lisateave importimise kohta

Järgmises märkmed leiate täiendavad tehnilised üksikasjad tsooni importimise protsess.

- Funktsiooni `$TTL` direktiivi on valikuline, ja see on toetatud. Kui `$TTL` direktiivi antakse, kirjed kuvatakse konkreetsed TTL on imporditud Sea vaike-TTL 3600 sekundit. Kui kahe kirje sama kirjekomplekti määrata eri paketid, kasutatakse väiksem väärtus.

- Funktsiooni `$ORIGIN` direktiivi on valikuline, ja see on toetatud. Kui `$ORIGIN` on määratud, kasutatakse vaikeväärtus on määratud käsureal tsooni nimeks (pluss soovitud lõpetamise ".").

- Funktsiooni `$INCLUDE` ja `$GENERATE` suuniseid ei toetata.

- Toetatakse järgmisi kirjetüübid: A-, AAAA, CNAME, MX, NS, SOA, SRV ja TXT.

- SOA kirje on loodud automaatselt Azure'i DNS-i tsooni loomisel. Tsooni faili importimisel SOA kõik parameetrid on võetud selle tsooni faili *peale* selle `host` parameeter. Seda parameetrit kasutab esitatud Azure DNS-i väärtust. See on esitatud Azure DNS-i esmane Nimeserver peavad viitama see parameeter.

- Nime serveri kirjekomplekti tsooni tipus ka luuakse automaatselt Azure'i DNS-i tsooni loomisel. Ainult TTL selle kirjekomplekti imporditakse. Need kirjed sisaldavad esitatud Azure DNS-i nimi serverite nimed. Andmete salvestamiseks kirjutatakse ei imporditud tsoonifail olevaid väärtusi.

- Avaliku eelvaate, Azure'i DNS-i toetab ainult ühe-string TXT-kirjed. TXT-kirjed multistring liitsõnumeid ja kärbitakse 255 märki.

### <a name="cli-format-and-values"></a>CLI vorming ja väärtused


Azure'i CLI käsk importimiseks DNS-i tsooni vorming on:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Väärtused:

- `<resource group>`on Azure DNS-tsooni ressursirühma nimi.
- `<zone name>`on tsooni nimi.
- `<zone file name>`on tsooni imporditava faili tee ja nimi.

Kui ressursirühma pole tsooni selle nimega, luuakse see teie eest. Kui tsooni juba olemas, ühendatakse olemasoleva kirje komplekti imporditud kirje komplektid. Olemasoleva kirje komplekti kirjutada, kasutage funktsiooni `--force` suvand.

Tsooni faili vormingu kontrollimine tegelikult importimata, kasutage funktsiooni `--parse-only` suvand.

### <a name="step-1-import-a-zone-file"></a>Samm 1. Tsooni faili importimine

Tsooni **contoso.com**zone faili importida.

1. Logige sisse Azure tellimuse Azure'i CLI abil.

        azure login

2. Valige tellimus, kuhu soovite luua oma uue DNS-i tsooni.

        azure account set <subscription name>

3. Azure'i DNS-i on Azure ressursihaldur ainult teenust, nii, et Azure'i CLI üle minna ressursihaldur režiim.

        azure config mode arm

4. Azure'i DNS-i teenuse kasutamiseks peate registreerima tellimuse Microsoft.Network ressursi pakkuja kasutamiseks. (See on ühekordne toiming iga tellimuse jaoks.)

        azure provider register Microsoft.Network

5. Kui teil ei ole veel üks, peate ka ressursihaldur ressursi rühma loomine.

        azure group create myresourcegroup westeurope

6. Faili **contoso.com.txt** tsooni **contoso.com** importimine uue DNS-i tsooni sisse ressursi rühma **myresourcegroup**käivitage käsk `azure network dns zone import`.<BR>See käsk on tsooni faili laadimine ja sõeluda seda. Käsk käivitatakse sarja käskude teenuses Azure DNS-i tsooni loomiseks ja kogu kirjet seab tsooni. Käsu ka aruande edenemise konsooli aknas kõik tõrked või hoiatused koos. Kuna komplektid kirje on loodud sarja, võib kuluda mõni minut suur ala-faili importimise kohta.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Samm 2. Veenduge, et tsooni

Kinnitamiseks DNS-tsooni pärast importimist faili, saate teha ühte järgmistest.

- Azure'i CLI järgmise käsu abil saate loetleda kirjeid.

        azure network dns record-set list myresourcegroup contoso.com

- Saate loetleda kirjete PowerShelli cmdleti abil `Get-AzureRmDnsRecordSet`.

- Saate kasutada `nslookup` kinnitamiseks nimelahendus kirjete jaoks. Kuna tsooni pole veel delegeeritud, peate õige Azure'i DNS-i nimeserverite selgesõnaliselt. Allpool valimi näitab, kuidas tuua tsooni määratud nimi serverite nimed. SEDA ka näitab, kuidas päringu abil kirje "www" `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Samm 3. Värskendage DNS-i delegeerimine

Kui teil on kinnitatud tsooni õigesti imporditud, peate DNS-i delegeerimine nimeserverite Azure'i DNS-i osutamiseks värskendada. Lisateavet leiate artiklist [värskendada DNS-i delegeerimine](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Azure'i DNS-i DNS-i tsooni faili eksportimine

Azure'i CLI käsk importimiseks DNS-i tsooni vorming on:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Väärtused:

- `<resource group>`on Azure DNS-tsooni ressursirühma nimi.
- `<zone name>`on tsooni nimi.
- `<zone file name>`on tsooni eksporditava faili tee ja nimi.

Nimega zone importida, kus esmalt tuleb sisse, valite tellimuse ja konfigureerida Azure CLI ressursihaldur režiimi kasutada.

### <a name="to-export-a-zone-file"></a>Tsooni faili eksportimine


1. Logige sisse Azure tellimuse Azure'i CLI abil.

        azure login

2. Valige tellimus, kuhu soovite luua oma uue DNS-i tsooni.

        azure account set <subscription name>

3. Azure'i DNS-i on ainult ressursihaldur Azure'i teenus. Azure'i CLI peab vahetada ressursihaldur režiim.

        azure config mode arm

4. Funktsiooni olemasoleva Azure'i DNS-i tsooni **contoso.com** ressursi rühma **myresourcegroup** eksportimine faili **contoso.com.txt** (praeguses kaustas), käivitage `azure network dns zone export`. See käsk on kõne Azure'i DNS-i teenuse loetlemine kirje komplekti tsooni ja tulemusi siduda ühilduv zone faili eksportida.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

