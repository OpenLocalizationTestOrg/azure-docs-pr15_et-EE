<properties
   pageTitle="Data Service turuplatsil loomise juhend | Microsoft Azure'i"
   description="Üksikasjalikud juhised, kuidas luua, kinnitamine ja juurutada andmeid teenuse osta Azure'i turuplatsilt."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Sõlmed skeemi vastendamine olemasoleva web teenuse kaudu alla CSDL OData mõistmine

>[AZURE.IMPORTANT] **Sel ajal me ei ole enam kasutuselevõtt mis tahes uute andmete teenuse tootjad. Uue dataservices ei saada heaks kirjet.** Kui teil on SaaS ärirakendusi soovite avaldada AppSource leiate lisateavet [siin](https://appsource.microsoft.com/partners). Kui teil on mõni IaaS rakendusi või arendaja teenuse soovite avaldatava Azure'i turuplatsi võite leida rohkem teavet [siin](https://azure.microsoft.com/marketplace/programs/certified/).

Selle dokumendi aitavad täpsustada sõlm struktuuri ja alla CSDL vastendamise soovitud OData Protocol (protokoll). See on oluline märkida, et sõlm struktuuri on hästi loodud XML-i. Nii juurkausta, ema ja lapse skeemi on rakendatav vastendamist OData kujundamisel.

## <a name="ignored-elements"></a>Ignoreeritud elemendid
Järgnevalt on kõrge taseme alla CSDL elemente (XML-i sõlmed), mida kavatsete kasutada Azure'i turuplatsi taustväärtus veebiteenuse metaandmete importimine. Ta saab Esita, kuid ignoreeritakse.

| Elemendi | Ulatus |
|----|----|
| Elemendi abil | Sõlme, sub sõlmed ja kõik atribuudid |
| Dokumentatsiooni Element | Sõlme, sub sõlmed ja kõik atribuudid |
| ComplexType | Sõlme, sub sõlmed ja kõik atribuudid |
| Seos-Element | Sõlme, sub sõlmed ja kõik atribuudid |
| Laiendatud atribuut | Sõlme, sub sõlmed ja kõik atribuudid |
| EntityContainer | Ignoreeritakse ainult järgmisi atribuute: *laiendab* ja *AssociationSet* |
| Skeemi | Ignoreeritakse ainult järgmisi atribuute: *Namespace* |
| FunctionImport | Ignoreeritakse ainult järgmisi atribuute: *režiimi* (eeldatakse vaikeväärtust, milleks on ln) |
| EntityType | Ainult järgmised sub sõlmed ignoreeritakse: *võti* ja *PropertyRef* |

Järgmist kirjeldatakse mitmesuguste alla CSDL XML-i sõlmi üksikasjalikult muudatused (lisatud ja ignoreeritud elemendid).

## <a name="functionimport-node"></a>FunctionImport sõlm
FunctionImport sõlm tähistab üks URL (sisendpunkti), mis seab lõppkasutaja teenuse. Sõlme võimaldab, mis kirjeldab, kuidas on adresseeritud URL-i, millised parameetrid on saadaval lõppkasutaja ja kuidas need parameetrid on olemas.

Selle sõlme üksikasjade leitakse [siin] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Järgmised täiendavad atribuudid (või täiendused atribuudid) mis on esitatud FunctionImport sõlm:

**d:BaseUri** -
The URI malli ülejäänud ressursi, mis on avatud turuplatsilt. Turuplatsi kasutab malli ehitada päringute REST veebiteenuse vastu. URI Mall sisaldab kohatäidete parameetrite kujul {parameterName}, kus on parameterName parameetri nimi. Nt. apiVersion = {apiVersion}.
Parameetrid lubatud kuvada URI parameetrid või URI tee. Ilme tee puhul see on alati kohustuslik (ei saa märkida nii nullable). *Näide:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Nimi** – imporditud funktsiooni nime.  Ei tohi olla sama, mis teised on alla CSDL määratletud nimed.  Nt. Nimi = "GetModelUsageFile"

**Olemikomplekt** *(valikuline)* – kui kogumi üksuse tüüpi, tagastab funktsioon **Olemikomplekt** väärtus peab olema üksus, kuhu selle saidikogumi kuulub seadmine. Muul juhul ei tohi kasutada **Olemikomplekt** atribuuti. *Näide:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(Valikuline)* – saate määrata elementide tagastatud URI tüüp.  Kasutage seda atribuuti, kui funktsioon väärtust ei tagasta. Järgnevalt on toetatud failitüübid.

 - **Saidikogumi (<Entity type name>)**: määrab kogumi määratletud üksuse tüüpi. Nimi on EntityType sõlme atribuudi nimi. Näide on saidikogumi (WXC. HourlyResult).
 - **Raw (<mime type>)**: määrab töötlemata dokumendi/bloobimälu, mis tagastatakse kasutajale. Näide on Raw(image/jpeg) näited:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "saidikogumi (salvei. DeleteAllUsageFilesEntity) "*

**d:Paging** – saate määrata, kuidas Piipar teostab ülejäänud ressursi. Parameetrite väärtused kasutatakse sees kõverate braches, nt lehe {$page} = & itemsperpage = {$size} saadaolevad suvandid on:

- **Pole:** pole Piipar on saadaval
- **Jäta:** Piipar väljendub loogiline "jäta" ja "Võta" (üleval). Jäta rongile hüpata M elemente ja võtta tagastab järgmise N elemendid. Parameetri väärtus: $skip
- **Võtta:** Annab vastuseks järgmise N elementide võtta. Parameetri väärtus: $take
- **PageSize:** Piipar väljendub loogilise lehe ja suurus (kirjed kaarti lehel). Lehe tähistab praeguse lehe, mis tagastatakse. Parameetri väärtus: $page
- **Suurus:** suurus tähistab iga lehekülje tagastatud üksuste arv. Parameetri väärtus: $size

**d:AllowedHttpMethods** *(Valikuline)* – saate määrata, millised tegusõna teostab ülejäänud ressurssi. Lisaks piirab aktsepteeritud tegusõna määratud väärtuse.  Vaikimisi = POSTITA.  *Näide:* `d:AllowedHttpMethods="GET"` Saadaolevad suvandid on:

- **Hankimine:** tavaliselt kasutatakse andmeid
- **Postituse:** tavaliselt kasutatakse uute andmete lisamiseks
- **Sellele:** tavaliselt kasutatakse andmete värskendamine
- **Kustutamine:** kasutatud andmete kustutamine

Täiendavad tütarsõlmede (ei kuulu alla CSDL dokumentatsiooni) FunctionImport sõlm sees on:

**d:RequestBody** *(Valikuline)* – koosolekukutse sisusse kasutatakse näitamaks, et kutse eeldab, et saata asutuse. Parameetrite võivad olla antud koosolekukutse kehas. Need on esitatud kõverate sulgudes, nt {parameterName}. Järgmiste parameetrite on vastendatud kasutaja sisendist kehasse, mis on üle sisu pakkuja teenuse kaudu. RequestBody element on atribuudi nimi httpMethod. Atribuut võimaldab kahest väärtusest:

- **Postituse:** Kui HTTP POST on kasutatud
- **Hankimine:** Kui HTTP GET on kasutatud

    Näide:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** ja **d:Namespace** – selle sõlme kirjeldatakse nimeruumid, mis on määratletud XML-i, mis tagastatakse funktsiooni import (URI lõpp-punkti). XML-i, mis tagastatakse kirjutamata teenuse võivad sisaldada mis tahes sisu, mis tagastatakse eristamiseks nimeruumid arv. **Kõik need nimeruumid, kui kasutatakse d:Map või d:Match XPath päringute peate olema lisatud.** D:Namespaces sõlm sisaldab Sea/loendi d:Namespace sõlmed. Üks neist on loetletud ühe nimeruumi kasutatud kirjutamata teenuse vastus. Atribuut d:Namespace sõlm on järgmised:

-   **d:Prefix:** Eesliite nimeruumi, nagu on näha XML-i tulemite teenuse, nt f:FirstName, f:LastName, kus f on eesliide.
- **d:Uri:** Täielik URI tulemi dokumendis kasutatud nimeruumi. See tähistab eesliide on otsustanud käitusajal soovitud väärtus.

**d:ErrorHandling** *(Valikuline)* – see sõlm sisaldab viga teisaldamise tingimusi. Kõik tingimused on valideeritud sisu pakkuja teenuse tagastatav tulemus. Kui tingimus vastab pakutud HTTP tõrkekoodi tagastatakse lõppkasutaja tõrketeade.

**d:ErrorHandling** *(Valikuline)* ja **d:Condition** *(valikuline)* - sõlm tingimus on üks tingimus, mis on märgitud sisu pakkuja teenuse tulemiga. Järgnevalt on **nõutavad** atribuudid.

- **d:Match:** XPath avaldis, mis kinnitab, kas sõlm/väärtus on olemas sisu pakkuja väljund XML-i. XPath suhtes väljund ja peaks tagastama väärtuse true, kui tingimus on false või match teisiti.
- **d:HttpStatusCode:** HTTP olekukoodi, mis tuleks tagastada, turuplatsi juhul tingimus vastab. Turuplatsi signalizes tõrgete kasutajale HTTP olekukoodi kaudu. Loendi HTTP olek on saadaval veebisaidil http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Tõrketeade, mis tagastatakse – koos HTTP olekukoodi – lõppkasutaja. See peaks olema sõbralik tõrketeade, mis ei sisalda mis tahes saladusi.

**d:title** *(Valikuline)* – võimaldab, mis kirjeldab funktsiooni pealkiri. Pealkirja väärtus on pärit

- Valikuline MAPI atribuut (xpath), mis määrab üles leida pealkirja tagastatud tugiteenuse taotluse vastuseks.
- - Või - väärtusena sõlme määratud pealkirja.

**d:Rights** *(Valikuline)* – seotud funktsiooni (nt autoriõiguse) õigused. Õiguste väärtus on pärit:

- Valikuline MAPI atribuut (xpath) mis määrab asukoht tagastatud tugiteenuse taotluse vastuseks õigused.
-   - Või - väärtusena sõlme määratud õigused.

**d:Description** *(Valikuline)* - funktsiooni lühikirjeldus. Kirjeldus väärtus on pärit

- Valikuline MAPI atribuut (xpath), kus saate määrata, kust kirjeldus tagastatud tugiteenuse taotluse vastus.
- - Või -määratud väärtusena sõlme kirjeldus.

**d:EmitSelfLink** - *Vt ülaltoodud näites "FunctionImport jaoks Piipar' kaudu tagastatud andmeid"*

**d:EncodeParameterValue** - OData valikuline laiendamine

**d:QueryResourceCost** - OData valikuline laiendamine

**d:Map** - OData valikuline laiendamine

**d:Headers** - OData valikuline laiendamine

**d:Headers** - OData valikuline laiendamine

**d:Value** - OData valikuline laiendamine

**d:HttpStatusCode** - OData valikuline laiendamine

**d:ErrorMessage** - OData valikuline laiendamine

## <a name="parameter-node"></a>Parameetri sõlm

Selle sõlme tähistab üks parameeter, mis on esitatud osa URI malli / Taotle sisu, mis on määratud FunctionImport sõlme.

Väga kasulik üksikasjad dokumendi lehe kohta "Parameeter Element" sõlm on leida [siin](http://msdn.microsoft.com/library/ee473431.aspx) (vajadusel vaadata dokumentatsiooni muu versioon valimiseks kasutage rippmenüüst **Muu versioon** ). *Näide:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameetri atribuut | On nõutav | Väärtus |
|----|----|----|
| Nimi | Jah | Parameetri nimi. Tõstutundlik!  Erista BaseUri suurtähti. **Näide:**`<Property Name="IsDormant" Type="Byte" />` |
| Tüüp | Jah | Parameetri tüüp. Väärtus peab olema ka **EDMSimpleType** või keerukas tüüp, mis on mudeli piires. Lisateabe saamiseks lugege teemat "6 toetatud parameeter/atribuudi".  (Tõstutundlik! Esimene märk on suur, ülejäänud on väiketähtedes.)  Vt ka, [kontseptuaalne mudel tüüpi (alla CSDL)] [MSDNParameterLink]. **Näide:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Režiim | Ei | **Tekstivormingus**, välja või InOut sõltuvalt sellest, kas parameeter on sisestatud, väljund või sisend parameeter. (Ainult "IN" on saadaval Azure'i turuplatsi.) **Näide:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Ei | Suurim lubatud parameetri pikkus. **Näide:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Täpsus | Ei | Parameetri täpsuse. **Näide:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skaala | Ei | Parameetri skaala. **Näide:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Järgnevalt on atribuute, mis on lisatud alla CSDL määratlus.

| Parameetri atribuut | Kirjeldus |
|----|----|
| **d:regex** *(Valikuline)* | Regex lause valideerimiseks sisendväärtust parameetri jaoks kasutada. Kui sisendväärtust ei vasta lause väärtus on tagasi lükatud. See võimaldab määrata ka võimalikud väärtused, nt ^ [0 – 9] +? $ lubama ainult arve. **Näide:** ' < parameetri nimi = "nimi" režiimi = "S" tüüp = "String" d: Nullable = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "on nimi, mis ei tohi sisaldada tühikuid või -alfa mitte-ingliskeelsed märgid" d:SampleValues = "Georg|John|Thomas|James "/ >" |
| **d:Enum** *(Valikuline)* | Toru eraldatud väärtuste parameetri jaoks sobiv loend. Väärtused tüüpi peab parameetri määratletud tüübiga. Näide: "inglise|meetermõõdustik|töötlemata`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< parameetri nimi = "Kestus" tüüp = "String" režiimi = "S" Nullable = "true" d:Enum = "1 aasta|5 aastat|10 aastat "/ >" |
| **d: Nullable** *(Valikuline)* | Võimaldab määratleda, kas parameetri võib olla null. Vaikimisi on: true. Parameetrite, mis on esitatud osa tee URI malli ei tohi olla tühi. Kui atribuut on seatud väärtusele väär järgmiste parameetrite – kasutaja sisendist on alistada. **Näide:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Valikuline)* | Näidisväärtuste kliendi kasutajaliidese märkme kuvamiseks.  On võimalik lisada, kasutades Triibu eraldatud loend, st mitut väärtust "lisamine|b|c` **Example:** `< parameetri nimi = "BikeOwner" tüüp = "String" režiimi = "S" d:SampleValues = "Georg|John|Thomas|James "/ >" |

## <a name="entitytype-node"></a>EntityType sõlm

Selle sõlme tähistab ühte tüüpi, mis tagastatakse lõppkasutaja turuplatsilt. See sisaldab ka väljund, mis tagastatakse väärtused, mis tagastatakse lõppkasutaja sisu pakkuja teenuse vastendust.

Selle sõlme üksikasjad on aadressil [siin](http://msdn.microsoft.com/library/bb399206.aspx) (vajadusel vaadata dokumentatsiooni muu versioon valimiseks kasutage rippmenüüst **Muu versioon** .)

| Atribuudi nimi | On nõutav | Väärtus |
|----|----|----|
| Nimi | Jah | Tippige üksuse nimi. **Näide:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Ei | Mõne muu üksuse tüüp, mis on alus tüüpi üksuse tüüp, mis on määratletud nimi. **Näide:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Järgnevalt on atribuute, mis on lisatud alla CSDL määratlus.

**d:Map** - käivitada teenuse väljundi XPath-avaldis. Siin eeldatakse teenuse väljundi sisaldab kogumi, korduvad elemendid, nt ATOM kanali, kus on kirje sõlmed korduvad kogum. Kõik need korduv sõlmed sisaldab üks kirje. XPath seejärel määratud punkt üksikute korduva sõlm sisu pakkuja teenuse tulemus, mis hoiab üksiku kirje väärtused. Teenuse väljund näide:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath-avaldist peaks olema /foo/lint, kuna iga riba sõlm on korduv sõlm väljund ja see sisaldab tegeliku sisu, mis tagastatakse lõppkasutaja.

**Klahv** - atribuudi ignoreeritakse turuplatsilt. ÜLEJÄÄNUD vastavalt veebiteenused, üldiselt Ärge jätke primaarvõti.


## <a name="property-node"></a>Atribuut sõlm

See sõlm sisaldab ühe atribuudi kirje.

Selle sõlme üksikasjad on aadressil [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (vajadusel vaadata dokumentatsiooni muu versioon valimiseks kasutage rippmenüüst **Muu versioon** .) *Näide:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Nõutav | Väärtus |
|----|----|----|
| Nimi | Jah | Atribuudi nimi. |
| Tüüp | Jah | Atribuudi väärtus tüüp. Atribuudi väärtus tüüp peab olema ka **EDMSimpleType** või keerukas tüüp (tähistatud nõuetele täielikult vastav nimi), mis on osa mudel. Lisateabe saamiseks vt kontseptuaalne mudel tüüpi (alla CSDL). |
| Nullable | Ei | **True** (vaikeväärtus) või **väära** vastavalt kas atribuut on tühiväärtus. Märkus: Alla CSDL tähistatud [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) nimeruumi versioon, keerukas tüüp atribuut peab olema Nullable = "False". |
| DefaultValue | Ei | Atribuudi vaikeväärtus. |
|MaxLength | Ei | Atribuudi väärtus maksimumpikkus. |
| FixedLength | Ei | **Tõene** või **väär** olenevalt sellest, kas atribuudi väärtus salvestatakse fiexed pikkus stringina. |
| Täpsus | Ei | Viitab kohtade säilitamise arvulise väärtuse maksimaalne arv. |
| Skaala | Ei | Suurima arvu kümnendkohtadeni pärast koma säilitamise arvulise väärtuse. |
| Unicode | Ei | **Tõene** või **väär** sõltuvalt sellest, kas atribuudi väärtust Unicode stringina talletatud. |
| Võrdlemine | Ei | String, mis määrab kasutatava andmeallika sortimisjada. |
| ConcurrencyMode | Ei | **Ükski** (vaikeväärtus) või **püsiv**. Kui väärtus on **fikseeritud**määratud, kasutatakse atribuudi väärtus loodetav kokkulangevus kontrollid. |

Järgnevalt on toodud täiendavad atribuute, mida on lisatud alla CSDL määratlus.

**d:Map** - teenus käivitada XPathi avaldis väljund ja ekstraktib ühe atribuudi väljund. Korduva sõlm, mis on valitud EntityType sõlm XPath suhtes on määratud XPathi. Samuti on võimalik määrata absoluutne XPathi lubamiseks, sh staatiline ressursi iga väljundi sõlmed, nagu näiteks autoriõiguse märge, mis on ainult leitud üks kord algse teenuse väljund, kuid mul peaks olema igas OData väljund read. Näide: teenuse:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

XPath-avaldis oleks ./bar/baz0 saada baz0 sõlm sisu pakkuja teenus.

**d:CharMaxLength** - string tüüp, saate määrata max pikkus. Vt DataService alla CSDL näide

**d:IsPrimaryKey** – näitab, kui veerg on primaarvõtme tabel/vaates. Vt DataService alla CSDL näide.

**d:isExposed** - määratleb, kui skeemi tabelis on esitatud (üldjuhul true). Vt DataService alla CSDL näide

**d:IsView** *(Valikuline)* - true, kui see põhineb vaate asemel tabeliks.  Vt DataService alla CSDL näide

**d:Tableschema** – vt DataService alla CSDL näide

**d:ColumnName** - on vaates tabeli/veeru nimi.  Vt DataService alla CSDL näide

**d:IsReturned** - on kahendväärtus, mis määrab, kui teenuse seab selle väärtuseks kliendi.  Vt DataService alla CSDL näide

**d:IsQueryable** - on kahendväärtus, mis määrab, kui veerg saab kasutada andmebaasi päringu.   Vt DataService alla CSDL näide

**d:OrdinalPosition** - on arvväärtuste veerupaigutust ilmet, x, tabelit või vaadet, kus x on 1 tabeli veergude arv.  Vt DataService alla CSDL näide

**d:DatabaseDataType** - on andmebaasi veeru andmetüüpi, st SQL-i andmetüüp. Vt DataService alla CSDL näide

## <a name="supported-parametersproperty-types"></a>Toetatud parameetrite/atribuudi tüübid
Toetatud failitüübid parameetrite ja atribuudid on järgmised. (Tõstutundlik)

| Lihtsad tüübid | Kirjeldus |
|----|----|
| Null (Tühimärk) | Tähistab väärtuse puudumisel |
| Kahendmuutuja | Tähistab kahendarvuks hinnatud loogika matemaatilise mõiste|
| Bait | Allkirjastamata 8-bitise täisarvulise väärtuse.|
|Kuupäev ja kellaaeg| Tähistab kuupäeva ja kellaaja 12:00:00 keskööst jaanuar 1, 1753 ad siniseni väärtustega 11:59:59 P.M, detsember 9999 M.A.J. kaudu|
|Decimal | Tähistab arvväärtuste fikseeritud täpselt ja skaala. Seda tüüpi saab kirjeldada arvuline väärtus vahemikus negatiivse 10 ^ 10 positiivne 255 + 1 ^ 255 -1|
| Kahekordne | Tähistab ujuv punkti arv täpsusega 15 numbrikohta, mis võib tähistada väärtuste ligikaudse vahemikus ± 2.23e-308 kuni ± 1, 79E + 308. **Kasutage kümnendkohtade Exel ekspordi probleemi tõttu**|
| Ühe | Tähistab arv 7 numbrit täpsusega, mis võib tähistada väärtuste ligikaudse vahemikus ± 1.18e-38 kuni ± 3.40e ujuv punkti 38|
|GUID |16-baidine (128-bitine) kordumatut tunnust väärtus |
|Int16|Tähistab allkirjastatud 16-bitise täisarvulise väärtuse |
|Int32|Tähistab allkirjastatud 32-bitise täisarvulise väärtuse |
|Int64|Tähistab allkirjastatud 64-bitise täisarvulise väärtuse |
|String | Fikseeritud - või pikkus muutuv märgi andmeid kujutav |


## <a name="see-also"></a>Vt ka
- Kui olete huvitatud üldine OData vastenduse protsess ja otstarve, lugege seda artiklit [Andmete teenuse OData vastenduse](marketplace-publishing-data-service-creation-odata-mapping.md) üle vaadata määratlused, struktuurid ja juhiseid.
- Kui olete huvitatud läbivaatamise näited, lugege seda artiklit [Andmete teenuse OData vastendamise näited](marketplace-publishing-data-service-creation-odata-mapping-examples.md) näha proovi kood ja koodi süntaksit ja konteksti mõistmine.
- Naasmiseks ettenähtud tee avaldamine Azure'i turuplatsi Data Service, lugege seda artiklit [Andmete teenuse avaldamise juhend](marketplace-publishing-data-service-creation.md).
