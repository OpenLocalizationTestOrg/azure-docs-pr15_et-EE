<properties
    pageTitle="Kasutusanalüüsi platvormid: Apache Storm võrdlus voo Analytics | Microsoft Azure'i"
    description="Juhised, valides pilve analytics platvormi Apache Storm võrreldes voo Analytics abil. Funktsioonid ja erinevuste mõistmine."
    keywords="Kasutusanalüüsi platvorm, analytics platvormid, pilve analytics platvormi, torm võrdlus"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Aitab valimise voogesituse analytics platvorm: Azure'i voo Analytics Apache Storm võrdlus

Juhised valimise pilve analytics platvormi see Apache Storm võrdlus Azure'i voo Analytics abil. Mõista väärtust ettepanekud voo Analyticsi võrreldes Apache Storm hallatav teenus Windows Azure Hdinsightiga, et saaksite valida oma ettevõtte jaoks õige lahenduse kohta kasutada juhtudel.

Nii analytics platvormid pakuvad eelised PaaS lahenduse, on mõned põhi eristada võimalusi, et eristada neid. Võimaluste ka nende teenuste piirangud on loetletud allpool aidata teil maa teie eesmärkidele lahendus.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Torm voo Analytics võrdlus: üldised funktsioonid ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure'i voo Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm Hdinsightiga kohta</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Avage allikas</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ei, Azure'i voo Analytics on Microsoft mittekaubanduslik pakub.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jah, Apache Storm on Apache litsentsitud tehnoloogia.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsofti toetatud</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Jah </p>
            </td>
            <td width="246" valign="top">
                <p>
Jah </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Riistvaranõuded</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Puuduvad riistvara nõuded. Azure'i voo Analytics on Azure'i teenus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Puuduvad riistvara nõuded. Apache Storm on Azure'i teenus.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ülemise taseme üksus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Azure'i voo Analytics klientidega juurutada ja jälgida streaming töö.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache torm Hdinsightiga klientidele juurutada ja jälgida kogu kobar, mida saab mitme Storm töö kui ka muude teenustest (sh paketi).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hind</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Voo Analytics on hinnaga, töödeldud andmete maht ja streaming ühikute (tunnis töö töötab) nõutav.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Hinnad täiendava teabe leiate siit.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Apache Storm Hdinsightiga sisse, ühiku ostmise on kobar-põhine ja ei lisandu klaster töötab sõltumatu töökohtade juurutatud aja.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Hinnad täiendava teabe leiate siit.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Iga Analyticsi platvormi koostamine##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure'i voo Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm Hdinsightiga kohta</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Võimalused: SQL-i DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Jah, on lihtne kasutada SQL-i keelte tugi on saadaval.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ei, kasutajate koodi kirjutamine ja Java C# või kasutada Trident API-d.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Võimalused: Ajaline tehtemärgid</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Välja kasti toetatud akendega ja agregaadid ja ajaline ühendused.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mida kasutaja peab ajaline tehtemärke.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Arendamise kogemus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktiivsete loome ja silumine kogemusi Azure portaali kaudu näidisandmeid.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Areng silumine ja jälgimise kogemus on antud kaudu Visual Studio .net-i kasutajate jaoks muude keelte arendajad ja Java kogemus võib kasutada nende valitud IDE.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Silumine tugi</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Voo Analytics pakub lihtsa töö olek ja toimingud logid viis silumine, kuid praegu ei paku paindlikkust mida ja kui palju on kaasatud logid st: Paljusõnaline režiim.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Üksikasjalik logid on saadaval silumine eesmärgil. On kaks võimalust Surface'i logid kasutajale, visual Studio või kasutaja kaudu saate RDP kobar logid juurdepääsu sisse.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>UDF-i tugi (kasutaja määratletud funktsioonid)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Praegu ei toeta UDF-ID.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Siit leiate ülevaate kirjutatud C#, Java või teie valitud keeles.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Laiendatav - kohandatud koodi</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ei toetata voo Analytics laiendatav kood.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jah, on kohandatud koodi kirjutamiseks C#, Java või muude toetatud keeltes tormi kättesaadavus.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Andmeallikate ja väljundid##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure'i voo Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm Hdinsightiga kohta</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Sisendandmete allikad</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Toetatud Sisestuskeel allikad on Azure sündmuse jaoturi ja Azure plekid.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Puuduvad konnektorid sündmuse jaoturi, teenuse siini, Kafka jne. Mittetoetatavad ühendused võivad rakendada kohandatud koodi kaudu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Sisendandmete vormingud</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Toetatud Sisestuskeel vormingud lähevad Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mis tahes vorming võib rakendada kohandatud koodi kaudu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Väljundid</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Voogesituse töö võib olla mitu väljundit. Toetatud väljundid: Azure'i sündmuse jaoturi, Azure'i bloobimälu, Azure tabelite SQL Azure'i DB ja PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tugiteenuste palju väljundeid topoloogia, iga väljund võib olla kohandatud loogika järgneval töötlemiseks. Välja kasti sisaldab Storm konnektorid PowerBI, Azure'i sündmuse jaoturi, Azure'i bloobimälu poe, Azure'i DocumentDB, SQL-i ja HBase. Mittetoetatavad ühendused võivad rakendada kohandatud koodi kaudu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Andmevormingute kodeering</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Voo Analytics nõuab UTF-8 andmete vorming, mida saab kasutada.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Mis tahes kodeering vormingut rakendada kohandatud koodi kaudu.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Haldus ja toimingud##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure'i voo Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm Hdinsightiga kohta</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Töö juurutamise mudeli</strong>
                </p>
                <p>
                    - <strong>Azure'i portaal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShelli</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Juurutamise on rakendatud Azure portaali, PowerShelli ja REST API-de kaudu.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment on rakendatud Azure portaali, PowerShelli, Visual Studio ja REST API-de kaudu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>(Statistika, hinnale jne).</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Jälgimine on rakendatud Azure portaali ja REST API-de kaudu.
                </p>
                <p>
