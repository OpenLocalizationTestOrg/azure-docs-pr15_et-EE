# <a name="quality-criteria-for-pull-request-review"></a>Kvaliteedi kriteeriumid pull taotlus läbi vaadata.

Kriteeriumide eesmärk on luua ja hallata tehnilised artiklid autorite jaoks ja tõmmake taotluse läbivaatajate läbi sisu pull taotlusi. Kui teie taotlus pull ei kvalifitseeru [automaatse ühendamise](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), vaadatakse selle inimese pull taotluse läbivaataja. Tõmmata taotluse läbivaatajad üldiselt vaadata ainult, mis on uut või muudetud. Pull taotluse läbivaatajad hindab pull taotluse vastavalt blokeerimise muudatusi ja selles artiklis toodud-blokeerimine kvaliteedi läbivaatus üksused.

## <a name="blocking-content-quality-items"></a>Sisu kvaliteedi üksuste blokeerimine

Värskenduste saamiseks taotluse peab vastama järgmistele kriteeriumitele, et ühendada. Pull taotluse läbivaatajad tagasiside saamiseks taotluse kommentaarid neid üksusi ja tippige `#hold-off` pull taotluse tagastamiseks (autor) teie tagasisidet.

| Kategooria | Üksuse kvaliteedi läbivaatus |
|----------|---------------------|
|Eeltingimused| Funktsiooni PR. on määratud "Valmis – kui soovite-kooste" ja "kinnitamine õnnestus" Sildid|
|Eeltingimused| Ühenda konflikti ei saa pull taotluse blokeerida.|
|Repo jõustamine|    Pole selge sisu regressioon sisaldab pull taotlus.|
|Repo jõustamine|    Pull koosolekukutse ei sisalda on manustatud repo või ebatavalised, kõrvalised faile.|
|Repo jõustamine |Pull taotlus sisaldab vähem kui 100 muudetud faile, kui PR tahtlikult värskendab väljaanne haru juhtslaidi kaudu. (Tõesti, PR peaks sisaldama palju vähem kui, kuid pärast 100 muudetud faili, ei GitHub kuvamine soovitud diff).|
|Nime andmise |Uute failide failinimedes järgige [faili nime panemise juhised](file-names-and-locations.md).|
|Nime andmise |Uute kaustade kasutusele repo jälgi [kausta nime juhised](file-names-and-locations.md#folder-names-in-the-repo).|
|Sisu    |See artikkel on tehniline dokument, ja seetõttu õige sisu kanali. Vaadake selle [mis juhtub, kui juhised](content-channel-guidance.md).|
|Sisu    |Tehniline dokumendi sisu on asjakohane tehniline artikkel. Vaadake selle [mis juhtub, kui juhised](content-channel-guidance.md).|
|Sisu    |See artikkel sisaldab ka Sissejuhatus ja sisu kontseptuaalne või toimingute kogum. See artikkel peab sisaldama piisavalt, täieliku sisu iseseisvalt artiklit. Tuleb väike fragment teavet. (Erand: "Piirangud" teema, kui see on suur artikkel, kus on loetletud kõik piirangud teenuse raames.)|
|Sisu| Mida peaks olema numberloendeid elemendid, elemente, mis peaks olema loendite unordered täpp. See on lihtne kasutatavuse.|
|Sisu| Uudse või ebatavalised graafika, teabearhitektuur või struktuurid või ilmselt mittestandardsed kujunduse vaja olema kontrollitud koos müügivihje PR läbivaataja. Meeskonnad, mida on uut võimalust katsetamiset peab olema hindamise katsete probleem/lahendus lõuend või leping.|
|Saidikujunduse funktsioonid| Lülititest kasutatakse ainult üleminek üle mitme versiooni kohta.|
|Saidikujunduse funktsioonid| Switchered artikleid pealkirjad sisaldada teavet, mis eristab iga artiklis teised artiklid switchered määramine.|
|Saidikujunduse funktsioonid| Käsitsi autoriks sisukordade ei ole lubatud. See artikkel peavad saama H2s selle lehe kohta TOC jaoks.|
|Saidikujunduse funktsioonid| Kui H2 pealkirjad on olemas, on see artikkel sisaldab vähemalt kaks H2 pealkirjad. Üks H2 Pealkiri abil loob ühe üksusega artiklis TOC. H2 pealkirjad tuleb kasutada enne H3 pealkirju Sisukorra on loodud tagamiseks.|
|Allahindlusest| HTML-vormingus: Allika sisu ei sisalda HTML-i plokk tasemel – mõnevõrra Tekstisisese HTML on lubatud – nt ülaindeks, allindeks, erimärkide ja muud väikesed asjad, mida te ei saa teha allahindlusest. HTML-i tabelid on lubatud ainult siis, kui tabel sisaldab täpp-või numberloendi, kuid see on tavaliselt sisu vaja lihtsustada nii, et allika võib kodeerida allahindlusest märge.|
|Allahindlusest   |Vajaduse korral kasutada kohandatud allahindlusest elemendid. Ex: Kodeeritakse märkmete abil soovitud AZURE. Pange tähele laiend, ei lihttekstina.|
|SEO    |Funktsiooni "& #124; Microsoft Azure'i"saidi ID on nõutav|
|SEO    |H1 pealkiri sisaldab piisavalt teavet selle artikli eristab seda muud Azure artiklid ja vastendamiseks tõenäoliselt kliendi märksõnade sisu kirjeldamiseks. Näiteks kui H1 pealkiri "ülevaade" on fail.|
|Terminoloogia| ARM lühend, V1 või V2 viidetena klassikaline ja ressursihaldur juurutamise mudelite kasutamine blokeerimise terminoloogia üksus.|
|Pildid |Animeeritud GIF-id ei aktsepteerita repo sisse.|
|Pildid | Pildid on selge eraldusvõimega, on valesti kirjutatud sõnad ja privaatne andmed sisaldavad | 
|Lavastus|Artikli eelvaate peab olema puhas lavastus kohta. See ei tohi sisaldada mis tahes selge vorminguprobleemid: <br><br>-Täpp- või numberloendi loend, mis kuvatakse lõik <br> -Osaliselt kood plokk ja osaliselt väljaspool seda koodi plokk kood <br>-Valesti nummerdatud tõttu vigase taande nummerdatud juhised|

## <a name="non-blocking-content-quality-items"></a>Mitte-blokeerimine sisu kvaliteedi üksused

Nende üksuste jaoks pull taotluse läbivaatajad sisestage tagasiside ja autor koos paranduste hiljem pull taotluse järeltegevuse juhised. Siiski, ei takista see tagasiside otsust ühendamiseks. Autorite järeltegevus koos parandustega 3 tööpäeva jooksul.

| Kategooria | Üksuse kvaliteedi läbivaatus |
|----------|---------------------|
|Sisu|Artiklid peaks olema "järgmised sammud" 1 – 3 asjakohaste ja pilkupüüdvaid järgmise sammu lõpus. Lühike teksti tuleks lisada, mis aitab kliendi aru, miks on oluline järgmiste juhiste juurde. (Uus artiklid ainult). Näide: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Sisu|Õigekiri, grammatika ja muud kirjutamist probleemid - pull taotluse läbivaatajad võib tagasisidet mõnevõrra teemadele nagu-blokeerimine tagasiside. Kui loendis on rohkem kui mõned sisu, logige läbivaatajate Redigeeri taotlus see artikkel just pärast avaldamist Redigeeri.|
|Pildid|Pilte kasutada õige viikteksti laad ja värv, ja kuvatõmmised õige ääris ja kohatäite laad. [Vt pilti](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Pildid|Piltide kaasata asetekst. [Vt pilti](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Saidikujunduse funktsioonid|H2 pealkirjad, kui TOC-leht, tuleks Mahuta ideaalvariandis mitte rohkem kui 2 rida. Rohkem pealkirjad võimaldavad artiklis TOC raskem skannida.|
|Laadi põhimõtted|Kõik pealkirjad ja pealkirjad on lausealguse suurtähed, Azure laadi kohta.|
|Protsess|Kui pull kutse saanud on hõlpsalt ümber konfigureeritud kasu PRmerger automaatika, pull taotluse läbivaatajad tagasisidet autori kohta, kuidas kasutada harude, nii et muudatusi võiks liita automaatselt. [PR etiquette](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically)artiklist.|
|Protsess|Kui kustutate või artikkel ümber nimetada, veenduge, et järgite protsess. Tõmmata taotluse läbivaatajad tuleks lisada kommentaari järgmisele kommentaar ja link:<br><br>*Kontrollige, kas olete täitnud protsess on osaliste juhendist kustutamine artikleid: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Seotud

- [Pull taotluse etikett ja Microsoft osaliste head tavad](contributor-guide-pull-request-etiquette.md)

- [Tõmmata taotluse kommentaari automatiseerimine](contributor-guide-pull-request-comments.md)
