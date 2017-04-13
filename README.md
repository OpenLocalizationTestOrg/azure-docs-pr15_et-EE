# <a name="azure-technical-documentation-contributor-guide"></a>Azure'i tehnilise dokumentatsiooni kaasautor juhend

Leidmist GitHub hoidla, mis majutab dokumentatsioon, mis on avaldatud Azure'i dokumentatsiooni Center [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)allikas.

See hoidla sisaldab juhiseid, et aidata teil oma tehnilise dokumentatsiooni osaleda.  Funktsiooni osaliste juhend artikleid loendi leiate teemast [registri](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Kaasa Azure dokumentatsioon

Täname teid huvitavate Azure dokumentatsiooni!

* [Osalus viisid](#ways-to-contribute)
* [Koodi läbiviimine](#code-of-conduct)
* [Teie panust Azure sisu kohta](#about-your-contributions-to-azure-content)
* [Hoidla asutuses](#repository-organization)
* [GitHub, Git ja selle hoidla kasutamine](#use-github-git-and-this-repository)
* [Kuidas kasutada allahindlusest käsitletavat teemat vormindamiseks](#how-to-use-markdown-to-format-your-topic)
* [Tagasiside, kommentaaride ja tugiteenused](./contributor-guide/feedback-and-comments.md)
* [Veel ressursse](#more-resources)
* [Kõigi osaliste juhend mõisteid](./contributor-guide/contributor-guide-index.md) (avatakse uus leht)

## <a name="ways-to-contribute"></a>Osalus viisid 

Saate kaasa [Azure dokumendid](http://azure.microsoft.com/documentation/) on mitu võimalust:

* Kaasa [Foorum arutelu](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Artiklid allosas Disqus kommentaaride edastamine.
* Saate hõlpsasti kaasa tehnilise artikleid GitHub kasutajaliides. Kas leiate artiklist selle hoidla või lugege artiklit [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) ja klõpsake linki artikli, mis läheb GitHub allika artikkel.
* Kui muudate olulisi olemasoleva artikkel, lisamine või muutmine pilte või uus artikkel, mis aitavad peate kahvli selle hoidla, installida Git Bash, allahindlusest Valimisklahvistik, ja saate teada, mõned git käsud.

##<a name="code-of-conduct"></a>Koodi läbiviimine

Selle projekti vastu võtnud [Microsoft avatud allika juhendi](https://opensource.microsoft.com/codeofconduct/). Lisateavet teemast [KKK läbiviimine koodi](https://opensource.microsoft.com/codeofconduct/faq/) või kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) täiendavad küsimused või kommentaarid.

##<a name="about-your-contributions-to-azure-content"></a>Oma panus Azure sisu kohta

###<a name="minor-corrections"></a>Väike parandused

Väikeste muudatuste tegemiseks või selgitused dokumentatsiooni ja koodi näited selle repo edastamist hõlmatud [Azure'i veebisaidi kasutamine (ToU)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Suuremate edastuste

Kui esitate pull taotluse uue või olulise muudatustega seotud dokumendid ja koodi näited, saadame kommentaari ka GitHub palume teil esitada on online osakaal litsentsi lepingu (CLA), kui teil on üks neist rühmadest:

* Microsoft avatud tehnoloogiate rühma liikmeid.
* Osaliste, kes ei tööta Microsofti.

Vajame veebivormi enne võtame pull kutse.

Täielikud üksikasjad on saadaval [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Hoidla ettevõttes

Azure'i-sisu hoidla sisu järgmiselt korraldamise ja dokumentatsiooni [Azure.Microsoft.com](http://azure.microsoft.com). See hoidla sisaldab kahte root kausta.

### <a name="articles"></a>\articles

*\Articles* kaust sisaldab dokumentatsiooni artikleid vormindatud allahindlusest failide *.md* -laiendiga.

Artiklid juurkaustas on avaldatud Azure.Microsoft.com tee *http://azure.microsoft.com/documentation/articles/ {artiklis-nime-ilma-md} /*.

* **Artiklit failinimed:** Vaadake [meie faili nime panemise juhised](./contributor-guide/file-names-and-locations.md).

Oma teenuse kaustas artiklid on avaldatud Azure.Microsoft.com tee *http://azure.microsoft.com/documentation/articles/service-folder/ {artiklis-nime-ilma-md} /*

* **Media alamkaustad:** *\Articles* kaust sisaldab kausta root directory artiklis meediumifailide *\media* sees, mis on iga artikkel piltidega alamkaustad.  Teenuse kaustad sisaldavad eraldi media kausta artiklite iga teenuse kaustas. Artikli pilt kaustade nimega artiklis faili miinus *.md* faililaiendit ühtemoodi.

### <a name="includes"></a>\includes

Korduvkasutatava sisu lisada ühe või mitme artikleid jaotiste loomine Vaadake [meie tehnilist sisu kasutatud kohandatud laiendid](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown Mallid

See kaust sisaldab lihtsa allahindlusest vormingut, peate artikli meie standard allahindlusest malli.

### <a name="contributor-guide"></a>\contributor-Guide

See kaust sisaldab artikleid, mis kuuluvad meie osalistele juhend.  

## <a name="use-github-git-and-this-repository"></a>GitHub, Git ja selle hoidla kasutamine

Kuidas täiendada, GitHub Kasutajaliidese abil saate oma väike muudatused ja kuidas kahvli ja klooni on märkimisväärselt suurem osakaalu hoidla kohta leiate teavet teemast [installimine ja häälestamine tööriistad loome ka GitHub](./contributor-guide/tools-and-setup.md).

Kui installite GitBash ja valige kohalik töötamiseks, juhised uue kohaliku töötamise haru loomise, muudatuste tegemist ja esitamine muutused tagasi peamine haru on loetletud [Git käske uus artikkel loomise või olemasoleva artikli värskendamine](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Harude

Me soovitame teil luua kohaliku töötamise harude, mis on suunatud teatud ulatuse muutmine. Iga haru tuleks piirata mõistet/artiklis sujuvamaks muutmine etapile ja vähendada Ühenda konfliktide võimalust.  Järgmised püüete on sobiv uue haru ulatuse.

* Uus artikkel (ja sellega seotud pilte)
* Õigekirja ja grammatika kontrollimine muudatused artikkel.
* Üle suure hulga artikleid (nt uue autoriõiguse jalusesse) rakendamine ühele vormingu muutmine.

## <a name="how-to-use-markdown-to-format-your-topic"></a>Kuidas kasutada allahindlusest käsitletavat teemat vormindamiseks

Kõik selles hoidla artikleid kasutada GitHub maitsestatud allahindlusest.  Siit leiate loendi ressursid.

- [Allahindlusest põhialused](https://help.github.com/articles/markdown-basics/)

- [Prinditava allahindlusest cheatsheet](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Meie allahindlusest redaktorid loendit vt [tööriistade ja häälestamine teema](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Artikli metaandmed

Artikli metaandmete võimaldab azure.microsoft.com veebisaidil teatud funktsioone, nt Autor omistamine, kaasautor omistamine, lingiridade, artiklis kirjeldused ja SEO optimeerimine kui ka aruannete Microsoft kasutab hindab sisu. Jah, on oluline metaandmete! [Siin on juhised tagada, et teie metaandmete on teinud õigus](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Sildid

Automatiseeritud sildid on määratud taotlusi aidake meil pull taotluse töövoo haldamine ja aidata teile teada, mis toimub päringu tõmmake tõmmata:

* Seotud osakaal litsentsileping
    * CLA ei ole vaja: muutmine on üsna väike ja ei nõua, et logite on CLA.
    * CLA nõutav: muutmine on üsna suur ja nõuab, et logite on CLA.
    * CLA allkirjastatud: kaastöö allkirjastatud CLA, et tõmmata taotluse nüüd liikuda edasi läbivaatamiseks.
* Siltide sammas: sildid nagu PnP, tänapäevased rakendused ja TDC aidata liigitada pull taotlusi sisemine organisatsioon, mis tuleb pull taotlus läbi vaadata.
* Muuda saadetud Autor: autor on teatatud ootel pull kutse.

## <a name="more-resources"></a>Veel ressursse

Vaadake [meie kaasautori juhend register](./contributor-guide/contributor-guide-index.md) kõigi juhised teemade.
