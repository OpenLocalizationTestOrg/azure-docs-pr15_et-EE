<properties
  pageTitle="Koormusetasakaalustusteenuse kohandatud sondid laadimine ja seisundi oleku jälgimine | Microsoft Azure'i"
  description="Saate teada, kuidas kasutada kohandatud sondid Azure'i laadi koormusetasakaalustusteenuse jälgida eksemplarid laadi koormusetasakaalustusteenuse taha"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Laadi koormusetasakaalustusteenuse sondid mõistmine

Azure'i laadi koormusetasakaalustusteenuse pakub võimalused jälgida Serveri eksemplari seisundi sondid abil. Kui mõne juures ei vasta, laadi koormusetasakaalustusteenuse lõpetab, saates uued ühendused vigane eksemplar. See ei mõjuta olemasolevad ühendused ja uued ühendused saadetakse terve eksemplaris.

Pilvepõhise teenuse rollid (töötaja rollid ja web rollid) kasutage külaline agent juures jälgimiseks. TCP või HTTP kohandatud sondid peab olema konfigureeritud virtuaalmasinates taha laadi koormusetasakaalustusteenuse kasutamisel.

## <a name="understand-probe-count-and-timeout"></a>Juures count ja ajalõpp mõistmine

Juures käitumine sõltub:

- Eduka sondid, mis võimaldavad näiteks tihedus arv nagu.
- Nurjunud sondid, mis põhjustavad eksemplari tihedus arv nagu alla.

SuccessFailCount ajalõpp ja sagedus väärtus kindlaks teha, kas näiteks kinnitatakse töötab või ei tööta. Azure'i portaalis aeg, mille on seatud kaks korda sagedus väärtus.

Juures konfiguratsiooni koormusetasakaalustusega läbivalt kogu lõpp-punkti (ehk teisisõnu öeldes koormusetasakaalustusega kogum) peab olema sama. See tähendab, et te ei tohi olla erinevad juures konfigureerimine iga rolli näiteks või virtuaalse masina kindla lõpp-punkti kombinatsiooni sama majutatud teenuses. Näiteks iga eksemplari peab olema identse kohaliku pordid ja ajalõpud.

>[AZURE.IMPORTANT] Laadi koormusetasakaalustusteenuse juures kasutab IP-aadressi 168.63.129.16. Avaliku IP-aadressi hõlbustab sisemine platvorm ressursid on Too-oma-omanik IP Azure virtuaalne võrgu stsenaarium teatis. Virtuaalne avaliku IP-aadressi 168.63.129.16 kasutatakse kõikides piirkondades ja ei muutu. Soovitame, et lubate mis tahes kohaliku tulemüüri poliitika IP-aadress. See ei saa pidada ohtu turvalisusele Kuna ainult sisemise Azure'i platvormi saate andmeallika sõnumi selle aadressilt. Kui te ei tee, tekib käitu erinevaid stsenaariumid nagu konfigureerimise sama IP address vahemikus 168.63.129.16 ja võttes dubleeritakse IP-aadressid.

## <a name="learn-about-the-types-of-probes"></a>Lisateavet sondid tüübid

### <a name="guest-agent-probe"></a>Külalisena agent juures

Selle juures on saadaval Azure pilveteenustega ainult. Laadi koormusetasakaalustusteenuse kasutab külaline agent virtual seadmes, ja siis kuulab ja vastab ainult siis, kui eksemplari on valmis olekus vastuse HTTP 200 OK (st teises mitte märkida, nagu hõivatud, ringlussevõtu või peatamine).