Kasutaja võib-olla ka Azure teatiste konfigureerimine.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Jälgimine on rakendatud Storm Kasutajaliidese ja REST API-de kaudu.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skaleeritavus.</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Iga töö Streaming üksuste arv. Iga üksuse Streaming töötleb kuni 1 MB/s. Max 50 üksuste vaikimisi. Kõne suurendamiseks limiit.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Sõlmed HDI Storm kobar arv. Pole piiratud arv sõlmed (Azure'i meiliteavitus määratletud ülemise limiit). Kõne suurendamiseks limiit.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Andmete töötlemise piirangud</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Kasutajad saavad skaala üles või alla Streaming ühikute töötlemise suurendamiseks või optimeerida kulusid.
                </p>
                <p>
Kuni 1 GB/s skaala </p>
            </td>
            <td width="246" valign="top">
                <p>
Kasutaja saab skaala üles või alla kobar suurus vajadustele.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Peata/elulookirjeldus</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Lõpetamine ja jätkake viimase kohast on peatatud.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Lõpetamine ja jätkake viimase paigutada peatatud põhjal vesimärk.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Teenuse ja framework värskendamine</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automaatne paikamine pole tööseisakute abil.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automaatne paikamine pole tööseisakute abil.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Süsteemi järjepidevuse tugevalt saadaval klienditeenindusele koos tagatud SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA 99,9% sees </p>
                <p>
Automaattaaste tõrke </p>
                <p>
Stateful ajaline tehtemärke on valmis.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA Storm klaster 99,9% sees. Apache Storm on viga salliv streaming platvorm, aga on klientide eest streaming töö käivitamine katkestusteta.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Täpsemad funktsioonid##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure'i voo Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm Hdinsightiga kohta</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hilinenud saabumise ja vales järjestuses sündmuse töötlemine</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Sisseehitatud konfigureeritav poliitikate ümber korraldada, langev sündmuste või reguleerida sündmuse ajal.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Kasutaja peab käsitlema selle stsenaariumi loogika rakendada.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Lähteandmed</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Viide andmete saadaolevatest Azure plekid max maht on 100 MB-mälu otsing vahemälu. Andmete värskendamine haldab teenus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Andmete maht piirangud. Saadaval HBase, DocumentDB, SQL serveri ja Azure'i konnektoreid. Mittetoetatavad ühendused võivad rakendada kohandatud koodi kaudu.
                </p>
                <p>
Andmete värskendamine peab käsitleja oli kohandatud koodi.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Seadme õ integreerimine</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Konfigureerimisega avaldatud Azure seadme õppe mudelid funktsioonide ASA töö loomine <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(privaatne eelvaade)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Saadaval Storm poldid kaudu.
                </p>
            </td>
        </tr>
    </tbody>
</table>
