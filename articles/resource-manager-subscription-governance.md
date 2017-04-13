<properties
   pageTitle="Head tavad teisaldada Azure'i suurettevõtete jaoks | Microsoft Azure'i"
   description="Kirjeldatakse raami suurettevõtete saate tagada turvaline ja mõistliku keskkond."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure'i enterprise tellingud - asemel tellimuse juhtimine

Suurettevõtete on järjest vastuvõtmise avaliku cloud nende kiirus ja paindlikkust. Nad kasutavad funktsiooni cloud tugevuste tulu või ettevõtte ressursid optimeerimine. Microsoft Azure'i pakub hulgaliselt teenuseid, et suurettevõtete saab koostada nagu koosteüksuste lahendamine laias valikus töökoormus ja rakendused. 

Kuid teab, kust algab sageli on raske. Olles otsustanud kasutada Azure'i, mõned sagedamini küsimused:

- "Kuidas täita meie juriidilised nõuded andmete suveräänsuse teatud riikides?"
- "Kuidas tagada, et keegi ei muuda tahtmatult kriitilise süsteemi?"
- "Kuidas kindlaks teha, mida iga ressursi toetab nii, et saate arvele see ja selle tagasi täpselt arve?"

Võimalus pole kaitsereelingud koos tühja tellimuse on heidutav. Selle tühja ruumi võivad takistada Azure'i liikuda.

Selles artiklis antakse alguspunkti tehnilise ärikasutajate juhtimise vajadust ja selle agility vajadust saldo. See toob mõiste enterprise tellingud, mis juhendab ettevõtted rakendamise ja nende Azure tellimuste haldamine. 

## <a name="need-for-governance"></a>Juhtimise vajadust

Liikudes Azure, peab teil aadress juhtimise varakult pilveteenuste ettevõttes eduka kasutamise tagamisel teema. Kahjuks aega ja luua täielik juhtimise süsteem bürokraatia tähendab, et mõned business rühmad minge otse tarnijatele enterprise kaasamata selle. Seda moodust saab jätke ettevõtte avatud nõrkade kohtade ressursside on õigesti juhitud. Avalik pilv - agility, paindlikkust ja tarbimine-põhiste hinnad - omaduste oluliste business rühmal on vaja kiiresti nõudmistele klientide (nii sise). Kuid ettevõtte vajab andmete ja süsteemide tõhusaks kaitsmiseks.

Tegelik kestus, kasutatakse tellingud luua struktuuri alusel. Tellingute juhendab üldine plaan ning tuuakse Ankurpunktid püsivam süsteemidele külge. Mõni ettevõte tellingud on sama: Azure'i võimalusi, mis pakuvad tugineb avaliku cloud services keskkonna ja ankrud struktuuri ja paindlike juhtelementide kogum. Pakub ehitajad (see ja business rühmad) fondi luua ja lisada uusi teenuseid.

Tellingute põhjal oleme kogunud palju kokkulepete klientidega erinevas suuruses tavad. Need kliendid ulatuvad väikeettevõtetele Fortune 500 ettevõtetele ja sõltumatu tarkvara müüjad, kes on migreerimine ja lahendusi pilves pilves lahendusi. Ettevõtte tellingud on "ehitatud" paindlik toetada nii traditsiooniline IT töökoormus dünaamilised töökoormus; näiteks loomise tarkvara-kui-a-service (SaaS) rakenduste arendajatele põhjal Azure võimalusi.

Ettevõtte tellingud on mõeldud igasuguse Azure'is iga uude tellimusse. Selle abil tagada töökoormus vastavad minimaalsed juhtimise ettevõtte ilma kiiresti oma eesmärkide business rühmad ja arendajate takistades.

> [AZURE.IMPORTANT] Juhtimise on oluline Azure'i edu. Selles artiklis tehniline rakendamine ettevõtte tellingud on eesmärgid, kuid ainult puudutab laiem protsess ja komponentide vahelisi seoseid. Poliitika juhtimise liigub sujuvalt ülevalt alla ja määrab äri soovib saavutada. Loomulikult juhtimismudeli loomine Azure sisaldab see esindajad, kuid olulisem pole tugev esituse ettevõtete rühma juhid, ja turvalisus ja riskijuhtimise. Lõpuks, on ka ettevõtte tellingud business turvalisus hõlbustamiseks ettevõtte ülesanne ja eesmärgid.

