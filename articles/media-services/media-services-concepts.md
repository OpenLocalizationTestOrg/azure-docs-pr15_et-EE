<properties 
    pageTitle="Azure Media Servicesi põhimõtet | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade Azure Media Services mõisted" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

#<a name="azure-media-services-concepts"></a>Azure Media Servicesi mõisted 

Selles teemas antakse ülevaade kõige olulisemad Media Servicesi põhimõtet.

##<a id="assets"></a>Varad ja salvestusruumi

###<a name="assets"></a>Varad

[Varade](https://msdn.microsoft.com/library/azure/hh974277.aspx) sisaldab digitaalse faile (sh video, heli, pilte, pisipiltide saidikogumid, teksti jälitab ja tiitrite failid) ja nende failide metaandmeid. Pärast digitaalse failid üles arvesse vara, saab ta Media Servicesi kodeering ja streaming töövood.

Vara on vastendatud bloobimälu container Azure Storage konto ja vara failid on salvestatud plekid selle ümbrises.

Meediumi sisu üles laadida ja talletada vara otsustamisel kehtivad järgmisega.

- Vara peaks sisaldama ainult ühe, kordumatu eksemplari meediumisisu. Näiteks ühe Redigeeri TV episood, filmi või reklaami.
- Vara ei tohi sisaldada mitme renderduste või audiovisuaalse faili muudatused. Üks näide valest kasutamise vara oleks proovite salvestada rohkem kui üks TV episood, reklaami või mitme kaamera nurgad ühe toodetud vara sees. Mitme renderduste või muudatused audiovisuaalse faili salvestamise vara võib põhjustada probleeme esitamise kodeering töö, streaming ja hiljem vara kohaletoimetamise tagamiseks.  

###<a name="asset-file"></a>Varade fail 
Mõne [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) tähistab tegeliku video või heli faili, mis on talletatud bloobimälu ümbrises. Rakenduse varade fail on alati seostatud vara ja vara võib sisaldada ühe või mitme faili. Tööülesande Media Services kodeerija nurjub, kui objekti varade fail pole seotud digitaalne faili bloobimälu ümbrises.

**AssetFile** eksemplar ja tegeliku media fail on kaks erinevat objekti. AssetFile eksemplari sisaldab metaandmeid meediumifail, samal ajal media fail sisaldab tegelik meediumisisu.

Te ei peaks proovida bloobimälu ümbriste loodud Media Servicesi meediumi teenuse API-de kasutamata sisu muuta.

###<a name="asset-encryption-options"></a>Varade krüptimise suvandid

Sõltuvalt sisu, mida soovite üles laadida, talletada ja esitada, Media Servicesi pakub mitmesuguseid krüptimise võimalusi, mille saate valida.

**Ükski** Kasutatakse krüptimist. See on vaikeväärtus. Pange tähele, et selle suvandi kasutamisel sisu pole kaitstud teel või ülejäänud salvestusruumi.

Kui plaanite esitamisel on MP4 abil järk-järgult allalaadimine, kasutage seda suvandit sisu üleslaadimiseks.

**StorageEncrypted** – kasutage seda suvandit Eemalda sisu kohalikult abil AES 256 bitises krüptimist krüptimiseks ja laadige Azure Storage salvestuskoha krüptitakse ülejäänud. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite oma kõrge kvaliteediga Sisestuskeel meediumifaile tugev krüptimist ja ülejäänud vaba. 

Salvestusruumi krüptitud varade osutamiseks tuleb konfigureerida vara kohaletoimetamise poliitika, et Media Servicesi teaksid, kuidas soovite esitada oma sisu. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu sisu abil määratud kohaletoimetamise poliitika (nt AES, PlayReady või krüptimine pole). 

**CommonEncryptionProtected** – kasutage seda suvandit, kui soovite krüptida (või üles juba krüptitud) sisu levinud krüptimise või PlayReady DRM (näiteks sujuva voogesituse kaitsega PlayReady DRM).

**EnvelopeEncryptionProtected** – kasutage seda suvandit, kui soovite kaitse (või üles juba kaitstud) HTTP Live Streaming (HLS) krüptitud koos täiustatud Standard (AES). Pange tähele, et juba krüptitud AES HLS üleslaadimisel see peab on krüptitud Manager muuta.

###<a name="access-policy"></a>Accessi poliitika 

Mõne [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) määratleb õigused (nt lugemine, kirjutamine ja loendi) ja kestuse Accessi varade. Tavaliselt oleks lähevad objekti AccessPolicy locator, et kasutataks juurdepääsu vara failid.


###<a name="blob-container"></a>Container bloobimälu

Bloobimälu container pakub rühmitamise plekid kogumi. Bloobimälu ümbriste kasutatakse Media Servicesi piiri punktina juurdepääsu reguleerimine ja ühiskasutuses Accessi allkirja (SAS) lokaatorid varad. Azure Storage konto võib sisaldada bloobimälu ümbriste piiramatu arv. Ümbris saate talletada plekid piiramatu arv.

>[AZURE.NOTE]Te ei peaks proovida bloobimälu ümbriste loodud Media Servicesi meediumi teenuse API-de kasutamata sisu muuta.

###<a id="locators"></a>Lokaatorid

[Üldine ressursiaadress](https://msdn.microsoft.com/library/azure/hh974308.aspx)s pakuvad sisendpunkti juurdepääsu vara failid. Õigused ja kestus, et kliendil on juurdepääs antud varade määratlemiseks kasutatakse Accessi poliitika. Lokaatorid võib olla mitu-üks seos on juurdepääs poliitika nii, et eri lokaatorid pakkuda muu algusaegade ja ühenduse tüüpi eri klientidele kõik kasutades sama õiguste ja kestus sätted; siiski tõttu ühiskasutusega juurdepääsu poliitika seatud piirang Azure storage teenused, ei tohi olla kuni viis kordumatu lokaatorid seotud konkreetse vara korraga. 

Media Services toetab kahte tüüpi lokaatorid: OnDemandOrigin lokaatorid, kasutatakse voogesituseks meediumi (nt MÕTTEKRIIPSU MPEG, HLS või sujuva voogesituse) või järk-järgult allalaadimine meediumid ja SAS URL-i lokaatorid, kasutatakse üles ega alla laadida media failide to\from Azure salvestusruumi. 

Pange tähele, et loendi õigus (AccessPermissions.List) ei tohi kasutada ka OrDemandOrigin locator loomisel. 

###<a name="storage-account"></a>Salvestusruumi konto

Kõigi juurdepääsu Azure Storage tehakse salvestusruumi konto kaudu. Meediumi teenusekonto saate seostada ühe või mitme salvestusruumi kontod. Konto võib sisaldada piiramatu arvu ümbriste, kui kogumahtu on alla 500TB salvestusruumi konto kohta.  Media Services pakub SDK taseme instrumentaarium abil saate hallata mitut salvestusruumi kontot ja laadimine saldo mõõdikute või juhusliku jaotuse põhjal oma varasid jaotuse ajal Laadi üles, et need kontod. Lisateavet leiate teemast [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx)töötamise. 

##<a name="jobs-and-tasks"></a>Ülesannete ja

Tavaliselt kasutatakse [töö](https://msdn.microsoft.com/library/azure/hh974289.aspx) -protsess (nt indekseerida või kodeerida) heli-ja videokõnede ühes esitluses. Kui teil on mitu videot, looge iga video kodeeritud töö.

Töö sisaldab metaandmeid tuleb teha töötlemiseks. Iga töö sisaldab ühe või mitme [tööülesande](https://msdn.microsoft.com/library/azure/hh974286.aspx)s määratud atomic töötlemine tööülesande, selle Sisestuskeel varad väljund varad, meediumi protsessor ja selle seotud sätted. Töö ülesandeid võib olla ühendatud, kus väljundi varade ühe tööülesande manustatakse Sisestuskeel varade järgmise ülesande. Sel viisil ühe töö võib sisaldada kõik vajalikud esitluse meediumi menetluse.

##<a id="encoding"></a>Kodeerimine

Azure Media Servicesi pakub meediumi pilveteenuses kodeerimine mitu võimalust.

Kui alustamist Media Servicesi, on oluline kodekite ja failivorminguid erinevus.
Kodekite on tarkvara, mis rakendab tihendab mahukaid algoritmide failivormingud on tihendatud video reguleerivad ümbriste.

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 või sujuva voogesituse kodeeritud sisu streaming Media Services (MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse, HDS) poolt toetatavad pole vaja uuesti sisse neid vorminguid streaming paketti.

[Dünaamiliste pakendit](media-services-dynamic-packaging-overview.md)ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili kogumi kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide (kodeering juhiseid on näidatud allpool selle õpetuse).
- Vähemalt üks nõudmisel streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [skaala nõudmisel Streaming reserveeritud üksused](media-services-portal-manage-streaming-endpoints.md).

Media Services toetab nõudmisel kodeerimisseadmetest, mida selles artiklis kirjeldatakse järgmist:

- [Media kodeerija Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media kodeerija Premium töövoo](media-services-encode-asset.md#media-encoder-premium-workflow)

Toetatud kodeerimisseadmetest kohta leiate teavet teemast [kodeerimisseadmetest](media-services-encode-asset.md).


##<a name="live-streaming"></a>Voogedastuse

Kanali tähistab Azure Media Servicesi, müügivõimaluste töötlemiseks reaalajas streaming sisu. Kanali võtab reaalajas Sisestuskeel voogu on kaks võimalust:

- Kohapealse reaalajas kodeerija saadab kanali mitme bitikiirusega RTMP või sujuva voogesituse (killustatud MP4). Saate kasutada järgmisi reaalajas kodeerimisseadmetest, mis väljasta mitme bitikiirusega sujuva voogesituse: oksiidvormis Envivio, Cisco. Järgmised reaalajas kodeerimisseadmetest väljund RTMP: Adobe Flash Live, Telestreami Wirecast ja Tricaster transcoders. Sissevõetava voogu läbida kanalite ilma edasiseks töötlemiseks. Nõudmisel pakub Media Servicesi voo klientidele.

- Ühe bitrate voo (ühes järgmises vormingus: RTP (MPEG-TS)), RTMP või sujuva voogesituse (killustatud MP4)) saadetakse kanal, mis on lubatud teha reaalajas kodeerimine Media teenustega. Kanali seejärel sooritab reaalajas kodeerimine sissetulevad ühe bitrate voo mitme bitrate (kohandatava) videovoo abil. Nõudmisel pakub Media Servicesi voo klientidele.

###<a name="channel"></a>Kanali

Media Servicesi, [kanali](https://msdn.microsoft.com/library/azure/dn783458.aspx)s vastutate töötlemine reaalajas streaming sisu. Kanali pakub Sisestuskeel lõpp (neelata URL-i), et siis anda reaalajas transcoder. Kanali saab reaalajas Sisestuskeel voogu reaalajas transcoder ja teeb selle kättesaadavaks streaming ühe või mitme StreamingEndpoints kaudu. Kanalite pakuvad ka eelvaade lõpp (preview URL-i), mis abil saate eelvaade ja kinnitage oma voo enne edasiseks töötlemiseks ja kohaletoimetamisega.

Kanali loomisel võite saada neelata URL-i ja URL-i eelvaade. Idest saamiseks kanalit ei ole alustatud olekus olema. Kui olete valmis alustama, lükates andmete reaalajas transcoder kaudu sisse kanalit, tuleb kanalit luua. Kui reaalajas transcoder hakkab söömisega andmeid, saate vaadata oma voo.

Iga Media Servicesi konto võib sisaldada mitut kanalid, mitme programmi ja mitme StreamingEndpoints. Sõltuvalt läbilaskevõime ja turvalisuse erivajadustega, saate ühe või mitme pühendunud StreamingEndpoint teenused. Mis tahes StreamingEndpoint tõmmata mis tahes kanali kaudu.


###<a name="program"></a>Programm

[Programm](https://msdn.microsoft.com/library/azure/dn783463.aspx) võimaldab teil määrata avaldamise ja lõikude reaalajas voo salvestusruumi. Kanalite haldamine programmid. Kanali ja programmi seos on väga sarnane traditsioonilise meedia, kus kanali on pidev vool sisu ja programmi on rakendatud mõni selle kanali Ajastatud sündmus.
Saate määrata tundide, mida soovite säilitada, seades atribuuti **ArchiveWindowLength** programmi salvestatud sisu. Seda väärtust saab määrata vähemalt 5 minutit kuni 25 tundi.

ArchiveWindowLength näitab ka suurt hulka aja kliendid saavad taotleda ajas tagasi reaalajas praegust paigutust. Programmide käitamise üle määratud aja jooksul, kuid sisu, mis langeb pikkus akna taga pidevalt hüljatakse. See selle atribuudi väärtust määratleb ka kaua kliendi manifestid võivad kasvada.

Iga programmi on seostatud vara. Programmi avaldada, peate looma locator, on seotud vara. Millel see locator võimaldab teil koostada streaming URL, mida saate sisestada oma klientidele.

Kanali toetab kuni kolm samaaegselt töötavad programmid nii, et saate luua mitu sama sissetuleva voo arhiivi. See võimaldab teil avaldada ja arhiivi erinevate osade sündmuse vastavalt vajadusele. Näiteks oma ettevõtte nõue on arhiivida 6 tunni programmi, kuid ainult viimase 10 minuti levisaadet edastada. Selle saavutamiseks, peate looma kaks samaaegselt töötavad programmid. Ühes programmis on seatud arhiivida sündmuse 6 tunni, kuid programm ei ole avaldatud. Muu programmi on seatud arhiivida 10 minutit ja see programm on avaldatud.


Lisateavet leiate teemast:

- [Kanalid, mis on lubatud teha Live kodeerimine Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md)
- [Kanalite selle vastuvõtu mitme bitikiirusega Live voo kaudu kohapealse kodeerimisseadmetest töötamine](media-services-live-streaming-with-onprem-encoders.md)
- [Kui ka piirangud](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Sisu kaitsmine

###<a name="dynamic-encryption"></a>Dünaamiliste krüptimine

Azure Media Servicesi võimaldab teil kaitsta oma meediumi jätab arvuti kaudu säilitamiseks, töötlemiseks ja kohaletoimetamise aeg. Media Services võimaldab teil esitada oma sisu krüptitud dünaamiliselt täiustatud Standard (AES) (kasutades 128-bitine) ja kasutades PlayReady ja/või Widevine DRM levinud krüptimise (CENC). Media Servicesi pakub ka teenust kohaletoimetamiseks AES võtmed ja PlayReady litsentside volitatud klientidele.

Praegu saab krüptida streaming failivormingud: HLS, MPEG MÕTTEKRIIPSU ja sujuva voogesituse. Te ei saa krüptida HDS streaming vorming või järkjärguline allalaadimine.

Kui soovite Media Servicesi vara krüptimiseks, peate seostada teie vara krüptimisvõtme (CommonEncryption või EnvelopeEncryption) ja ka konfigureerimiseks autoriseerimine poliitikate võti.

Kui soovite voona salvestusruumi krüptitud varade, tuleb konfigureerida vara kohaletoimetamise poliitika selleks, et määrata, kuidas soovite oma varade esitamisel.

Kui voo nõuab mängija, kasutab Media Servicesi määratud klahvi krüptimiseks dünaamiliselt kasutades ümbriku krüptimise (koos AES) või levinud krüptimise (koos PlayReady või Widevine) sisu. Dekrüptida voo, mängija taotleb võti teenusest võtmed. Otsustada, kas kasutajal on õigus saada võti, hindab teenuse autoriseerimine poliitikate võti määratud.


###<a name="token-restriction"></a>Turbeloa piirang

Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avatud, sümboolne piirang või IP piiranguteta. Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned lihtne Web sõned (SWT) vorming ja JSON Web Turbeloa (JWT) vormingus. Media Servicesi ei paku Secure Turbeloa teenused. Saate luua kohandatud STS või kasutada Microsoft Azure ACS, et probleem sõned. Määratud võti ja probleemi nõuete Turbeloa piirang konfiguratsioonis määratud allkirjastatud märgiks loomiseks tuleb konfigureerida STS. Media Servicesi võtme teenus tagasi nõutud klahvi (või litsentside) kui luba on kehtiv ja taotluste Turbeloa kattuvad need konfigureeritud võti (või litsentside).

Kui poliitika konfigureerimise luba piiratud, peate määrama esmane kinnitamine klahvi, väljaandja ja sihtrühma parameetrid. Esmane kinnitamine võti sisaldab klahvi, mis on allkirjastatud luba, väljaandja on turvaline Turbeloa teenus, mis annab luba. Sihtrühma (mõnikord nimetatakse ulatus) kirjeldatakse soovidele vastavaks luba või ressursi luba lubab juurdepääsu. Media Servicesi võtme teenus kinnitab, et need väärtused luba vastavad väärtused mall.

Lisateavet leiate järgmistest artiklitest:

[Kaitse sisu ülevaade](media-services-content-protection-overview.md)
[kaitse koos AES 128](media-services-protect-with-aes128.md)
[DRM kaitse](media-services-protect-with-drm.md)

##<a name="delivering"></a>Pakkuda

###<a id="dynamic_packaging"></a>Dünaamiliste pakendit

Töötamisel on soovitatav on kohandatava bitrate MP4 Standard failide kodeerida Media Servicesi seadmine ja seejärel teisendage määramine [Dünaamilise pakendit](media-services-dynamic-packaging-overview.md)abil soovitud vorming.


###<a name="streaming-endpoint"></a>Lõpp-punkti streaming

Mõne StreamingEndpoint tähistab streaming teenus, mis suudab sisu otse kliendi Playeri rakendus või soovite sisu kohaletoimetamise võrk (CDN-ID) täpsemaks jaotuse (Azure Media Servicesi nüüd pakub integreerimine Azure CDN.) Väljamineva voo StreamingEndpoint teenuse saab reaalajas voo või video nõudmisel varade Media Servicesi kontol. Lisaks saate määrata StreamingEndpoint teenuse võimsus kasvab läbilaskevõime vajadustele reageerimine, reguleerides skaala üksused (nimetatakse ka streaming ühikud). Soovitatav on eraldada rakenduste tootmiskeskkonnas skaala üks või mitu üksust. Skaala üksuste teile asjakohast sealt võimsus, mida saab osta kerimine 200 Mbps nii lisafunktsioone, mis sisaldab praegu kasutada dünaamilise pakendit.

Soovitatav on krüptimise dünaamiline pakendit ja või dünaamiline. Nende funktsioonide kasutamiseks peab teil olema vähemalt üks streaming ühik, millest plaanite voona lõpp-punkti. Lisateabe saamiseks lugege teemat [mastaapimine streaming üksused](media-services-portal-manage-streaming-endpoints.md).

Vaikimisi on teil võib olla kuni 2 streaming Media Servicesi konto lõpp-punktid. Kõrgema piirmäära taotlemiseks vt artiklit [kvootide ja -piirangud](media-services-quotas-and-limitations.md).

Teil on ainult arve, kui teie StreamingEndpoint töötab olekus.

###<a name="asset-delivery-policy"></a>Varade kohaletoimetamise poliitika

Üks Media Servicesi sisu kohaletoimetamise töövoo juhised konfigureerib soovite olla voona [kohaletoimetamise poliitika varad ](https://msdn.microsoft.com/library/azure/dn799055.aspx). Varade kohaletoimetamise poliitika ütleb Media Servicesi, kuidas soovite oma vara kohaletoimetamiseks: üheks mis streaming protokoll peaks teie vara olema dünaamiliselt pakitud (näiteks MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse või kõik), kas soovite oma varade dünaamiliselt krüptimiseks ja kuidas (ümbriku või levinud krüptimise).

Kui teil on salvestusruumi krüptitud varade enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu abil määratud kohaletoimetamise poliitika sisu. Näiteks teie vara krüptitud Advanced Standard (AES) krüptovõtme osutamiseks seatud poliitika tüüp DynamicEnvelopeEncryption. Salvestusruumi krüptimise eemaldamine ja voona varade selge, tippige poliitika määramine NoDynamicEncryption.

###<a name="progressive-download"></a>Järk-järgult allalaadimine

Järk-järgult allalaadimine võimaldab teil alustamiseks meediumi enne kogu fail on alla laaditud. Saate vaid järk-järgult MP4 faili alla laadida.

Pange tähele, et peate dekrüptida krüptitud varad soovi neid järk-järgult allalaadimiseks saadaval olla.

Kasutajatele järk-järgult allalaadimine URL-id, esmalt peate looma ka OnDemandOrigin locator. Luua selle locator, pääsete base tee vara. Seejärel tuleb lisada MP4 faili nimi. Näiteks:

http://amstest1.streaming.mediaservices.Windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming URL-id

Streaming klientidele sisu. Kasutajatele streaming URL-id, esmalt peate looma ka OnDemandOrigin locator. Luua selle locator, pääsete base tee varade, mille sisu soovite taustal alla laadida. Siiski saama sisu voona peate muutma selle tee põhjalikumaks. Ehitada streaming Avaldamisfail täielik URL-i, peate concatenate soovitud locator tee väärtus ja manifest (filename.ism) faili nimi. Seejärel lisab locator tee /Manifest ja asjakohase vormingu (vajadusel).

Voogesituseks sisu SSL-ühenduse kaudu. Selleks, veenduge, et HTTPS Alusta voogesituse URL-ide.

Pange tähele, et ainult voogesituseks SSL kui streaming lõpp-punkti, kus saate esitada oma sisu on loodud pärast 10 mai 2014. Kui streaming URL-ide on loodud pärast mai 10 streaming lõpp-punktid, URL sisaldab "streaming.mediaservices.windows.net" (uus vorming). SSL-i ei toeta streaming URL-id, mis sisaldavad "origin.mediaservices.windows.net" (vana vorming). Kui teie URL on vana vormingus ja soovite üle SSL-i, luua uue streaming lõpp-punkti. URL-id, mis on loodud uue streaming lõpp-punkti põhineb abil sisu voona SSL.

Järgmises loendis kirjeldatakse streaming vorminguid ja näited.

- Sujuva voogesituse

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG MÕTTEKRIIPSU

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



- Apple HTTP Live Streaming (HLS) V4

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-AAPL)



- Apple HTTP Live Streaming (HLS) V3

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-AAPL-v3)

- HDS (jaoks Adobe PrimeTime Access kes ainult)

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.streaming.mediaservices.Windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-F4F)


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