Lisateabe saamiseks vt [konfigureerida määratluse faili (csdef) teenuse seisund sondid](https://msdn.microsoft.com/library/azure/ee758710.aspx) või [on Interneti-ühendusega laadi koormusetasakaalustusteenuse puhul pilveteenuste loomise alustamiseks](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Milline külaline agent juures märkida näiteks nimega vigane?

Kui külaline agent ei vasta koos HTTP 200 OK, laadi koormusetasakaalustusteenuse märgib eksemplari nimega ei reageeri ja lõpetab liikluse saata selle eksemplari. Laadi koormusetasakaalustusteenuse jätkuvalt ping eksemplari. Kui külaline agent vastab mõne HTTP 200, laadi koormusetasakaalustusteenuse saadab liikluse selle eksemplari uuesti.

Web rolli kasutamisel veebisaidi kood töötab tavaliselt w3wp.exe, kus jälgitakse on Azure struktuuri või Külastajate agent. See tähendab, et tõrkeid w3wp.exe (nt HTTP 500 vastuseid) ei saa esitada külalisena agent ja laadi koormusetasakaalustusteenuse ei võta see eksemplar välja pööre.

### <a name="http-custom-probe"></a>HTTP kohandatud juures

Kohandatud laadi HTTP koormusetasakaalustusteenuse juures alistab vaikimisi külaline agent juures, mis tähendab, et saate luua oma kohandatud loogika määratlemiseks seisundi rolli eksemplari. Laadi koormusetasakaalustusteenuse sondid oma lõpp-punkti iga 15 minutit, vaikimisi. Laadi koormusetasakaalustusteenuse pööre olla, kui see vastab mõne HTTP 200 ajalõpu jooksul (vaikimisi 31 sekundites) loetakse eksemplari.

See võib olla kasulik, kui soovite rakendada oma loogika eksemplari eemaldamiseks laadi koormusetasakaalustusteenuse pööre. Näiteks võib otsustada, näiteks eemaldada, kui see on suurem kui 90% CPU ja tagastab-200 olek. Kui teil on web rollid, mis kasutavad w3wp.exe, järelikult ka saate automaatse jälgimise veebisaidi, kuna tõrkeid veebisaidi koodi naaseb laadi koormusetasakaalustusteenuse juures-200 olek.

>[AZURE.NOTE] HTTP kohandatud juures toetab suhtelised teed ja ainult HTTP-protokolli. HTTPS-i ei toetata.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Milline märkida näiteks nimega vigane HTTP kohandatud juures?

- HTTP rakendus tagastab mõne HTTP vastuse koodi peale 200 (nt 403, 404 või 500). See on positiivne kinnitus, mis rakenduse peaks kasutusest kohe.
- HTTP-server ei vasta aja pärast üldse. Sõltuvalt sellest, mis on määratud ajalõpu väärtust see võib tähendab see mitme juures taotleb Mine vastamata enne selle juures märgitud saab ei tööta (st enne SuccessFailCount sondid saadetud).
- Server sulgeb ühenduse kaudu TCP lähtestamine.

### <a name="tcp-custom-probe"></a>TCP kohandatud juures

TCP sondid alustada ühenduse läbimiseks on kolme Kätlus määratletud Port.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Milline TCP kohandatud juures märkida näiteks nimega vigane?

- TCP server ei vasta aja pärast üldse. Kui soovitud juures on märgitud ei tööta sõltub nurjunud juures taotlusi minna vastamata enne märkimise juures, kui ei tööta konfigureeritud.
- Selle juures saab TCP rolli eksemplarist lähtestada.

Mõne HTTP seisundi juures või TCP juures konfigureerimise kohta leiate lisateavet teemast [Interneti-ühendusega laadi koormusetasakaalustusteenuse rakenduses ressursihaldur PowerShelli kaudu loomise alustamiseks](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Terve eksemplari uuesti sisse laadimise koormusetasakaalustusteenuse pööre lisamine

TCP ja HTTP käsitletakse terve ja märkida rolli eksemplari terve, kui:

- Laadi koormusetasakaalustusteenuse saab positiivne juures, esimest korda VM saapad.
- Arv SuccessFailCount, (eespool kirjeldatud) määratleb eduka sondid märkimiseks rolli eksemplari nimega terve vajalike väärtus. Kui rolli eksemplari on eemaldatud, eduka, järjestikuste sondid arv võrdne või suurem kui SuccessFailCount märkimiseks rolli eksemplari käivitamine.

>[AZURE.NOTE] Kui seisundi rolli eksemplari on kõikuv, laadi koormusetasakaalustusteenuse, mille möödumisel enam lihtsate rolli eksemplari tagasi terve olekus. Seda poliitika kasutaja ja infrastruktuuri kaitsmiseks.

## <a name="use-log-analytics-for-load-balancer"></a>Laadi koormusetasakaalustusteenuse log analytics kasutamiseks

[Laadi koormusetasakaalustusteenuse log analytics](load-balancer-monitor-log.md) abil saate kontrollida juures seisundit ning juures loendus. Logimise saab kasutada Power BI või Azure'i töö ülevaated statistikat laadi koormusetasakaalustusteenuse seisund oleku kohta.
