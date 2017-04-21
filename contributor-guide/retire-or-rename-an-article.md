# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Juhiseid järgida pensionile või ACOM tehnilise artikli nime muutmine

Juhised on väikestele, kes on loetletud artiklis, mis tuleb tehniliste dokumentide osas azure.microsoft.com pensionile autorina. Juhised kehtivad ka siis, kui fail on ümber nimetatud.

Kui olete meie Azure kogukonnafoorumi liige ja teie arvates artikkel peaks selle kustutama mingil põhjusel, jätke kommentaari Disqus kommentaari voos, kui soovite, et leida midagi on valesti artikli autori artikkel.

VKE autorid on vaja mitut juhiste nõtkelt pensionile sisu, et veebisaidi kasutajate pole halb kogemus, kui me pensionile saidi sisu. Artikli kustutamine või muuta selle nime peaks olema Viimane asi, mis juhtub!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Samm 1: Määratud artiklist ei registri/ei jälgi ja uuesti avaldada selle (soovitatav)

Tehke kõigepealt on, uuesti avaldada artiklit nimega ei index/ei jälgi mõni nädal enne tegelikult kustutada. See on kaaluda parima tava "eelse töö" pensionile sisu. See eemaldab artiklist search engine registrid nii, et inimesed ei leia artiklist otsing. [Lugege teemat sisemise viki üksikasju.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Samm 2: Haldamine sissetulev lingid (nõutav)

Kindlaks teha, kui seal on mis tahes mitte-Microsofti sissetulev linke sisu. Sageli, Ajaveebid, Foorumid ja muud sisu veebis osutab artikleid. Sageli, saate töötada ajaveebi omanikud neid linke muuta, ja saate eemaldada või värskendada lingid foorum postitusi. Web analytics tööriistad teile öelda, kui seal on veebiaadresside sissetulev lingid sel viisil haldamiseks peate.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Samm 3: Eemaldada kõik crosslinks artiklist tehnilise sisu hoidla (nõutav)

Toetuvad ümbersuunamised crosslinks teiste artiklite hoida. Värskendamine või eemaldamine rist viitab artikli teil on kustutamine ning ümbernimetamine, sh artiklid kuulub teised inimesed.

1. Veenduge, et töötate ajakohane kohaliku haru – käivitada `git pull upstream master` (või selle käsu korral variatsioon.

2.  Skannige Azure'i sisu hind/artiklid kausta ja azure-sisu-hind/sisaldab kausta, mis tahes artikleid ja sisaldab selle artikli, mida soovite pensionile ja kas eemaldamine on crosslinks link või asendage need on sobiv uue crosslinks. Saate kasutada otsingu ja asendada kasuliku soovitud crosslinks leidmiseks, kui teil on installitud. Kui te pole, saate kasutada Windows PowerShelli tasuta! Siin on PowerShelli kasutamine on crosslinks leidmiseks:

 lisamine. Käivitage Windows PowerShelli.

 b. Muutke PowerShelli käsuviibas azure-sisu-pr\articles kausta:

 `cd azure-content-pr\articles`

 c. Tippige see käsk, mis loetleb kõik failid, mis viite kustutate artiklist:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Kui soovite saata faili nimede loendi tekstifaili (juhul, nimega psoutput.txt), saate teha järgmist.

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Lisamine ja kõigi muudatuste kinnitamiseks, lükake toidulauani ja muudatuste toidulauani liikumiseks juhtslaidi haru peamise hoidla pull koosolekukutse loomiseks.

## <a name="step-4-update-the-fwlink-tool-required"></a>Samm 4: Värskendada FWLink tööriist (nõutav)

Edasisuunamislink: tööriista otsida mis tahes turundusmaterjalide juurde suunavad lingid, mis võib viidata artiklit. Nihutage kursor mis tahes turundusmaterjalide juurde suunavad lingid asendamine sisu; kui te ei ole alias (pseudonüüm), mis kuulub link, sellega liituda. Kui omanikud ei Värskenda link, failide lisamine Piletite koos MSCOM on link, mis on muutunud. Lisateave - [sisemise viki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Juhis 5: Eemaldage kõik crosslinks artiklit muude lehtede azure.microsoft.com ja suunata endine lehe loomine kui vastav (nõutav)

Peate töötada isik, kes säilitab ja värskendab dokumentatsiooni sihtleht see osa teie teenuse jaoks. Oma sisu meeskonnatöö partneri poole, kui te ei tea, kes see isik on. Isik, kes säilitab ja värskendab dokumenti sihtleht tuleb teha kaks toimingut:

1. Visual Studio skannida **kogu** ACOM web lahendus ristviidete pensionile faili. Soovitud ristviidete eemaldage või asendage need on värskendatud ristviidete. Peate eemaldada linkide HTML-i kui ka seotud ressursside stringid HTML lingid. Lisateave – vt [ettevõtte viki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Kui asendamine artikkel, luua suunata. Lisateave – lugege teemat [sisemise viki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Märkige ruut hoidlasse muudatused.

## <a name="step-6-retire-the-article"></a>Samm 6: Pensionile artikkel

Kui olete eelnevalt kirjeldatud juhiseid ja need muudatused on reaalajas, saate kustutada selle artikli hoidla. 

**Oluline:** Kui kustutate faili, kasutage funktsiooni `git add --all` käsk.

## <a name="step-7-remove-links-from-msdn-required"></a>Juhis 7: Linkide eemaldamine MSDN-i (nõutav)

Vaadake üle katkenud linkide endine või ümbernimetatud spikriteema sisu kv tööriista ja Eemalda/Paranda linkide kõigi MSDN-i teemade mõjutatud.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Samm 8: Vahemällu talletatud lehtede eemaldamine otsingumootorid (ainult vajadusel)

Tehke seda ainult, kui sisu on vaja kiiresti eemaldada tõttu juriidiliste või raske kliendi probleemid. Kohta Google põhitõed, tavaline prioriteet lehe kustutamised lihtsalt käsitleks füüsiline otsing mootori protsessid. Avage need veebilehed vahemällu talletatud veebilehtede eemaldamiseks otsingumootorid.

[Bingi](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)
