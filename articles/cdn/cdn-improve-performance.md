<properties
    pageTitle="Jõudluse parandamiseks tihendamise Azure'i CDN | Microsoft Azure'i"
    description="Saate teada, kuidas faili kiiruse parandamiseks ja suurendavad lehe laadimise jõudluse Azure'i CDN failide tihendamise teel."
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

# <a name="improve-performance-by-compressing-files"></a>Failide tihendamise jõudluse parandamiseks

Tihendamine on lihtne ja tõhus meetod faili kiiruse parandamiseks ja suurendada lehe laadimise jõudluse failimahu vähendamine enne saatmist serverist. See vähendab ribalaius kulud ning pakub rohkem kasutusvõimalusi kasutajate jaoks.

On kaks võimalust lubada tihendamine.

- Saate lubada tihendamine origin serveris, millisel juhul on CDN on tihendatud failid läbi ning tihendatud failide toomiseks klientidele, et paluda neil.
- Saate lubada tihendamine otse CDN Edge'i serverid, kuvatakse juhul on CDN tihendamine failid ja aitavad lõppkasutajad, isegi juhul, kui need pole tihendatud origin server.

> [AZURE.IMPORTANT] CDN konfiguratsioonimuudatuste võtta aega kajastuma võrgu kaudu.  <b>Azure'i CDN kaudu Akamai</b> profiilid, lõpetab paljundamine tavaliselt ühe-minuti.  <b>Azure'i CDN Verizon</b> profiilid, kuvatakse enamasti muudatused rakendada 90 minuti jooksul.  Kui olete häälestanud tihendamise oma CDN lõpp-punkti esimest korda, peate arvestama ootamine 1-2 tundi olla kindel, et sätted on olevatesse hüppab enne tõrkeotsingu tihendus

## <a name="enabling-compression"></a>Tihendamise lubamine

> [AZURE.NOTE] Standard- ja Premium CDN astme pakuvad tihendamise samu funktsioone, kuid kasutajaliidese erineb.  Standard- ja Premium CDN astme erinevuste kohta leiate lisateavet teemast [Azure CDN ülevaade](cdn-overview.md).

### <a name="standard-tier"></a>Standardse taseme

> [AZURE.NOTE] See jaotis kehtib **Azure'i CDN Standard Verizon** ja **Azure CDN Standard kaudu Akamai** profiil.

1. Klõpsake keelest CDN profiili CDN lõpp-punkti soovite hallata.

    ![CDN profiili blade lõpp-punktid](./media/cdn-file-compression/cdn-endpoints.png)

    CDN lõpp-punkti tera avaneb.

2. Klõpsake nuppu **Konfigureeri** .

    ![CDN profiili blade haldamise nupp](./media/cdn-file-compression/cdn-config-btn.png)

    CDN konfiguratsiooni tera avaneb.

3. Lülitage sisse **tihendamise**.

    ![CDN Tihendussuvandid](./media/cdn-file-compression/cdn-compress-standard.png)

4. Kasutage vaikimisi tüüpi või eemaldada või lisada failitüüpide loendit.
    
    > [AZURE.TIP] Kui võimalik, seda ei ole soovitatav tihendamise rakendamiseks tihendatud vormingud, nt ZIP-, MP3-, MP4-, JPG-, jne.
    
5. Pärast muudatuste tegemist klõpsake nuppu **Salvesta** .

### <a name="premium-tier"></a>Premium taseme

> [AZURE.NOTE] See jaotis kehtib **Azure CDN Premium Verizon** profiilid.

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-file-compression/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Kursoriga **HTTP suur** vahekaarti ja seejärel kursoriga **Vahemälusätete** hüpik.  Klõpsake **tihendamise**.

    Tihendussuvandid kuvatakse.

    ![Faili tihendamine](./media/cdn-file-compression/cdn-compress-files.png)

