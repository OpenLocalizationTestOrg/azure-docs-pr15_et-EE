<properties
    pageTitle="Rist-päritolu ressursside ühiskasutus (CORS) tugi | Microsoft Azure'i"
    description="Saate teada, kuidas Microsoft Azure'i salvestusruumi teenuste CORS toe lubamine."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Rist-päritolu ressursside jagamise (CORS) tugi Azure Storage teenused

Alustades versiooni 2013 – 08-15, Azure storage teenused toetavad Cross-päritolu ressursside ühiskasutus (CORS) bloobimälu, tabelist, kuhjuda ja faili teenuste jaoks. CORS on HTTP funktsioon, mis lubab juurdepääsu ressursside lisadomeeni töötab ühe domeeni veebirakenduse. Veebibrauserid rakendada turvalisus piirang nimetatakse [sama päritolu poliitika](http://www.w3.org/Security/wiki/Same_Origin_Policy) , mis takistab veebilehe helistaja API-de domeenil; CORS pakub turvaliselt lubamiseks helistada API-de lisadomeeni ühe domeeni (origin Domeen). Vaadake teemat CORS [CORS spetsifikatsioon](http://www.w3.org/TR/cors/) üksikasjad.

Saate reeglite CORS eraldi iga salvestusruumi teenuseid, helistades [Bloobimälu atribuutide seadmine](https://msdn.microsoft.com/library/hh452235.aspx)ja [Järjekorda seadmine atribuutide](https://msdn.microsoft.com/library/hh452232.aspx) [Tabeli atribuutide seadmine](https://msdn.microsoft.com/library/hh452240.aspx). Kui olete seadistanud teenuse CORS reegleid, siis õigesti autenditud taotluse vastu teenuse domeenil tehtud hinnatakse määratlemaks, kas see on lubatud teie määratud reeglite kohaselt.

>[AZURE.NOTE] Pange tähele, et CORS on pole autentimist. Mis tahes taotluse vastu salvestusruumi ressursi CORS lubamisel peavad olema proper autentimise signatuuri, või tuleb suhtes avaliku ressursi.

## <a name="understanding-cors-requests"></a>CORS taotlusi mõistmine

Origin domeeni CORS taotluse võivad olla kahes eraldi kutsed:

- Eelkontrolli taotluse, milles päringute CORS teenuse piirangud. Eelkontrolli kutse on nõutav, kui koosolekukutse viis on [lihtne viis](http://www.w3.org/TR/cors/), s.t toomine, looma või postitus.

- Tegelik taotluse vastu soovitud ressurss.

### <a name="preflight-request"></a>Eelkontrolli taotlus

Päringute eelkontrolli taotluse CORS piirangud on tuvastatud salvestusruumi teenuse konto omanik. Veebibrauseri (või muude kasutajaagent) saadab SUVANDID päringu, mis sisaldab taotluse päised, meetod ja päritolu domeeni. Salvestusruumi teenuse hindab ettenähtud toimingut CORS reeglid, mis määrata, millised origin domeenid, taotleda meetodite ja taotleda päised määrata tegeliku taotluse vastu salvestusruumi ressursi eelnevalt konfigureeritud kogumi alusel.

Kui teenus on lubatud CORS ja on CORS reegel, mis vastab eelkontrolli kutse, teenuse vastab olekukoodi 200 (OK), ning sisaldab vaja juurdepääsu reguleerimine päiste vastus.

Kui CORS teenus pole lubatud või pole CORS reegel vastab eelkontrolli kutse, vastab teenuse olek koodiga 403 (keelatud).

Kui kutse SUVANDID ei sisalda nõutud CORS päised (Origin ja juurdepääsu juhtimine-taotluse meetod päised), vastab teenuse olek koodiga 400 (vigane päring).

Pange tähele, et eelkontrolli taotluse hinnatakse (bloobimälu järjekord ja tabel) teenuse eest pole selle ressursi. Konto omanik on lubatud CORS teenuse konto atribuutide selleks, et õnnestub taotluse osana.

### <a name="actual-request"></a>Tegelik taotlus

Kui eelkontrolli kutse on aktsepteeritud ja vastus tagastatakse, kuvatakse brauseri lähtekoha tegelik taotluse vastu salvestusruumi ressursi. Brauseri keelab tegelik taotleda kohe, kui eelkontrolli taotlus on tagasi lükatud.

Tegelik taotluse käsitletakse tavaline koosolekukutse salvestusruumi teenuse vastu. Origin päise olemasolu näitab, et kutse on CORS taotlus ja teenuse kontrollib sobitamine CORS reegleid. Vaste leidmisel juurdepääsu reguleerimine päised on lisatud vastuse ja saadetakse kliendile. Kui vastet ei leitud, CORS pääsuõiguste päised ei tagastatud.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Azure Storage teenuste CORS lubamine

CORS reeglid määratakse tasemel, seega peate lubamine või keelamine CORS (bloobimälu, järjekord ja tabel) iga teenuse jaoks eraldi. Vaikimisi on CORS iga teenuse jaoks keelatud. CORS lubamiseks peate määrama versiooni 2013 – 08-15 vastav teenuse atribuutide või uuem versioon ja teenuse atribuutide lisamine CORS reeglid. Lisateavet selle kohta, kuidas lubada või keelata CORS teenuse ja kuidas CORS reeglid loonud, vt [Bloobimälu atribuutide seadmine](https://msdn.microsoft.com/library/hh452235.aspx)ja [Järjekorda seadmine atribuutide](https://msdn.microsoft.com/library/hh452232.aspx) [Tabeli atribuutide seadmine](https://msdn.microsoft.com/library/hh452240.aspx).

Siin on näide ühe CORS reegli määratud atribuutide seadmine toiming kaudu:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Iga element, mis sisaldab CORS reegel on kirjeldatud allpool.

- **AllowedOrigins**: origin domeenid, mis on lubatud teha taotluse vastu salvestusteenus CORS kaudu. Origin domeen on selle domeeni, millest pärineb kutse. Pange tähele, et päritolu peab olema täpne vaste tõstutundlik origin, mis saadab kasutaja vanus teenuse abil. Saate kasutada ka metamärki "*" kõik origin domeenide teha teel CORS lubama. Ülaltoodud näites domeenide [http://www.contoso.com](http://www.contoso.com) ja [http://www.fabrikam.com](http://www.fabrikam.com) saab teha taotlusi CORS teenuse suhtes.

- **AllowedMethods**: viise (HTTP taotluse verbide) CORS taotlus origin domeeni kasutavad. Ülaltoodud näites on lubatud ainult panna ja saada kutsed.

- **AllowedHeaders**: origin domeeni võib määratud CORS taotluse taotluse päised. Ülaltoodud näites on lubatud kõik metaandmete päised, alustades x-ms-metaandmete, x ms metaandmete sihtrühma ja x ms-metaandmete abc. Pange tähele, et metamärk "*" näitab, et kõik päise alguses määratud eesliitega on lubatud.

- **ExposedHeaders**: vastus päised, mis võivad olla vastuseks CORS taotluse saatnud ja esitatud taotluse väljaandja brauseri. Ülaltoodud näites on brauseri juhendatud esitamist mis tahes päise x-ms-metaandmete alguses.

- **MaxAgeInSeconds**: suurt aeg, et peaksite brauseri vahemälu eelkontrolli SUVANDID taotleda.

Azure storage teenused toetavad eelkinnitatud päised soovitud **AllowedHeaders** ja **ExposedHeaders** elementide jaoks täpsustades. Päiste kategooria lubamiseks saate määrata selle kategooria levinud eesliite. Näiteks, milles *x-ms-metaandmete** nagu eelkinnitatud päise luuakse reegli kõik päised, mis algavad x-ms-metaandmete vastavad.

Järgmistele piirangutele CORS reeglid rakendamiseks tehke järgmist.

- Saate määrata kuni viis CORS reeglite salvestusteenus (bloobimälu, tabeli ja järjekorda).
- Kõik CORS reeglid sätted taotluse XML-sildid, v.a maksimaalne maht ei tohi olla suurem kui 2 KB.
- Pikkus on lubatud päis, exposed päis või lubatud origin ei tohi olla maksimaalselt 256 märki.
- Lubatud päised ja exposed päised võivad olla kas:
  - Täpse päisenimi osutamise, nt **ms x metaandmete töödeldud**sõnasõnaline päised. Taotluse võib-olla kuni 64 sõnasõnaline päiste määrata.
  - Päised, kui eesliide päis on ette, nt **x-ms-metaandmete**eelkinnitatud *. Sel viisil, mis määrab eesliite abil või mis tahes päise, mis algab väärtusega antud eesliite seab. Kuni eelkinnitatud kahe päiste võib määratud taotluse.
- Meetodid (või HTTP verbide) määratud **AllowedMethods** elemendi peab vastama meetoditega ei toeta Azure salvestusteenus API-d. Toetatud meetodid on Kustuta, toomine, juht, Ühenda, postituse, suvandite ja panna.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS reeglite hindamise loogika mõistmine

Kui salvestusruumi teenuse saab taotluse eelkontrolli või tegelik, on tulemiks olete loonud teenuse kaudu soovitud atribuutide seadmine toiming CORS vastavalt taotluse. Tellimuse, kus on koosolekukutse kehas atribuutide seadmine selle toimingu määrata hinnatakse CORS reeglid.

CORS reeglid hinnatakse järgmiselt:

1. Esmalt kontrollitakse origin domeeni taotluse domeenide loendis **AllowedOrigins** elemendi jaoks. Kui loend sisaldab origin domeeni või kõik domeenid on lubatud metamärk "*", siis reeglite hindamise tulu. Kui domeeni origin pole kaasatud, siis taotlus nurjub.

2. Järgmiseks kontrollitakse taotluse meetod (või HTTP tegusõna) toodud **AllowedMethods** elemendi meetodid. Kui meetod on loendis, siis reeglite hindamise tulu; Kui ei, siis taotlus nurjub.

3. Kui taotlus vastab selle origin domeeni ja selle meetodi reegel, see reegel on valitud taotluse ja täiendavaid reegleid ei väärtustata protsess. Enne kutse õnnestub, aga mis tahes määratud taotluse päised on kontrollida loetletud **AllowedHeaders** elemendi päised. Kui saadetud päised ei vasta lubatud päised, taotlus nurjub.

Kuna reegleid töödeldakse nii, et nad viibivad koosolekukutse kehasse, head tavad on mõistlik, määrake kõige piiravad reeglid päritolu suhtes esmalt loendist nii, et need hinnatakse esmalt. Määrake reeglid, mis on vähem piiravad – näiteks saate reegli luba kõik päritolu – loendi lõpus.

### <a name="example--cors-rules-evaluation"></a>Näide – CORS reeglite hindamise

Järgmises näites on kujutatud osalise taotluse keha toimingu määramiseks CORS reeglid salvestusruumi teenuste jaoks. Vt täpsemat ehitamine taotluse [Bloobimälu atribuutide seadmine](https://msdn.microsoft.com/library/hh452235.aspx), [Järjekorra atribuutide seadmine](https://msdn.microsoft.com/library/hh452232.aspx)ja [Tabeli atribuutide seadmine](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Järgmiseks võtke arvesse järgmisi CORS taotlused.

Taotlemine||| Vastus||
---|---|---|---|---
**Meetod** |**Origin** |**Taotluse päised** |**Reegli Match** |**Tulemus**
**SELLELE** | http://www.contoso.com |x-ms-bloobimälu-sisutüüp | Esimese reegli |Edu
**HANKIMINE** | http://www.contoso.com| x-ms-bloobimälu-sisutüüp | Teise reegli |Edu
**HANKIMINE** | http://www.contoso.com| x-ms-bloobimälu-sisutüüp | Teise reegli | Tõrge

Esimese taotluse vastab esimese reegli – origin domeeni vastab lubatud päritolu, meetodit vastab lubatud meetodite ja päis vastab lubatud päised – ja nii õnnestus.

Teine taotlus ei vasta esimese reegli, kuna meetodit ei vasta lubatud meetodite. See, aga vastavad teise reegli nii, et see ei õnnestu.

Kolmas taotluse vastab teise reegli origin domeeni ja meetod nii täiendavaid reegleid ei väärtustata. Siiski ei tohi *päise x-ms-kliendi-taotlus-id* teise reegli järgi nii, et taotlus nurjub, vaatamata sellele, et kolmanda reegli semantika oleks lubatud seda õnnestub.

>[AZURE.NOTE] Kuigi selles näites on kujutatud vähem piiravad reegli enne piiratud ühe, üldiselt parim on kõige piiravad reeglid esmalt loend.

## <a name="understanding-how-the-vary-header-is-set"></a>Kuidas seatakse Vary päise mõistmine

*Vary* päis on standardse HTTP/1.1 päise koosneb kogumi taotluse päiseväljadelt nõu brauseris või kasutaja agent kriteeriumid, mida server taotluse valitud kohta. *Vary* päise kasutatakse peamiselt puhverserverid, brauserid ja CDN-ID, mille abil määrata, kuidas peaks vastuse vahemälus talletatud vahemällu. Lisateavet leiate teemast [Vary päise](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)määratlus.

Kui brauseri või mõne muu kasutajaagendi vahemälu CORS taotluse vastust, kui lubatud origin origin domeeni vahemällu. Teise domeeni küsimused sama taotluse salvestusruumi ressursi vahemälu on aktiivne, toob selle kasutajaagent vahemällu talletatud origin domeeni. Teise domeeni ei vasta vahemällu talletatud domeeni nii, et taotlus nurjub, kui õnnestub teisiti. Teatud juhtudel määrab Azure Storage Vary päise **Origin** juhendada kasutajaagendi edaspidised CORS kutse saatmiseks teenusega kui vahemällu talletatud origin erineb taotlemise domeeninimi.

Azure'i salvestusruumi määrab *Vary* päise **Origin** tegelik toomine/HEAD taotluste järgmistel juhtudel:

- Kui koosolekukutse origin vastab täpselt CORS reegliga määratud lubatud päritolu. Täpsed vasted olevat CORS reegel ei tohi sisaldada metamärke "*" märgi.

- Puudub sobitamine taotluse origin reegel, kuid CORS salvestusruumi teenus on lubatud.

Juhul kui GET-/ HEAD taotluse vastab CORS reegel, mis lubab kõigi päritolu vastus näitab, et kõik päritolu on lubatud, ja kasutajale agent vahemälu võimaldab edaspidised taotlused origin domeene vahemälu on aktiivne.

Pange tähele, et kasutades välja arvatud toomine/HEAD, salvestusruumi teenuste pole määratud Vary päis, kuna kasutaja esindajad pole vahemällu talletatud vastused järgmised võimalused.

Järgmine tabel näitab, kuidas Azure'i salvestusruumi on vastata toomine/HEAD eelnevalt mainitud juhtudel põhjal:

Taotlemine|Konto sätted ja reeglite hindamise tulemus|||Vastus|||
---|---|---|---|---|---|---|---|---
**Origin päise taotluse esitamine** | **Selle teenuse jaoks määratud CORS reeglite** | **Vastavaid reegel olemas, mis võimaldab kõik päritolu (*)** | **Kattuvad reegel olemas origin täpne vaste** | **Vastuse sisaldab Vary päise Origin määramine** | **Vastus sisaldab Accessi-juhtelemendi lubatud-Origin: "*"** | **Vastuse sisaldab Accessi-juhtelemendi esitatud-päised**
Ei|Ei|Ei|Ei|Ei|Ei|Ei
Ei|Jah|Ei|Ei|Jah|Ei|Ei
Ei|Jah|Jah|Ei|Ei|Jah|Jah
Jah|Ei|Ei|Ei|Ei|Ei|Ei
Jah|Jah|Ei|Jah|Jah|Ei|Jah
Jah|Jah|Ei|Ei|Jah|Ei|Ei
Jah|Jah|Jah|Ei|Ei|Jah|Jah


## <a name="billing-for-cors-requests"></a>Arveldus CORS taotlused

Eduka eelkontrolli taotlused on arve, kui olete lubanud CORS jaoks mis tahes teie konto jaoks teenuse salvestusruumi (helistades [Bloobimälu atribuutide seadmine](https://msdn.microsoft.com/library/hh452235.aspx), [Järjekorra atribuutide seadmine](https://msdn.microsoft.com/library/hh452232.aspx)või [Tabeli atribuutide seadmine](https://msdn.microsoft.com/library/hh452240.aspx)). Kulude minimeerimiseks kaaluge määramise **MaxAgeInSeconds** elemendi CORS reeglite suurem väärtus, et kasutaja agent vahemälu taotluse.

Kahjuks eelkontrolli kutsed ei saadetakse teile arve.

## <a name="next-steps"></a>Järgmised sammud

[Bloobimälu teenuse atribuutide seadmine](https://msdn.microsoft.com/library/hh452235.aspx)

[Atribuutide seadmine järjekord](https://msdn.microsoft.com/library/hh452232.aspx)

[Tabeli atribuutide seadmine](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C Cross-päritolu ressursside ühiskasutus määratlus](http://www.w3.org/TR/cors/)