Järgmisel pildil kirjeldatakse tellingute komponendid. Fondi tugineb ühtlase kavandamine osakondade, kontod ja tellimused. Sammaste koosnevad ressursihaldur poliitikate ja keeruka nime standarditele. Ülejäänud tellingud pärineb core Azure võimaluste ja selle luba turvaline ja mõistliku keskkond funktsioonid.

![tellingud komponendid](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure'i kasvanud kiiresti alates selle 2008. See nõutav Microsofti tehnika meeskondadel nende lähenemine haldamine ja teenuste juurutamine mõelda. Azure'i ressursihaldur mudeli võeti kasutusele 2014 ja asendab klassikaline juurutamise mudel. Ressursihaldur võimaldab asutustel hõlpsam juurutada, korraldada ja kontrollida Azure ressursse. Ressursihaldur sisaldab parallelization ressursid keerukas, omavahel seotud lahendusi kiirem juurutamiseks loomisel. See hõlmab ka Varundustöö juurdepääsu reguleerimine ja võimalus sildi ressursid metaandmed. Microsoft soovitab teil kõik ressursid ressursihaldur mudeli kaudu. Ressursihaldur mudeli kavandatud enterprise tellingud.

## <a name="define-your-hierarchy"></a>Määratlege oma hierarhia

Fondi tellingute on Azure Enterprise registreerimine (ja ettevõtteportaali). Ettevõtte registreerimine määratleb kujundit ja ettevõtte Azure teenuseid kasutada ja on core juhtimise struktuuri. Enterprise'i leping, kliendid saavad täpsemaks jagada keskkonna osakondade, kontod ja lõpuks tellimused. Kui kõik ressursid on esitatud põhiühik on Azure tellimuse. See määratleb ka mitu piirangud Azure'is, näiteks arv südamikud, ressursid jne.

![hierarhia](./media/resource-manager-subscription-governance/agreement.png)

Iga ettevõte on erinevad ja eelmise pildil hierarhia võimaldab märkimisväärset paindlikkust Azure'i korraldamine ettevõttes. Enne rakendamisel selles dokumendis sisalduvad juhised, peaksite oma hierarhia mudel ja mõista mõju arveldus, ressursside juurdepääsu ja keerukuse.

Kolm levinud mustrite Azure'i registreerimiste on:

- **Otstarbekas** muster

    ![otstarbekas](./media/resource-manager-subscription-governance/functional.png)

- Mustri **üksus** 

    ![Business](./media/resource-manager-subscription-governance/business.png)

- **Geograafilised** muster

    ![geograafilised](./media/resource-manager-subscription-governance/geographic.png)

Laiendada haldamine ettevõtte nõudeid tellimuse tasemel tellimuse tellingute rakendamine

## <a name="naming-standards"></a>Nime andmise standardid

Esimese samba tellingute on nimetamine standarditele. Hästi kujundatud nime standardid võimaldavad teil tuvastada portaalis arve ja skriptide ressursid. Tõenäoliselt, olete juba failinimede standardid kohapealne taristu. Azure'i lisamisel keskkonna tuleks nime standardeid laiendada oma Azure ressursse. Nime andmise standard hõlbustada tõhusamalt hallata keskkond, mis on kõigil.

> [AZURE.TIP]
>
> - Läbi vaadata ja vastu võtta, kus võimalik [mustrite ja tavade juhised](./guidance/guidance-naming-conventions.md). Juhised, mis aitab teil otsustada tähendusega nime standardne.
> - Kasutage camelCasing nimed ressursid (nt myResourceGroup ja vnetNetworkName). Märkus: On teatud ressursse, näiteks salvestusruumi kontod, kus on ainus võimalus kasutada väiketähti (ja muid erimärke).
> - Jõusta nime standardid kaaluge Azure'i ressursihaldur poliitikate (järgmises jaotises kirjeldatud).
 
## <a name="policies-and-auditing"></a>Poliitika ja auditeerimine

Tellingute sambaga hõlmab [Azure'i ressursihaldur poliitikate](resource-manager-policy.md) ja [auditeerimine tegevuste Logi](resource-group-audit.md)loomine. Ressursihaldur poliitikate teile võimalust hallata risk Azure. Saate määratleda poliitikad, mis tagavad andmete suveräänsuse piiramine, jõustamine või teatud toimingute audit. 

- Poliitika on vaikimisi **Luba** süsteem. Saate reguleerida toimingute määratlemine ja ressursse, mis keelamiseks või toimingute ressursid auditi poliitika määramine.
- Poliitika on kirjeldatud poliitika määratlused poliitika määratlus keeles (kui-siis tingimuste) alusel.
- Saate luua poliitikat koos JSON (JavaScripti Objektiesituse) vormindatud failid. Pärast poliitikast, saate selle määrata teatud ulatus: tellimuse, ressursirühma. või ressurss.

Poliitika on mitu toiminguid, mida teie stsenaariumid kohandatud lähenemisviisi luba. Toimingud on:

-   **Keela**: blokeerib ressursi taotlus
-   **Auditilogi**: võimaldab kutse, kuid lisab rea tegevuse log (mida saab kasutada teatiste või tegevusraamatud käivitamine)
-   **Lisa**: määratud teave lisatakse ressurss. Näiteks kui ressursi pole "CostCenter" sildi, lisada koos vaikeväärtuse silti.

### <a name="common-uses-of-resource-manager-policies"></a>Ressursihaldur poliitika kasutusalad

Azure'i ressursihaldur poliitika on võimas tööriista Azure tööriistakomplekt. Need võimaldavad teil vältida loomise, maksumus keskuse kaudu sildistamine ressursid tuvastamiseks ja tagamaks, et meiliserverit nõuded on täidetud. Kui poliitika on koos sisseehitatud auditeerimisfunktsioonide, saate mood keerukate ja paindlikus lahendusi. Poliitika luba osutada juhtelementide töökoormus "Traditsiooniline see" ja "Agile" töökoormus; näiteks kliendi rakenduste väljatöötamise. Kõige levinum mustreid näeme poliitika on:

- **Andmete/Geo-vastavuse suveräänsuse** - Azure pakub piirkondade kogu maailmas. Suurettevõtete sageli soovite reguleerida, kus luuakse ressursid (kas andmed suveräänsuse tagamiseks või lihtsalt, et tagada ressursid on loodud end klientide ressursside lähedal).
- **Kulude haldamine** – An Azure'i tellimus võib sisaldada ressursid paljude tüübid ja skaala. Ettevõtted sageli soovin, et tagada, et standard tellimuste Vältige liiga suur ressursse, mis võib maksta sadu dollarit kuu või rohkem.
- **Vaikimisi halduse nõutud sildid** - sildid vajavad on üks kõige levinum ja väga soovitud funktsioone. Suurettevõtete Azure'i ressursihaldur reeglite abil on võimalik tagada, et ressursi on õigesti sildistatud. Kõige levinum sildid on: osakonna, ressursside omanik ja keskkonna tüüp (nt - tootmine, test, arengu)

**Näited**

"Traditsiooniline see" tellimuse ärivaldkonna rakenduste

-   Jõusta osakonna ja omaniku sildid kõik ressursid
-   Piira ressursside loomine alale Põhja-Ameerika
-   Võimalus luua G-seeria VMs ja Hdinsightiga kogumite piiritlemine

"Kiire" keskkonna üksuse business cloud rakenduste loomine

- Andmete suveräänsuse nõuete täitmiseks luba ressursside loomine ainult teatud piirkonnas.
- Jõusta keskkonna sildi kõik ressursid. Kui ressurss on loodud ilma sildi, lisa selle **keskkonnas: Tundmatu** silti, et ressurss.
- Auditilogi kui ressursid on loodud väljaspool Põhja-Ameerikas, kuid ei takista.
- Auditilogi kõrge kuluressursid loomisel.

> [AZURE.TIP]
>
> Kõige levinum ressursihaldur poliitika ettevõtted kasutamine juhtelemendile *kus* ressursid saab luua ja *millist* tüüpi ressursid saab luua. Lisaks juhtelementide kohta, *kus* ja *millist*, paljude ettevõtete tagada ressursid on sobiv metaandmete arve tagasi tarbeks poliitikate abil. Soovitame tellimuse tasemeks poliitika rakendamist:
>
> - Andmete/Geo-vastavuse suveräänsete
> - Kulude haldus
> - Nõutud sildid (otsustades vajadust, nt BillTo rakenduse omanik)
>
> Saate rakendada täiendavate poliitikate madalama taseme ulatuses.
 
### <a name="audit---what-happened"></a>Auditi - mis on juhtunud?

Vaadata, kuidas teie keskkond, mis toimib, peate kasutaja tegevuste audit. Ressursi enamiku Azure'is luua diagnostikalogid, mida saate analüüsida log tööriist või Azure Operations Management Suite. Võite koguda logid mitmest tellimused esitada mõne osakonna või ettevõtte vaade. Auditilogi kirjed on diagnostika tööriist nii oluline süsteem päästik sündmuste Azure keskkonnas.

Logid: ressursihaldur juurutuste võimaldavad määrata **tehtavad toimingud** , mida tegite eelnevalt koht ja kes neid liita. Logid saab koguda ja kokkuvõtliku tööriistadega nagu Log Analytics.

## <a name="resource-tags"></a>Ressursi Sildid

Oma asutuse kasutajate lisamine ressursid tellimus, muutub järjest olulised ressursid seostada vastav osakonna, klientide ja keskkonnas. [Siltide](resource-group-using-tags.md)abil saate manustada metaandmete. Siltide abil saate sisestada teavet ressursi või omanik. Sildid, mis võimaldavad teil mitte ainult liitmine ja ressursid mitmel viisil rühmitada, kuid tagasinõude eesmärgil andmete kasutamiseks. Saate sildistada ressursse kuni 15 võti: väärtuse paarideks. 

Ressursi sildid on paindlik ja tuleks lisada kõige rohkem ressursse. Levinud ressursi siltide näited:

- BillTo
- Osakonna (või üksus)
- Keskkonna (, esitusala kaevandustööstustest)
- Esimese taseme (Web taseme, rakenduse taseme)
- Rakenduse omanik
- ProjectName

![Sildid](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Veel näiteid silte, vt [soovitatav Nimepaneku tavad Azure ressursid](./guidance/guidance-naming-conventions.md).

> [AZURE.TIP]
>
> Suhtlussiltide strateegia, mille abil tuvastatakse teie tellimustes metaandmed, mida on vaja business finance turvalisus, riskijuhtimise ja haldamist keskkonna loomine. Kaaluge poliitika, et mandaatide sildistamise.
>
> - Ressursi rühmad
> - Salvestusruumi
> - Virtuaalmasinates
> - Rakenduste teenuse keskkonnas/web serverid

## <a name="resource-group"></a>Ressursirühm 

Ressursihaldur võimaldab teil panna ressursside haldus arve või loomulikus osaleja mõtestatud rühmad. Nagu varem mainitud, on Azure kaks juurutamise mudelid. Varasemate klassikaline mudelis halduse põhiühik oli tellimus. Oli raske murda ressursside tellimus, mis viis suure hulga tellimuste loomine. Mudeliga ressursihaldur me kuvati ressursirühma kasutuselevõtt. Ressursi rühmad on ümbriste ressursse, mis on levinud elutsükli või ühiskasutusse andmiseks atribuut nt "kõik SQL-i serverid" või "A".

Ressursi rühmad ei saa sisaldas üksteise sees ja ressursid saate ainult kuuluvad ühte ressursirühma. Saate rakendada teatud toimingute ressursirühma kõik ressurssidele. Näiteks ressursirühma kustutamine eemaldab kõik ressursside ressursirühma. Tavaliselt asetate kogu rakenduse või seotud süsteemi ühte ressursirühma. Näiteks on kolm rakendus nimega Contoso veebirakenduse sisaldaks veebiserverisse, rakenduse server ja SQL server ühte ressursirühma.

> [AZURE.TIP]
>
> Kuidas saate korraldada oma ressursi rühmad võivad erineda "Traditsiooniline see" töökoormus "Dünaamilised see" töökoormus
>
> - "Traditsiooniline see" töökoormus on kõige sagedamini rühmitatud üksusi sama elutsükli, nt rakenduse. Rühmitamise rakendus võimaldab üksikute Rakendusehaldus.
> - "Dünaamilised see" töökoormus pigem keskenduda väliste kliendi-ühendusega pilv rakendusi. Ressursi rühmade peaks kajastamiseks (nt Web taseme, rakenduse taseme) juurutamise kihid ja haldus.

## <a name="role-based-access-control"></a>Rollipõhine juurdepääsu reguleerimine

Tõenäoliselt esitate endale "kes peaks olema juurdepääs ressursse?" ja "Kuidas määrata seda Accessi?" Lubamisel või lubades juurdepääsu Azure portaali ja ressursside portaalis juurdepääsu kontrollimine on väga oluline. 

Kui algselt ilmus Azure, access juhtelementide tellimuse olnud: administraator või koostöö administraator. Juurdepääs tellimuse klassikaline mudeli vaikimisi juurdepääs kõigile ressurssidele portaalis. Kohandatud juhtelemendi puudumine viinud leviku tellimused on Azure registreerimine ette järgu mõistliku juurdepääsu reguleerimine.

Selle leviku tellimused pole enam vaja. Rollipõhine juurdepääsu reguleerimine, saate määrata kasutajatele standard rollid (nt "lugeja" ja "kirjutaja" levinumat rollid). Samuti saate määratleda kohandatud rollid.

> [AZURE.TIP]
>  
> - Teie ettevõtte identiteedi poe (kõige sagedamini Active Directory) ühenduse Azure Active Directory AD-ühendus tööriista abil.
> - Juhtelemendi abil hallatavad identiteedi tellimuse administraator/Co-administraator. Uue tellimuse omanik administraator/Co-admin **ei** määrata. Selle asemel kasutage RBAC rollid **omaniku** õigused rühmale või üksikute.
> - Azure'i kasutajat lisada Active Directory rühma (nt rakenduse X omanikud). Sünkroonitud rühma abil saate esitada rühma liikmete haldamine taotlus ressursirühma vajalikke õigusi.
> - Järgige andmine ei tööta oodatud nõutav **vähimate** õiguste põhimõte. Näiteks:
>     - Juurutamise rühma: Rühm, mis on ainult võimalus juurutada ressursid.
>     - Virtuaalse masina haldamine: Rühma, mis on võimalik taaskäivitamiseks VMs (toimingud)

## <a name="azure-resource-locks"></a>Azure'i ressursi lukud

Ettevõtte core services lisatakse see tellimus, muutub järjest olulised tagamaks, et need teenused on saadaval business katkemise vältimiseks. [Ressursi lukud](resource-group-lock-resources.md) võimaldavad piirata kõrge väärtusega ressursid, kus muutmise või kustutamise oleks olulist mõju teie rakendused või pilvetaristu toiminguid. Saate rakendada lukud tellimus, ressursirühm või ressurss. Tavaliselt rakendate lukud põhilisi ressursse, nt virtuaalne võrkude, lüüside ja salvestusruumi kontod. 

Ressursi lukud toeta kahest väärtusest: CanNotDelete ja kirjutuskaitstud. CanNotDelete tähendab, et (vajalikke õigusi) kasutajad saavad endiselt lugeda või ressursi muuta, kuid ei saa kustutada. Kirjutuskaitstud tähendab, et autoriseeritud kasutajad ei saa kustutada ega muuta ressursi.

Saate luua või kustutada halduse lukud, peab olema juurdepääs `Microsoft.Authorization/*` või `Microsoft.Authorization/locks/*` toimingud.
Sisseehitatud rolle, antakse ainult omanik ja kasutajale Accessi administraator need toimingud.

> [AZURE.TIP] Lukud peavad olema kaitstud Core võrgu suvandid. Lüüsi juhuslikku kustutamist, VPN saidilt oleks katastroofilised Azure märkimiseks. Azure'i ei luba teil virtuaalse võrk, mis kasutab kustutada, kuid veel piirangud on kasulik ettevaatuse. Poliitika on ka vastav kontrolli säilitamine. Soovitame teil on **CanNotDelete** Lukusta poliitikat kasutatavate.
>
> - Virtuaalse võrgu: CanNotDelete
> - Võrgu turberühma: CanNotDelete
> - Poliitika: CanNotDelete

## <a name="core-networking-resources"></a>Peamised võrgu ressursid

Ressurssidele saab sisemiste (ettevõtte võrgustikus) või väliste (internet) kaudu. See on lihtne, Pange tahtmatult ressursid vale kohapeal ja potentsiaalselt avama pahatahtlik juurdepääsu oma asutuse kasutajate jaoks. Kohapealne seadmed, sarnaselt lisama suurettevõtete kontrolli selleks, et tagada, et Azure'i kasutajate õigeid otsuseid. Tellimuse juhtimist, selgitame core ressursse, mis pakuvad juurdepääsu juhtimise. Core ressursside hulka kuuluvad:

- **Virtuaalne võrgud** on container objektide jaoks alamvõrku. Kuigi mitte tingimata vajalik, seda kasutatakse sageli sisemise ettevõtte ressurssidega rakenduste loomisel.
- **Võrgu turberühmad** sarnanevad tulemüüri ja sisestada reeglid kuidas ressursi "rääkida" võrgu kaudu. Varundustöö juhtida, kuidas need / kui alamvõrgu (või virtuaalse masina) saate luua ühenduse Interneti-ühendus või muude alamvõrku virtuaalse samasse võrku.

![Core võrgunduse](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Saate luua virtuaalse võrgu väline suunatud teenustest ja sisemise suunatud töökoormus. Seda moodust vähendab tahtmatult paigutamise virtuaalmasinates, mis on mõeldud sisemise töökoormus välise suunatud ruum võimalust.
> - Selle konfiguratsiooni on võrgu turberühmad. Vähemalt, Blokeeri juurdepääs Interneti virtuaalse sise võrkudes ja ettevõtte võrguga juurdepääsu takistada virtuaalse väliseid võrgustikke.

### <a name="automation"></a>Automatiseerimine

Ressursside haldamine ükshaaval on nii aeganõudev ja potentsiaalselt vigu teatud toimingute jaoks. Azure'i pakub mitmesuguste automaatika võimaluste Azure'i automaatika, loogika rakendused ja Azure funktsioonid. [Azure'i automaatika](./automation/automation-intro.md) abil luua ja määratleda tegevusraamatud käsitlema ressursside tavalised toimingud. Saate luua tegevusraamatud PowerShelli koodi redaktor või graafilise redaktori abil. Saate koostada keerukate mitmeetapiline töövood. Azure'i automaatika kasutatakse enamasti levinud toiminguid nagu sulgemise kasutamata ressursid või ressursside loomine vastuseks kindla päästik inimsekkumist vajamata käsitlema.

> [AZURE.TIP]
>
> - Looge konto Azure automatiseerimine ja vaadake üle saadaolevad tegevusraamatud (nii graafiline ja command line) [Käitusjuhendi Galerii](./automation/automation-runbook-gallery.md)saadaval.
> - Impordi- ja olulisi tegevusraamatud kasutamiseks kohandada.
>
> Stsenaarium on võimalus käivitada virtuaalmasinates ajakava. On näide tegevusraamatud, mis on saadaval galeriis nii toime selle stsenaariumi ja õpetada, kuidas laiendada.


## <a name="azure-security-center"></a>Azure'i Security Center

Võib-olla üks suurim blokaatorid Cloud kasutuselevõtu on probleeme üle turvalisus. See risk haldurid ja turvalisus osakondade tuleb tagada turvaline Azure ressursse. 

[Azure'i turbekeskus](./security-center/security-center-intro.md) annab keskse vaate turvalisus oleku ressursside tellimused, ja soovitused, mis aitavad vältida rikutud ressursid. Selle saate lubada täpsema poliitikate (nt rakendamine poliitikate teatud ressursi rühmadele, mis võimaldavad kohandada oma asendi ohtu, kui need on mõeldud enterprise). Lõpuks Azure'i turbekeskus on avatud platvormi, mis võimaldab Microsofti partnerite ja sõltumatu tarkvara tarnijate ühendatakse Azure'i turbekeskus täiustamiseks võimalusi tarkvara loomiseks. 

> [AZURE.TIP]
>
> Azure'i turbekeskus on vaikimisi iga tellimus. Siiski tuleb lubada andmete kogumine virtuaalmasinates lubamiseks Azure turbekeskus installida oma agent ja alustage andmete kogumine.
>
> ![andmete kogumine](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Järgmised sammud

- Nüüd, kui olete tellimuse juhtimise õppinud, on aeg praktikas soovituste kuvamiseks. Teemast [Azure tellimuse juhtimise rakendamise näited](resource-manager-subscription-examples.md).

*Selles teemas kaasa [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