3. Luba tihendamine, klõpsates raadionuppu **Tihendamise lubatud** .  Sisestage MIME tüübid, mida soovite tihendada loendina komaga eraldatud (ilma tühikuteta) **Failitüüpide** tekstiväljale.
        
    > [AZURE.TIP] Kui võimalik, seda ei ole soovitatav tihendamise rakendamiseks tihendatud vormingud, nt ZIP-, MP3-, MP4-, JPG-, jne. 

4. Pärast muudatuste tegemist, klõpsake nuppu **Värskenda** .


## <a name="compression-rules"></a>Tihendamise reeglid

Järgmised tabelid kirjeldavad Azure'i CDN tihendamise käitumine kõigi stsenaariumide.

> [AZURE.IMPORTANT] Jaoks **Azure'i CDN Verizon** (Standard ja Premium), ainult õigus failid on tihendatud.  Faili peab olema õigus tihendamise:
>
> - Olla suurem kui 128 baiti.
> - Olema väiksem kui 1 MB.
> 
> **Azure'i CDN Akamai kaudu**, kõik failid on õigus saada tihendamise.
>
> Toodete Azure'i CDN faili peab olema MIME tüüp, mis on [konfigureeritud lubama tihendamise](#enabling-compression).
>
> **Azure'i CDN Verizon** profiilid (Standard- ja Premium) toetavad **gzip**, **deflate**või **bzip2** kodeerimine.  **Azure'i CDN kaudu Akamai** profiilid toetavad ainult **gzip** kodeerimine.
>
> **Azure'i CDN kaudu Akamai** lõpp-punktid alati päringu **gzip** kodeeritud failide päritolu, olenemata kliendi taotlus.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Keelatud tihendamise või fail ei vasta nõuetele pakkimine

|Kliendi soovitud vorming (kaudu Aktsepteeri kodeerimine päis)|Vahemällu salvestatud failivormingus|Kliendi CDN vastust|Märkmete|
|----------------|-----------|------------|-----|
|Tihendatud|Tihendatud|Tihendatud|   |
|Tihendatud|Tihendamata|Tihendamata|    | 
|Tihendatud|Vahemälust|Tihendatud või tihendamata|Sõltub origin vastus|
|Tihendamata|Tihendatud|Tihendamata|    |
|Tihendamata|Tihendamata|Tihendamata|    |   
|Tihendamata|Vahemälust|Tihendamata|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Lubatud tihendamine ja fail on õigus tihendamine

|Kliendi soovitud vorming (kaudu Aktsepteeri kodeerimine päis)|Vahemällu salvestatud failivormingus|Kliendi CDN vastust|Märkmete|
|----------------|-----------|------------|-----|
|Tihendatud|Tihendatud|Tihendatud|CDN transcodes vahel Toetatud vormingud|
|Tihendatud|Tihendamata|Tihendatud|CDN sooritab tihendamine|
|Tihendatud|Vahemälust|Tihendatud|CDN teostab tihendamise origin tagastab tihendamata.  **Azure'i CDN Verizon** edastavad esimese taotluse tihendamata faili ja seejärel tihendada ning vahemälu faili edaspidised taotlused.  Failide jagamine `Cache-Control: no-cache` päise pole võimalik tihendada. 
|Tihendamata|Tihendatud|Tihendamata|CDN sooritab rõhu|
|Tihendamata|Tihendamata|Tihendamata|     |  
|Tihendamata|Vahemälust|Tihendamata|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN tihendamine

Media Services CDN lubatud streaming lõpp-punktid, tihendamise on vaikimisi jaoks järgmisi sisutüüpe: rakenduse/vnd.ms-sstr XML-i, application/dash+xml,application/vnd.apple.mpegurl, rakenduse/f4m + XML-i. Te ei saa lubada või keelata tihendamise mainitud tüüpi Azure'i portaalis.  

## <a name="see-also"></a>Vt ka
- [Tõrkeotsingu CDN faili tihendamine](cdn-troubleshoot-compression.md)    
