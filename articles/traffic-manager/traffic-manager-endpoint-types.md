<properties
    pageTitle="Liikluse haldur lõpp-punkti tüüpi | Microsoft Azure'i"
    description="Selles artiklis selgitatakse eri tüüpi lõpp-punktid, mida saab kasutada Azure liikluse haldur"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Liikluse haldur lõpp-punktid

Microsoft Azure'i liikluse haldur võimaldab teil määrata, kuidas võrguliiklust jagatakse rakenduse kasutuselevõttu erinevate andmekeskuste töötab. Saate konfigureerida nimega 'lõpp' iga rakenduse juurutamine liikluse haldur. Kui liikluse haldur saab DNS-i taotluse, see valib saadaval tagastamiseks vastuseks DNS-i lõpp-punkti. Liikluse haldur valik aluseks lõpp-punkti praeguse oleku ja liikluse marsruutimist meetod. Lisateabe saamiseks lugege teemat [Kuidas liikluse haldur töötab](traffic-manager-how-traffic-manager-works.md).

On kolme tüüpi lõpp-punkti-liikluse haldur toetab.

- **Azure'i lõpp-punktid** , mida kasutatakse Azure teenuseid.
- **Väliste lõpp-punktide** kasutatakse teenuste majutatud Azure, kas kohapeal või mõni muu majutusteenuse pakkuja.
- **Pesastatud lõpp-punktid** kasutatakse liikluse haldur profiilid paindlikumad liikluse marsruutimist skeemid suurema ja keerukamaid juurutuste vajadustele loomiseks ühendada.

Puudub piirang kohta, kuidas ühe liikluse haldur profiili ühendatakse eri tüüpi lõpp-punktid. Iga profiili võivad sisaldada mis tahes tüüpi lõpp-punkti kombinatsiooni.

Järgmistes jaotistes kirjeldatakse põhjalikumalt iga lõpp-punkti tüüp.

## <a name="azure-endpoints"></a>Azure'i lõpp-punktid

Azure'i lõpp-punktid, mida kasutatakse Azure-põhiste teenuste liikluse haldur. Azure'i ressursi järgmist tüüpi on toetatud:

