<properties 
   pageTitle="StorSimple hetktõmmise halduri kasutajaliidese | Microsoft Azure'i"
   description="Kirjeldab StorSimple hetktõmmise halduri kasutajaliides ja selgitatakse, kuidas seda kasutada varundamise ja varukoopia kataloogi."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="storsimple-snapshot-manager-user-interface"></a>StorSimple hetktõmmise halduri kasutajaliides

## <a name="overview"></a>Ülevaade

StorSimple hetktõmmise Manager on teie käsutusse intuitiivse kasutajaliidese, mille abil saate võtta ja hallata varukoopiad. Selle õpetuse annab kasutajaliidese sissejuhatuse ja seejärel selgitatakse, kuidas kasutada iga komponendi. StorSimple hetktõmmise halduri üksikasjaliku kirjelduse leiate teemast [mis on StorSimple hetktõmmise haldur?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsooli kirjeldus

Kasutajaliidese kuvamiseks ikooni StorSimple hetktõmmise halduri töölauale. Kuvatakse aken konsooli, nagu on näidatud järgmisel joonisel.

![StorSimple hetktõmmise halduri paanid](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Konsooli aknas on viis põhilist elementi. Klõpsake vastavat linki täielikku kirjeldust iga elemendi jaoks.

- [Menüüriba](#menu-bar) 
- [Tööriistariba](#tool-bar) 
- [Ulatus paan](#scope-pane) 
- [Otsingutulemite paan](#results-pane) 
- [Paanil toimingud](#actions-pane) 

Lisaks toetab StorSimple hetktõmmise halduri [klaviatuuri navigeerimis- ja otseteede arvu](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konsooli hõlbustusfunktsioonid

StorSimple hetktõmmise halduri kasutajaliides toetab hõlbustusfunktsioonide Windowsi operatsioonisüsteemi ja Microsoft Management Console (MMC) kui ka mõned StorSimple hetktõmmise halduri – konkreetset kiirklahvid. 

- Windowsi hõlbustusfunktsioonid kirjeldus, minge [kiirklahvid Windowsi](https://support.microsoft.com/kb/126449). 

- Hõlbustusfunktsioonide MMC kirjeldus, minge [MMC 3.0 hõlbustusfunktsioonid](https://technet.microsoft.com/library/cc766075.aspx)

- Minge StorSimple hetktõmmise halduri hõlbustusfunktsioonid kirjeldust [klaviatuuri navigeerimis- ja kiirklahvid](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menüüriba

Konsooli akna ülaosas menüüriba sisaldab [faili](#file-menu), [toimingu](#action-menu), [Vaade](#view-menu), [Lemmikud](#favorites-menu), [akna](#window-menu)ja menüüd [Spikker](#help-menu) .

Klõpsake menüüribal Saadaolevate käskude loendi kuvamiseks klõpsake selle menüü iga üksuse. Järgmises näites on kujutatud menüü **Vaade** valitud menüü.

![Valitud menüü Vaade](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Menüü fail

Menüü **fail** sisaldab standard Microsoft Management Console (MMC) käske.

#### <a name="menu-access"></a>Menüü juurdepääs

Menüü **fail** kuvamiseks klõpsake menüü **fail** . Kuvatakse järgmine menüü.

![Menüü fail StorSimple hetktõmmise haldur](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmises tabelis kirjeldatakse üksused, mis kuvatakse menüü **fail** .

| Menüükäsk | Kirjeldus |
|:----------|:-------------|
| Uue       | Klõpsake nuppu **Uus** , et luua uue konsooli vastavalt StorSimple hetktõmmise haldur. |
| Avatud      | Klõpsake, et **avada** olemasoleva konsooli avamiseks. |
| Salvestamine      | Klõpsake nuppu **Salvesta** praegune konsooli salvestada. |
| Salvesta nimega   | Klõpsake nuppu **Salvesta nimega** luua uue, ümbernimetatud eksemplari jooksva konsooli. Vaate kohandamine ja salvestage see hilisemaks allalaadimiseks suvandi **Salvesta nimega** abil. Näiteks võite luua StorSimple hetktõmmise halduri lisandmoodulid, mis osutavad teatud serverites. |
| Lisa/eemalda lisandmoodul | Klõpsake nuppu **Lisa/eemalda lisandmoodul** lisamiseks või eemaldamiseks lisandmoodulid ja korraldamiseks sõlmed **ulatus** paanil. Lisateabe saamiseks minge [lisamine, eemaldamine, ja korraldamine lisandmoodulid ning MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Suvandid   | Klõpsake nuppu **Suvandid** muuta konsooli ikooni, määrake kasutajale juurdepääs režiimi ja õigused või vaba ruumi vabastamiseks konsooli failide kustutamine. |
| Loendi failiteed | Klõpsake nummerdatud loendi uuesti viimati avatud faili tee. |
| Väljumine      | Klõpsake käsku **välju** sulgemiseks menüü **fail** . |
 
### <a name="action-menu"></a>Menüü Action

Valige saadaval toimingute menüü **Action** abil. Teile saadaolevad üksused sõltuvad **ulatus** paan või **otsingutulemite** paan valikule.

#### <a name="menu-access"></a>Menüü juurdepääs

**Toimingu** menüü kuvamiseks tehke ühte järgmistest.

- Paremklõpsake paanil **ulatus** või **otsingutulemite** paan.

- **Otsingutulemite** paan või **ulatus** paanil üksuse valimine, ja seejärel klõpsake **toimingut** menüüriba. 

Näiteks, kui **ulatus** paanil valige ülemine sõlm ja seejärel paremklõpsake või menüüribal nuppu **toiming** , kuvatakse järgmine menüü.
 
![Menüü StorSimple hetktõmmise halduri tegevus](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Soovitud **toimingud** (parempoolsel paanil konsooli) sisaldab **toimingu** menüü toimingud samas loendis. Lisaks sisaldab paanil **toimingud** **vaate** menüü Suvandid, mis võimaldavad teil luua kohandatud vaate **tulemuste** paanil.

![Avage toimingud aken, kus menüü Vaade](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmine tabel sisaldab tähestikulises loendit StorSimple hetktõmmise halduri toimingud. 

- Veerus **toiming** toiminguid, mida saate teha sõlmed ja tulemusi. 

- **Navigeerimispaanil** veerus selgitatakse, kuidas vastav **toimingu** menüü kuvamiseks nii, et saate valida soovitud toiming. Mitme **toimingu** menüü kuvatakse mõned toimingud. Järgmiste toimingute tegemiseks ühe **navigeerimise** võimalus valida täpploendi. 

- **Kirjelduse** veerg kirjeldab, kuidas kasutada iga toimingu menüü **toimingu** või toimingute paan ja selgitab, mida see tähendab.

>[AZURE.NOTE] Paanil **toimingud** ja **toiming** menüüdes sisaldavad lisasuvandid, nt **Vaade**, **Uus aken siit**, **värskendamine**, **Loendi eksportimine**ja **aidata**. Need suvandid on saadaval MMC osana ja pole seotud StorSimple hetktõmmise haldur. Tabel sisaldab kirjeldused järgmistest suvanditest.
 
| Toiming  | Navigeerimine  | Kirjeldus  |
|:--------|:------------|:-------------|
| Autentida | Klõpsake sõlme **seadmed** ja paremklõpsake seadme **tulemuste** paanil. | Klõpsake **autentimise** sisestada parool, mida seadme jaoks konfigureeritud. |
| Klooni  | Laiendage **Varukoopia kataloogi**, laiendage **Pilve pilte**, klõpsake kupongil varukoopia ja valige maht **tulemuste** paanil. | Klõpsake nuppu **klooni** hetktõmmise cloud koopia luua ja salvestada selle kohas, kus saate määrata. |
| Seadme konfigureerimine | Paremklõpsake sõlme **seadmed** . | Klõpsake ühe seadme või mitmes seadmes ühenduse Windows hosti konfigureerimiseks **konfigureerige seade** . |
| Poliitika varukoopia loomine | Tehke ühte järgmistest.<ul><li>Paremklõpsake **varukoopia poliitika**.</li><li>Klõpsake või Laienda **Rühmad helitugevust**ja paremklõpsake helitugevuse rühma.</li><li>Klõpsake või laiendamine **Varundamise kataloogi**ja paremklõpsake helitugevuse rühma.</li></ul> | Klõpsake **Luua varukoopia poliitika** konfigureerida ajastatud varundus helitugevuse rühma. |
| Helitugevuse rühma loomine | Tehke ühte järgmistest.<ul><li>Klõpsake **mahud** sõlme ja paremklõpsake maht **tulemuste** paanil.</li><li>Paremklõpsake sõlme **Helitugevuse rühmad** .</li></ul> | Klõpsake mahud määramine rühmale helitugevuse **Helitugevuse Loo rühm** . |
| Kustutamine | Klõpsake sõlm või tulemi (selle üksuse kuvatakse palju **toiming** menüüdes ja **toimingud** paanide.) | Klõpsake nuppu **Kustuta** sõlm või tulemi teie valitud kustutada. Kui kuvatakse kinnituse dialoogiboks, kinnitage või kustutamise tühistamiseks. |
| Üksikasjad | Klõpsake sõlme **seadmed** ja paremklõpsake seadme **tulemuste** paanil. | Klõpsake nuppu **üksikasjad** seadme konfiguratsiooni teabe kuvamiseks. |
| Redigeerimine | Klõpsake nuppu **Varundus poliitikate**ja paremklõpsake poliitika **tulemuste** paanil. | Klõpsake nuppu **Redigeeri** varunduse ajakava rühma Helitugevuse muutmiseks. |
| Loendi eksportimine | Klõpsake mis tahes sõlm või tulemi (selle üksuse kuvatakse kõik **toiming** menüüdes ja **toimingud** paanide.) | Klõpsake **Loendi eksportimine** loendi komaeraldusega (CSV) faili salvestada. Seejärel saate selle faili importida tabelarvutusrakendust analüüsi jaoks. |
| Abi | Klõpsake mis tahes sõlm või tulemi. (Selle üksuse kuvatakse kõik **toiming** menüüdes ja **toimingud** paanide.) | Klõpsake nuppu **Spikker** võrguspikker avamine eraldi brauseriaknas. |
| Uus aken siit | Klõpsake mis tahes sõlm või tulemi (selle üksuse kuvatakse kõik **toiming** menüüdes ja **toimingud** paanide.) | Klõpsake nuppu **Uus aken siit** StorSimple hetktõmmise halduri uues aknas avamiseks.|
| Värskendamine | Klõpsake mis tahes sõlm või tulemi (selle üksuse kuvatakse kõik **toiming** menüüdes ja **toimingud** paanide.) | Klõpsake käsku **Värskenda** kuvatud StorSimple hetktõmmise halduri akna värskendamine. |
| Seadme värskendamine | Klõpsake sõlme **seadmed** ja paremklõpsake seadme **tulemuste** paanil. | Klõpsake nuppu **Värskenda seadme** sünkroonima teatud ühendatud seadet StorSimple hetktõmmise halduriga. |
| Seadmete värskendamine | Paremklõpsake sõlme **seadmed** . | Klõpsake nuppu **Värskenda seadmete** StorSimple hetktõmmise halduriga ühendatud seadmete loendi sünkroonimiseks. |
| Uuesti mahud | Paremklõpsake sõlme **maht** . | Klõpsake **uuesti mahud** värskendamiseks mahud loend, mis kuvatakse **tulemuste** paanil. |
| Taastamine | Laiendage **Varukoopia kataloogi**, helitugevuse rühma laiendamine, laiendage **Kohaliku pilte** või **Cloud hetktõmmiseid**ja paremklõpsake varukoopia. | Klõpsake praeguse helitugevuse rühma andmete asendamiseks andmed valitud varukoopia põhjal **taastamine** . |
| Varukoopia tegemine | Tehke ühte järgmistest.<ul><li>Laienda **Rühmad helitugevust**ja paremklõpsake rühma helitugevust.</li><li>Laiendage **Varundamise kataloogi**ja paremklõpsake rühma helitugevust.</li></ul> | Klõpsake **Teha varukoopia** Varundustöö kohe alustada. |
| Lülita impordi kuvamine | Paremklõpsake ülemises paani **ulatus** ( **StorSimple hetktõmmise halduri** sõlme näiteid). | Klõpsake nuppu **Lülita impordi kuvamine** kuvamiseks või peitmiseks helitugevuse rühmade ja seotud varukoopiate StorSimple halduri teenuse armatuurlaual imporditud. |

### <a name="view-menu"></a>Menüü Vaade

Menüü **Vaade** abil saate luua kohandatud vaate **tulemuste** paanil sisu. Menüü **Vaade** sisaldab **Veergude lisamine või eemaldamine** ja **kohandamine** suvandid.

#### <a name="menu-access"></a>Menüü juurdepääs

Pääsete juurde menüüst **Vaade** menüü või paanil **toimingud** .

![Menüü Vaade StorSimple hetktõmmise haldur](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmises tabelis kirjeldatakse üksused, mis kuvatakse menüü **Vaade** .

| Menüükäsk  | Kirjeldus |
|:-----------|:-------------|
| Veergude lisamine või eemaldamine | Klõpsake käsku **Lisa/Eemalda veerud** veergude lisamine või eemaldamine **tulemuste** paanil. |
| Kohandamine | Klõpsake nuppu **Kohanda** StorSimple hetktõmmise halduri konsooli aknas üksuste kuvamiseks või peitmiseks. |

### <a name="favorites-menu"></a>Menüü Lemmikud

Menüü **Lemmikud** abil saate lisada, eemaldada ja korraldada lehe vaated ja toimingud, mida kasutate sageli. 

#### <a name="menu-access"></a>Menüü juurdepääs

Pääsete juurde menüü menüü **Lemmikud** .

![Menüü StorSimple hetktõmmise halduri lemmikud](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmises tabelis kirjeldatakse üksused, mis kuvatakse menüüs **Lemmikud** .

| Menüükäsk |  Kirjeldus |
|:----------|:-------------|
| Lisa lemmikute hulka | Klõpsake käsku **Lisa lemmikute hulka** praeguse vaate lisamiseks loendisse lemmikud. |
| Lemmikute korraldamine | Klõpsake nuppu **Korralda kausta Lemmikud** kausta Lemmikud sisu korraldamiseks. |

### <a name="window-menu"></a>Menüü aken

Lisamine ja ümberkorraldamine StorSimple hetktõmmise halduri konsooli Windowsi menüü **aken** abil.

#### <a name="menu-access"></a>Menüü juurdepääs

Pääsete juurde menüü menüü **aken** .

![Menüü StorSimple hetktõmmise Manager aken](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Numberloendi menüü allservas kuvatakse aknad, mis on praegu avatud. Klõpsake loendi toomiseks esiplaanil akna kõik aknad. 

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmises tabelis kirjeldatakse üksused, mis kuvatakse menüü aken.

| Menüükäsk  | Kirjeldus |
|:-----------|:-------------|
| Uues aknas | Klõpsake **Uues aknas** avamiseks uues aknas console (lisaks olemasoleva akna). |
| Kaskaadvärskenda   | Klõpsake **Kaskaadvärskenda** kaskaadlaadistiku laadi avatud konsooli windows kuvamiseks. |
| Paani horisontaalselt | Klõpsake **Paani horisontaalselt** kuvatakse avatud konsooli windows paan (või ruudustikus) vormingus. |
| Ikoonide korraldamine | Kui teil on mitu konsooli windows avamine ja laiali üle töölaua minimeerida need ja seejärel klõpsake nuppu **Korralda ikoonid** korraldada need reast ekraani allservas. |

### <a name="help-menu"></a>Menüü Spikker

Kasutage **menüü** saadaval elektroonilise spikri StorSimple hetktõmmise juhataja ja MMC. Samuti saate vaadata teavet MMC ja StorSimple hetktõmmise Manager tarkvara versioonide praegu teie arvutisse installitud. 

Pääsete juurde menüü menüü **Spikker** . Võite kasutada ka StorSimple hetktõmmise halduri spikriteemad paanilt **toimingud** .

![Menüü StorSimple hetktõmmise Manager spikker](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menüü kirjeldus

Järgmises tabelis kirjeldatakse üksused, mis kuvatakse menüü Spikker.

| Menüükäsk  | Kirjeldus  |
|:-----------|:-------------|
| StorSimple hetktõmmise Manager spikker | Klõpsake **spikri StorSimple hetktõmmise halduri** StorSimple hetktõmmise halduri spikri avamiseks eraldi aknas. |
| Spikriteemad |Klõpsake **Spikriteemad** MMC võrguspikker avamiseks eraldi aknas. |
| Veebisaidi Tehnokeskus | Klõpsake **Tehnokeskus veebisaidilt** Microsoft TechNeti Tech Center avalehe avamiseks eraldi aknas. |
| Microsoft Management Console kohta | Klõpsake **Microsofti halduskonsooli** näha, milline Microsoft Management Console versioon on teie arvutisse installitud. |
| StorSimple hetktõmmise haldur | Teie arvutisse on installitud lisandmooduli versiooni kuvamiseks klõpsake **StorSimple hetktõmmise haldur** . |

## <a name="tool-bar"></a>Tööriistariba

Tööriistariba, menüü all asuv sisaldab navigeerimise ja tööülesande ikoonid. Iga ikoon on otsetee kindla tööülesande.

### <a name="icon-descriptions"></a>Olekuikoonide kirjeldused

Järgmises tabelis kirjeldatakse ikoone, mis kuvatakse tööriistaribal. 

| Ikoon  | Kirjeldus  |
|:------|:-------------| 
| ![Vasaknool](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) | Eelmisele lehele naasmiseks ikooni vasaknoolt. |
| ![Paremnool](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) | Klõpsake paremnoolt järgmisele lehele (kui noolt on Hall, toiming on saadaval). |
| ![Üles ikoon](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) | Klõpsake üles ikooni konsoolipuus ( **ulatus** paan) ühe taseme võrra ülespoole liikumine. |
| ![Kuva/peida konsoolipuus](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) | Ikooni Kuva/peida konsooli puu kuvamiseks või peitmiseks paani **ulatust** . |
| ![Loendi eksportimine](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) | Loendi eksportimine CSV-faili, mis teie määratud ikooni loendi eksport. |
| ![Spikriikoon](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png)  |Klõpsake ikooni spikker online MMC spikriteema avamiseks. |
| ![Kuva/peida paanil toimingud](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) | Klõpsake nuppu Kuva/peida **toimingud** paanil ikooni, et kuvada või peita paani **toimingud** . 
 
## <a name="scope-pane"></a>Ulatus paan

Paani **ulatus** on vasakpoolse paani StorSimple hetktõmmise Manager UI. See sisaldab console (või sõlm) puu ja esmane navigeerimise süsteem StorSimple hetktõmmise haldur. 
 
### <a name="scope-pane-structure"></a>Ulatus paani struktuur

**Ulatus** paan sisaldab sarja klõpsatava objektide (sõlmed) korraldada puustruktuuris. 

![Ulatus paan](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

- Laiendamiseks või ahendamiseks sõlm, klõpsake noolt ikooni sõlm nime kõrval.

- Oleku või sõlm sisu vaatamiseks klõpsake sõlm nime. Teave kuvatakse **tulemuste** paanil. 

Paani **ulatus** sisaldab järgmised sõlmed: 

- [Seadmete sõlm](#devices-node) 
- [Mahud sõlm](#volumes-node) 
- [Helitugevuse rühmade sõlm](#volume-groups-node) 
- [Poliitika sõlme varundamine](#backup-policies-node) 
- [Varukoopia kataloogi sõlm](#backup-catalog-node) 
- [Töö sõlm](#jobs-node) 

### <a name="scope-pane-tasks"></a>Ulatus paanil ülesanded

Saate paani **ulatus** teatud sõlme toimingu lõpuleviimiseks. Tööülesande valimiseks tehke ühte järgmistest.

- Paremklõpsake sõlme ja valige seejärel kuvatavas menüüs tööülesanne.

- Klõpsake sõlme, ja seejärel klõpsake **toimingut** menüüriba. Valige kuvatavas menüüs tööülesanne.

- Klõpsake sõlme ja valige soovitud toiming paanil **toimingud** .

Kui valite sõlm ja kasutada mistahes meetodiga tööülesannete loendi kuvamiseks, kuvatakse ainult need toimingud, mida saab teha sõlme.

### <a name="devices-node"></a>Seadmete sõlm

**Seadmete** sõlm tähistab StorSimple seadmed ja StorSimple virtuaalse ühendatud seadmete StorSimple hetktõmmise haldur. Valige selle ühenduse loomiseks ja konfigureerimiseks seadme sõlme ja impordi oma seotud maht, mahud rühmad ja olemasolevaid varukoopiaid. Mitmes seadmes saab ühendada ühe host.

- Laiendage, ikooni **seadmed**kõrval olevat noolt.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **seadmete** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Konfigureeritud seadmete loendi vaatamiseks klõpsake paanil **ulatus** **seadmed** . Seadmete koos teabega iga seadme kohta loendis kuvatakse **tulemuste** paanil.

### <a name="volumes-node"></a>Mahud sõlm

**Mahud** sõlm tähistab draivid teineteisele draivid ühendada host, kaasa arvatud iSCSI ja nende abil seadme abil. Selle sõlme abil saate vaadata loendit saadaval mahud ja üksikute mahud helitugevuse rühmadesse määrata.

- Laiendage, klõpsake **mahud**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **mahud** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Mahud loendi vaatamiseks klõpsake paanil **ulatus** **maht** . Loendi maht, koos teabega iga helitugevuse kohta kuvatakse **tulemuste** paanil.

### <a name="volume-groups-node"></a>Helitugevuse rühmade sõlm

Helitugevuse rühmad on ka järjepidevuse rühmad. Iga rühma helitugevus on rakendusega seotud mahud kogumi, mis aitab tagada rakenduse ajal varukoopia. Abil **Helitugevuse rühmade** sõlm konfigureerida nende rühmade ja võtta interaktiivsed varukoopiate või luua varukoopia ajakava. 

- Laiendage, klõpsake **Helitugevuse rühmade**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **Helitugevuse rühmade** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Helitugevuse rühmade loendi vaatamiseks klõpsake paanil **ulatus** **Helitugevuse rühmad** . Helitugevuse rühmade koos helitugevuse iga rühma kohta teabe, kuvatakse **tulemuste** paanil.

### <a name="backup-policies-node"></a>Poliitika sõlme varundamine

Varukoopia poliitika on kohalik töö ajakava ja Pilveteenusest hetktõmmiseid luua. Kasutage **Varukoopia poliitika** sõlme määramiseks sageduse luuakse varukoopia ja kaua varukoopia tuleks säilitada. 

- Laiendage, klõpsake nuppu **Varundus poliitikate**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **Varundamise poliitikate** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Varukoopia poliitikate loendi vaatamiseks klõpsake paanil **ulatus** **Varukoopia poliitika** . Varukoopia poliitika, koos teabega iga poliitika kohta kuvatakse **tulemuste** paanil.

>[AZURE.NOTE] Saate säilitada kuni 64 varukoopiad.


### <a name="backup-catalog-node"></a>Varukoopia kataloogi sõlm

**Varundus kataloogi** sõlm sisaldab loendite Azure StorSimple mahud kohapeal ja väljapoole varukoopiaid. See sõlm on korraldatud helitugevuse rühma ja iga helitugevuse rühma container sisaldab eraldi struktuurid kohaliku hetktõmmiseid ( **Kohaliku hetktõmmise**s sõlme) ja pilveteenuse hetktõmmiseid ( **Pilve hetktõmmiseid** sõlme). Kui laiendatud, iga helitugevuse rühma container on ära toodud kõik interaktiivseks või konfigureeritud poliitika tehtud eduka varukoopiate.

- Laiendage, klõpsake nuppu **Varundus kataloogi**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **Varundamise kataloogi** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Varukoopia hetktõmmiste loendi vaatamiseks klõpsake paanil **ulatus** **Varukoopia kataloogi** . Hetktõmmiseid luua, koos teabega iga hetktõmmise kohta loendis kuvatakse **tulemuste** paanil.

### <a name="local-snapshots-node"></a>Kohaliku hetktõmmiseid sõlm

**Kohaliku hetktõmmiseid** sõlm on loetletud kohaliku hetktõmmiseid köite rühma. Sõlme asub jaotises **ulatus** paanil sõlme **Varundamise kataloogi** . Kohaliku hetktõmmiseid on helitugevuse andmeid, mis on talletatud Azure StorSimple seadme punkti /-kellaaja koopiad. Tavaliselt saab seda tüüpi varukoopia luua ja kiiresti taastada. Kohaliku hetktõmmise saate kasutada nii, nagu teeksite kohaliku varukoopia.

- Laiendage, klõpsake **Kohaliku hetktõmmiseid**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **Kohaliku pilte** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Kohaliku hetktõmmiste loendi vaatamiseks klõpsake paanil **ulatus** **Kohaliku pilte** . Hetktõmmiseid luua, koos teabega iga hetktõmmise kohta loend kuvatakse **tulemuste** paanil.

### <a name="cloud-snapshots-node"></a>Pilveteenuse hetktõmmiseid sõlm

**Pilveteenuse hetktõmmiseid** sõlm on loetletud cloud hetktõmmiseid köite rühma. Sõlme asub jaotises **ulatus** paanil sõlme **Varundamise kataloogi** . Pilveteenuse hetktõmmiseid on helitugevuse andmed, mis talletatakse pilves punkti /-kellaaja koopiad. Pilveteenuse hetktõmmise võrdub hetktõmmise kopeeritud erinev, väljapoole salvestusruumi süsteem. Pilveteenuse hetktõmmiseid on eriti kasulik Avariijärgne taaste stsenaariumid.

- Laiendage, klõpsake **Cloud hetktõmmiseid**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **Cloud pilte** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Pilveteenuse hetktõmmiste loendi vaatamiseks klõpsake paanil **ulatus** **Cloud hetktõmmiseid** . Hetktõmmiseid luua, koos teabega iga hetktõmmise kohta loendis kuvatakse **tulemuste** paanil.

### <a name="jobs-node"></a>Töö sõlm

**Töö** sõlm sisaldab teavet ajastatud, käivitatud ja viimati lõplikus varukoopia tööde haldamine. 

- Laiendage, klõpsake **töö**kõrval olevat noolt ikooni.

- Saadaval toimingute menüü kuvamiseks paremklõpsake sõlme **töö** või mis tahes sõlmed, mis kuvatakse laiendatud vaates.

- Ajastatud tööd loendi vaatamiseks laiendage **tööde haldamine** ja klõpsake **ajastatud**. Eelnevalt konfigureeritud töö ja iga töö teavet loendit kuvatakse **tulemuste** paanil. 

- Hiljuti lõpetatud projektide loendi vaatamiseks laiendage **tööde haldamine** ja klõpsake **viimase 24 tundi**. Loendi viimase 24 tunni valmis tööd, kuvatakse **tulemuste** paanil. **Otsingutulemite** paan sisaldab ka teavet iga lõpuleviidud töö.

- Töö käivitatud loendi vaatamiseks laiendage **tööde haldamine** ja klõpsake **töötab**. Loendis praegu töötavate töökohtade ja iga töö teavet kuvatakse **tulemuste** paanil.

## <a name="results-pane"></a>Otsingutulemite paan

**Otsingutulemite** paan on keskmisel paanil StorSimple hetktõmmise Manager UI. See sisaldab loendeid ja üksikasjaliku oleku teabe **ulatus** paanil valitud sõlm.

### <a name="example"></a>Näide

Järgmises näites vaatamiseks klõpsake paanil **ulatus** sõlme **Helitugevuse rühmad** . **Otsingutulemite** paan kuvatakse helitugevuse rühmade üksikasjadega iga rühma kohta.

![Otsingutulemite paan](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Saate konfigureerida **tulemuste** paanil üksikasjad: Paremklõpsake sõlme **ulatus** paanil, klõpsake menüüd **Vaade**ja seejärel klõpsake käsku **Lisa/Eemalda veerud**.

## <a name="actions-pane"></a>Paanil toimingud

Paanil **toimingud** on parempoolsel paanil StorSimple hetktõmmise Manager UI. See sisaldab menüü toimingud, mida saate teha sõlm, vaate või valite **ulatus** paan või **otsingutulemite** paan andmeid. Paanil **toimingud** sisaldab sama nimega **toiming** menüüdes **tulemuste** paanil ja **ulatus** paani üksuste jaoks saadaolevad käsud. Iga toimingu kirjelduse leiate teemast jaotises **toiming** käsku Tabel.

### <a name="examples"></a>Näited

Järgmises näites **ulatus** paanil kuvamiseks laiendage **töö** ja nuppu **ajastatud**. **Toimingud** paanil kuvatakse saadaolevad toimingud **ajastatud** sõlme.

![Toimingud paani ajastatud tööd näide](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Täiendavate suvandite vaatamiseks klõpsake paanil **ulatus** kuvamiseks laiendage **töö** , klõpsake **ajastatud**ja klõpsake plaanitud projekti **tulemuste** paanil. Paanil **toimingud** kuvab saadaolevad toimingud ajastatud tööd, nagu on näidatud järgmises näites.

![Toimingud paani töö toimingud näide](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Klaviatuuri navigeerimis- ja kiirklahvid

StorSimple hetktõmmise halduri võimaldab hõlbustusfunktsioonide Windowsi operatsioonisüsteemi ja Microsoft Management Console (MMC). See hõlmab ka mõned klaviatuurifunktsioonid navigeerimine ja kiirklahve, mis on teatud abil StorSimple hetktõmmise ülemus, nagu on kirjeldatud järgmistes lõikudes.
 
- [Klaviatuuri abil](#keyboard-navigation-keys) 
- [Menüü kiirklahvid](#menu-bar-shortcut-keys) 
- [Ulatus paani kiirklahvid](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Klaviatuuri abil

Järgmises tabelis kirjeldatakse klahvid, mille abil saate liikuda StorSimple hetktõmmise halduri kasutajaliides. 

| Navigeerimise klahvi  | Toiming  |
|:----------------|:--------| 
| Nool | Liikumine järgmisele üksusele menüü või paani vertikaalselt siis allanooleklahvi abil. |
| Sisestage | Vajutage sisestusklahvi (Enter) toimingu lõpuleviimine ja siis jätkake järgmise juhisega. Näiteks võite vajutada sisestusklahvi valige **järgmise**, **OK**või **Loo**ja asuge järgmise juhise juurde viisardi.|
| Paoklahv (ESC) | Menüü sulgemine või, et tühistada ja sulgege lehe vajutage paoklahvi (ESC).|
| F1 | Spikriteema hetkel aktiivse akna kuvamiseks vajutage klahvi F1.|
| F5 | Sõlm värskendamiseks klahvi F5. |
| KLAHVI F6 | **Otsingutulemite** paan paanilt **ulatus** liikumiseks klahvi F6.|
| F10 | Menüü avamiseks klahvi F10. |
| Nool vasakule | Liikumine Eelmisele suvandile horisontaalselt menüü riba suvand vasaknooleklahvi abil. Eelmisele üksusele teisaldamisel menüü kuvatakse menüü eelmise üksuse toimingu (või konteksti). |
| Paremnoolt | Kasutage paremnoolt teisaldada horisontaalselt ühe menüüsuvandi riba järgmisele. Järgmisele üksusele teisaldamisel menüü kuvatakse uue üksuse menüü action (või konteksti).
| Tabeldusklahvi | Liikumine järgmisele paanile konsooli või lehe järgmise valiku või tekst väljale tabeldusklahvi (TAB) abil. |
| NOOLEKLAHVI | Eelmisele üksusele menüüs või paani vertikaalselt teisaldamine värskendatud NOOLEKLAHVI abil. |

### <a name="menu-bar-shortcut-keys"></a>Menüü kiirklahvid

Järgmises tabelis kirjeldatakse menüüriba kiirklahvikombinatsioone. Kui vajutate kiirklahve ja avaneb menüü, saate kasutada menüü kiirklahvid (allakriipsutatud võtmed menüü). Menüüriba kohta lisateabe saamiseks minge [menüüriba](#menu-bar).

| Otsetee | Tulemus                    | Kiirklahv menüü | Tulemus          |
|:---------|:--------------------------|:------------------|:----------------|
| ALT + F    | Avaneb menüü **fail** .  | N | Avab uue konsooli eksemplari.   |
|          |                           | O | Avaneb leht **Haldustööriistad** . |
|          |                           | S | Salvestab StorSimple hetktõmmise Manager konsooli.|
|          |                           | A | Avaneb leht **Salvesta nimega** . |
|          |                           | M | Avaneb leht **lisandmooduli lisamine või eemaldamine** .|
|          |                           | P | Avaneb leht **Suvandid** . |
|          |                           | H | Avaneb võrguspikker.|
| ALT + A    | Avaneb menüü **toiming** .| Ma | Kuva suvandit impordi lülitab sisse ja välja.|
|          |                           | W | Avab uue StorSimple hetktõmmise halduri konsooli.|
|          |                           | F | Värskendatakse StorSimple hetktõmmise Manager konsooli.|
|          |                           | L | **Loendi eksportimine** leht avatakse. 
|          |                           | H | Avaneb võrguspikker.|
| ALT + V    | Avab menüü **Vaade** .  | A | Avab lehe **Veergude lisamine või eemaldamine** . |
|          |                           | U | Avaneb leht **Vaate kohandamine** . |
| ALT + O    | Avaneb menüü **Lemmikud** . | A | Avaneb leht **Lisa lemmikute hulka** . |
|          |                           | O | Avab lehe **Korraldada lemmikud** .|
| ALT + W    | Avab **akna** menüü.| N | Avab akna teise StorSimple hetktõmmise Manager.|
|          |                           | C | Kuvab kõik avatud konsooli windows kaskaadlaadistiku laadi.|
|          |                           | T | Kuvab kõik avatud konsooli Windowsi võrgu mudelit. |
|          |                           | Ma | Korraldab ikoonid reast ekraani allosas.|
| ALT + H    | Avaneb menüü **Spikker** .  | H | Avaneb võrguspikker.|
|          |                           | T | Avatakse veebileht Microsoft TechNeti Tech Center.|
|          |                           | A | Avaneb leht **Microsofti halduskonsooli** . |
 
### <a name="scope-pane-shortcut-keys"></a>Ulatus paani kiirklahvid

Järgmistes tabelites on otsetee klahvikombinatsioone iga sõlme **ulatus** paanil. 

- [Seadmete sõlm kiirklahvid](#devices-node-shortcut-keys)
- [Mahud sõlm kiirklahvid](#volumes-node-shortcut-keys)
- [Helitugevuse rühmade sõlm kiirklahvid](#volume-groups-node-shortcut-keys)
- [Varukoopia poliitikate sõlm kiirklahvid](#backup-policies-node-shortcut-keys)
- [Varukoopia kataloogi sõlm kiirklahvid](#backup-catalog-node-shortcut-keys)
- [Töö sõlm kiirklahvid](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Seadmete sõlm kiirklahvid

| Menüü otsetee | Tulemus                               |
|:--------------|:-------------------------------------|
| C             | Avaneb leht **seadme konfigureerimine** . |
| D             | Värskendab loendit seadmed ja seadme üksikasjad.|
| V             | Avab menüü **Vaade** . |
| W             | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **üksikasjad** . |
| F             | Värskendatakse StorSimple hetktõmmise Manager konsooli. |
| L             | **Loendi eksportimine** leht avatakse. 
| H             | Avaneb võrguspikker.|
 

#### <a name="volumes-node-shortcut-keys"></a>Mahud sõlm kiirklahvid

| Menüü otsetee   | Tulemus                              |
|:----------------|:------------------------------------|
| V               | Uueneb sealne mahtu.        |
| V (vajutage kaks korda) | Avab menüü **Vaade** .            |
| W               | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **maht** .|
| F               | Värskendatakse StorSimple hetktõmmise Manager konsooli.|
| L               | **Loendi eksportimine** leht avatakse. 
| H               | Avaneb võrguspikker.|
 
#### <a name="volume-groups-node-shortcut-keys"></a>Helitugevuse rühmade sõlm kiirklahvid

| Menüü otsetee   | Tulemus                              |
|:----------------|:------------------------------------|
| G               | Avaneb leht **Helitugevuse rühma loomine** . |
| V               | Avab menüü **Vaade** . |
| W               | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **Helitugevuse rühmad** .|
| F               | Uuendab StorSimple hetktõmmise Manager konsooli. |
| L               | **Loendi eksportimine** leht avatakse. |
| H               | Avaneb võrguspikker.|

#### <a name="backup-policies-node-shortcut-keys"></a>Varukoopia poliitikate sõlm kiirklahvid

| Menüü otsetee   | Tulemus                              |
|:----------------|:------------------------------------|
| B               | Avab lehe **loomine poliitika** . |
| V               | Avab menüü **Vaade** .            |
| W               | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **Helitugevuse rühmad** .|
| F               | Värskendatakse StorSimple hetktõmmise Manager konsooli.|
| L               | **Loendi eksportimine **leht avatakse. 
| H               | Avaneb võrguspikker.|
 
#### <a name="backup-catalog-node-shortcut-keys"></a>Varukoopia kataloogi sõlm kiirklahvid

| Menüü otsetee   | Tulemus                              |
|:----------------|:------------------------------------|
| W               | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **Helitugevuse rühmad** . |
| F               | Värskendatakse StorSimple hetktõmmise Manager konsooli. |
| H               | Avaneb võrguspikker.|
 
#### <a name="jobs-node-shortcut-keys"></a>Töö sõlm kiirklahvid

| Menüü otsetee   | Tulemus                              |
|:----------------|:------------------------------------|
| V               | Avab menüü **Vaade** .            |
| W               | Avab uue StorSimple hetktõmmise halduri konsooli värskendustest sõlme **tööde haldamine** .|
| F               | Värskendatakse StorSimple hetktõmmise Manager konsooli.|
| L               | **Loendi eksportimine** leht avatakse.     |
| H               | Avaneb Võrguspikker                   |
 
## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise halduri ühenduse loomiseks ja haldamiseks seadmete kasutamise](storsimple-snapshot-manager-manage-devices.md)kohta.