- "Klassikaline" IaaS VMs ja PaaS cloud services.
- Web Apps
- PublicIPAddress ressursid (VMs saab ühendada otse või mõne Azure'i laadi koormusetasakaalustusteenuse kaudu). Funktsiooni publicIpAddress peab olema määratud liikluse haldur profiilis kasutada DNS-i nimi.

PublicIPAddress ressursse leiate Azure'i ressursihaldur ressursse. Neid ei ole klassikaline juurutamise mudeli. Seega nad on ainus toetatud rakenduses liikluse haldur Azure'i ressursihaldur kogemusi. Muud tüüpi lõpp-punkti on toetatud nii ressursihaldur ja klassikaline juurutamise mudeli kaudu.

Kui kasutate Azure lõpp-punktid, liikluse haldur tuvastab, kui "Klassikaline" IaaS VM, pilveteenuses või Web App on peatatud ja alustada. See olek kajastub lõpp-punkti olek. Üksikasjad leiate [liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md#endpoint-and-profile-status) . Kui aluseks teenus on peatatud, liikluse haldur teha lõpp-punkti seisundikontrollid või otse liikluse lõpp-punkti. Liikluse haldur pole sündmused arveldustoe peatatud eksemplari. Teenuse taaskäivitamisel arvelduse elulookirjeldused ja lõpp-punkti on õigus saada liikluse. Selle tuvastamise ei kehti PublicIpAddress lõpp-punktid.

## <a name="external-endpoints"></a>Väliste lõpp-punktid

Väliste lõpp-punktide kasutatakse väljaspool Azure'i teenuste jaoks. Näiteks teenuse majutatud kohapealse või mõne muu teenusepakkuja juures. Väliste lõpp-punktide saate kasutada eraldi või kombineerida sama liikluse haldur profiili Azure'i lõpp-punktid. Azure'i lõpp-punktid abil väliste lõpp-punktide kombineerides võimaldab erinevaid võimalusi.

- Kas aktiivne aktiivne või passiivne aktiivne Tõrkesiirde mudel, kasutage Azure'i suurema koondamise olemasoleva kohapealse rakenduse.
- Rakenduse latentsus kasutajatele kogu maailmas vähendamiseks laiendamine kohapealse taotluse täiendavad geograafiliste asukohtade Azure. Lisateavet leiate teemast [liikluse haldur "Jõudlus" liikluse marsruutimist](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Kasuta Azure'i esitada täiendav läbilaskevõime olemasoleva kohapealse rakenduse pidevalt või kohtumise lisamine rakenduses nõudmisel kühvli lahendusena "lõhkemist cloud".

Teatud juhtudel on kasulik väliste lõpp-punktide viidata Azure'i teenuste abil (vt [FAQ](#faq)näited). Sel juhul on seisundikontrollid arve Azure lõpp-punktid määra, ei määra väliste lõpp-punktid. Siiski erinevalt Azure lõpp-punktid, peatada või kustutamise aluseks oleva teenuse seisundi kontrollimine arveldamine jätkub kuni keelamine või kustutamine lõpp-punkti liikluse haldur.

## <a name="nested-endpoints"></a>Pesastatud lõpp-punktid

Pesastatud lõpp-punktid ühendada mitu liikluse haldur profiili loomine paindlik liikluse marsruutimist skeemid ja suurem, keerukate juurutuste vajadustele. Pesastatud lõpp-punktid, kus 'lapse' profiili lisatakse lõpp 'ema' profiilile. Laps ja vanem profiilid võivad sisaldada mis tahes tüüpi, sh muude pesastatud profiilid muude lõpp-punktid. Lisateabe saamiseks vt [pesastatud liikluse haldur profiilid](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps nimega lõpp-punktid

Mõned täiendavad järgmine kehtida veebirakenduste konfigureerimisel nimega lõpp-punktid liikluse haldur.

1. Ainult veebirakenduste "Standard" SKU-ga või kohal laiene kasutamiseks liikluse haldur. Katsete lisada Web Appi alumise SKU-ga, nurjub. Kasutuselevõttu SKU olemasoleva Web App, on tulemuseks liikluse halduris liikluse enam saata selle Web App.

2. Kui lõpp saab HTTP-päring, kasutab see 'host' päise taotluse määratlemiseks millist veebirakendust peaks teenuse taotluse. Host päises DNS-i nimi, mida kasutatakse algatada taotluse, näiteks "contosoapp.azurewebsites.net". Erinevate DNS-i nimi oma veebirakenduse kasutamiseks peab olema registreeritud DNS-i nimi rakenduse kohandatud domeeni nime. Veebirakenduse endpoint lisamisel nimega Azure lõpp-punkti automaatselt registreeritud liikluse haldur profiili DNS-i nimi rakenduse. Selle registreerimise eemaldatakse lõpp-punkti kustutatakse automaatselt.

3. Iga liikluse haldur profiili võib olla kõige ühe Web Appi lõpp-punkti iga Azure'i piirkonnast. Lahendamiseks see piirang, saate konfigureerida Web Appi kui väline lõpp-punkt. Lisateavet leiate artiklist [FAQ](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Lubamine ja keelamine lõpp-punktid

Lõpp-punkti liiklust halduri keelamine võib olla kasulik ajutiselt liiklust eemaldamiseks lõpp-punkti, mis on hooldus režiimis või kogurahastus. Kui lõpp-punkti uuesti käivitada, saab seda uuesti lubada.

Lõpp-punktid saate lubatud ja keelatud liikluse haldur portaali, PowerShelli, CLI või REST API-ga, mis on toetatud ressursihaldur nii klassikaline juurutamise mudeli kaudu.

>[AZURE.NOTE] Azure'i lõpp keelamine ei ole midagi Azure juurutamise olekuga. Azure'i teenuse (näiteks VM või veebirakenduse jääb töötava ja saada liikluse isegi siis, kui keelatud liikluse haldur. Liikluse võib käsitleda eksemplari asemel kaudu liikluse haldur profiili DNS-i nimi. Lisateabe saamiseks vt [liikluse haldur tööpõhimõte](traffic-manager-how-traffic-manager-works.md).

Praeguse sobilikkuse iga lõpp-punkti saada liikluse sõltub järgmisi tegureid.

- Profiili olek (lubatud või keelatud)
- Lõpp-punkti olek (lubatud või keelatud)
- Tulemuste seisundi kontrollib sellele lõpp

Lisateavet leiate teemast [liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Kuna liikluse haldur töötab DNS-i tasemel, seda ei saa mõjutada olemasolevad ühendused, mis tahes lõpp-punkti. Kui lõpp pole saadaval, suunab liikluse haldur uusi ühendusi teise saadaval lõpp-punkti. Host taha keelatud või vigane lõpp-punkti võib jätkuvalt saada liikluse kaudu olemasolevad ühendused, kuni need seansid lõpetatakse. Rakenduste tuleks piirata seansi tühjendada olemasolevad ühendused liikluse lubamiseks.

Kui kõik lõpp-punktid profiili on keelatud või kui ise profiili on keelatud, liikluse haldur saadab 'NXDOMAIN' vastuseks uue DNS-i päringu.

## <a name="faq"></a>KKK

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Saate kasutada liikluse haldur lõpp-punktid: mitu tellimust?

Lõpp-punktid: mitu tellimust abil ei saa Azure Web Apps. Azure'i veebirakenduste nõuab, et kõik kohandatud domeeninime kasutada veebirakenduste kasutatakse ainult ühe tellimuse sees. Ei saa kasutada veebirakenduste mitu tellimust sama domeeni nimega.

Lõpp-punkti muude tüüpide, on võimalik liikluse Kujundushalduri abil rohkem kui üks tellimuselt lõpp-punktid. Liikluse haldur konfigureerimise sõltub sellest, kas kasutate klassikaline juurutamise mudel või ressursihaldur kogemus.

- Ressursihaldur, mis tahes tellimusest lõpp-punktid saab lisada-liikluse haldur seni, kuni konfigureerimine liikluse haldur profiili isik on lugemisõigus lõpp-punkti. [Azure'i ressursihaldur Rollipõhine juurdepääsu reguleerimine (RBAC)](../active-directory/role-based-access-control-configure.md)abil saab anda need õigused.
- Klassikaline juurutamise mudeli liideses liikluse haldur nõuab pilveteenustega või veebirakenduste konfigureeritud Azure lõpp-punktid asuvad samas tellimuses liikluse haldur profiil. Muude tellimuste pilvepõhise teenuse lõpp-punktid saab lisada liikluse haldur nimega "väline" lõpp-punktid. Azure'i lõpp-punktid, mitte välise määr on arve nende väliste lõpp-punktid.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Saate kasutada liikluse haldur pilveteenuses 'lavastus slots?

Jah. Lavastus slots pilveteenuses saab konfigureerida liikluse haldur väliste lõpp-punktid. Seisundikontrollid nõutakse veel määra Azure'i lõpp-punktid. Kuna väline lõpp-punkti tüüp on kasutusel, aluseks oleva teenuse muudatustest on ei kätte automaatselt. Väliste lõpp-punktid, kus liikluse haldur ei saa tuvastada, kui pilveteenusesse on peatatud või kustutatud. Seetõttu liikluse haldur jätkub seisundikontrollid arveldus, kuni lõpp-punkti on keelatud või kustutada.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Kas liikluse haldur toetab IPv6 lõpp-punktid?

Praegu ei paku liikluse haldur IPv6 addressible nimeserveritele. Siiski saate liikluse haldur kasutada ikka IPv6 kliendid ühenduse IPv6 lõpp-punktid. Klient ei tee DNS päringud otse liikluse haldur. Selle asemel klient kasutab Rekursiivsed DNS-i teenus. Kui IPv6 ainult klient saadab taotlused Rekursiivsed DNS-i teenuse IPv6 kaudu. Seejärel Rekursiivsed teenuse peaks oskama pöörduge liikluse haldur nimeservereid, kasutades IPv4.

DNS-i lõpp-punkti nimi vastab liikluse haldur. Lõpp IPv6 toetamiseks peavad olemas AAAA DNS-i kirje, mis osutab IPv6-aadress DNS-i lõpp-punkti nimi. Liikluse haldur seisundikontrollid toetavad ainult IPv4 aadressid. Teenuse peab esitamist lõpp IPv4 sama DNS-i nimi.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Saate kasutada rohkem kui üks Web App piirkonna liikluse haldur?

Tavaliselt kasutatakse liikluse haldur kasutusele võetud eri piirkondades liikluse suunamiseks. Siiski saate kasutada, kui rakendus on rohkem kui üks juurutamise piirkonna. Liikluse haldur Azure'i lõpp-punktid ei võimalda lisada sama liikluse haldur profiili Azure ühe piirkonna mitu Web Appi lõpp-punkti.

Järgmiste juhiste abil see piirang lahendust:

1. Kontrollige, kas teie lõpp-punktid on erinevate web appi 'skaala üksuste". Domeeninimi peab vastendama ühe saidi antud skaala üksus. Seetõttu ei saa kaks veebirakenduste samas skaala üksuses liikluse haldur profiili ühiskasutusse anda.
2. Lisada kohandatud hostname edevus domeeninime iga Web App. Erinevate skaala ühik peab olema iga Web App. Kõigi veebirakenduste peab kuuluma sama tellimus.
3. Ühe (ja ainult üks) Web Appi lõpp-punkti lisada liikluse haldur profiili nimega Azure lõpp.
4. Iga täiendava Web Appi lõpp-punkti lisada liikluse haldur profiili nimega väline lõpp-punkt. Väliste lõpp-punktide saab lisada ainult ressursihaldur juurutamise mudeli abil.
5. DNS-i CNAME-kirje loomine teie domeeni edevus, mis viitab teie haldur liikluse profiili DNS-i nimi (<> …. trafficmanager.net).
6. Juurdepääs saidi kaudu edevus domeeninimi, mitte liikluse haldur profiili DNS-i nimi.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [liikluse haldur tööpõhimõte](traffic-manager-how-traffic-manager-works.md).
- Teavet liikluse haldur [lõpp-punkti jälgimine ja automaatselt Tõrkesiirde](traffic-manager-monitoring.md).
- Teavet liikluse haldur [marsruutimise meetodite liikluse](traffic-manager-routing-methods.md).
