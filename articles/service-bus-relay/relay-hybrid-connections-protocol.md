<properties 
    pageTitle="Azure'i Relay hübriid ühendused protokoll | Microsoft Azure'i"
    description="zure Relay hübriid ühendused protokolli juhend."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure'i Relay hübriid ühendused Protocol (protokoll)

Azure'i edastamine on üks klahv võimalus samba Azure'i teenus platvormile. Funktsiooni Relay uue "Hübriid ühendused" võimalus on turvaline, Ava-protokolli uuendus, mis põhineb HTTP ja WebSockets.

See alistab sama nimega "BizTalki teenused" funktsioon, mis oli ehitatud kaitstud protokolli foundation esimesena. Hübriid ühendused integreerimine Azure rakenduse Services kasutab ka edaspidi toimida-on.

"Hübriid ühendused" võimaldab, millega kahesuunalise, kahendarvu voo suhtlemine kaks võrku ühendatud rakendust, mille taga NATs või tulemüürid võivad asuda üks või mõlemad pooled.

Selle dokumendi kasutusviisid on kliendipoolne hübriid ühendused relay ühenduse kliendid kuulajale ja saatja rollid ja kuidas kuulajatele aktsepteerige uue ühenduse abil.

## <a name="interaction-model"></a>Suhtluse mudel

Hübriidjuurutuse ühendused relay loob osapooled esitada Azure'i pilves, et nii, et isikud saavad leida ja ühenduse loomiseks oma võrgu perspektiivi rendezvous punkti. Siinkohal rendezvous nimetatakse "Hübriid ühendus" see ja muud dokumendid on API-d ja Azure portaalis. Teenuse lõpp-punkti hübriid ühendused on edaspidi "teenus" selle dokumendi ülejäänud.

Suhtluse mudeli leans loodud mitme võrgu API nomenklatuuri:

On kuulajale, mis näitab esmalt valmisoleku käsitlema sissetulevaid ühendusi ja seejärel aktsepteerib nende saabumisel. Teisel pool on ühendusjooni klient, mis loob ühenduse kuulajale, eeldatakse, et ühenduse loomise kahesuunaline ühendus tee aktsepteeritud. "Ühendust", "Kuulata", "Nõus" on enamik turvasoklite API-de kohta leiate tingimustel.

Mis tahes võrdõigusvõrku side mudel on kumbagi, veenduge, et väljaminevad ühendused suunas on teenuse lõpp-punkti, mis teeb "Kuulajale" ka "klient" kõnekeelne Kasuta ja võib põhjustada ka muid terminoloogia ülekoormuse; Seetõttu kasutame hübriid ühenduste täpne terminid on järgmine:

Mõlemale poolele ühenduse programme nimetatakse "klient", kuna need on kliendid teenusega. Klient, mis ootab ja aktsepteerib ühendused on "Kuulajale" või "kuulajale rolli" olevat. Kliendi, mis käivitab uue ühenduse suunas on kuulajale teenuse kaudu nimetatakse "saatja" või "saatja roll".

## <a name="listener-interactions"></a>Kuulajale kasutusviisid

Kuulajale on neli kasutusviisid teenuse; Kõik kaabel üksikasjad on kirjeldatud hiljem viide jaotises selle dokumendi.

### <a name="listen"></a>Kuulake

Tähistamiseks valmisolek teenus, mis on kuulajale on valmis vastu võtma ühendusi, see loob väljaminev web turvasoklite ühenduse. Ühenduse käepigistus kannab nime ühenduse hübriid konfigureeritud nimeruumi edastamiseks ja turvalisuse luba, mis annab õiguse "Kuulata" mida nime. Kui web turvasoklite vastu teenuse, registreerimine on lõpule viidud ja loodud web turvasoklite säilitatakse elus nagu "juhtimise kanal", mis võimaldab kõigi järgnevate kasutusviisid. Teenuse võimaldab kuni 25 samaaegseid kuulajatele ühenduse hübriid. Kui 2 või rohkem aktiivne kuulajatele, sissetulevaid ühendusi tasakaalustatakse kogu neid juhuslikus järjekorras; ei ole tagatud ja tehnikamessi jaotuse.

### <a name="accept"></a>Aktsepteerige

Alati saatja avatakse uus ühendus teenuse, teenuse valimine ja teavitamise üks aktiivne kuulajatele ühenduse hübriid. Teate saadetakse kuulajale üle avatud juhtelemendi kanali JSON sõnumina, mis sisaldab Web turvasoklite näitaja, mis on kuulajale tuleb ühendada vastuvõtmise ühenduse URL-i.

URL-i saate ja peate kasutama otse kuulajale ilma mis tahes lisatööd; encoded teave kehtib ainult lühikest aega, sisuliselt nii kaua, kui saatja on valmis ootama ühendus on loodud end end, kuid kuni 30 sekundit. URL-i saab kasutada ainult ühe eduka ühenduse loomise katse. Kui rendezvous URL-iga Web turvasoklite ühendus on loodud, edastatakse kõik edasise tegevuse Web pesa alates ja kuni saatja ilma sekkumise või teenuse poolt.

### <a name="renew"></a>Pikendamine 

Turbeloa, mida tuleb kasutada kuulajale registreerida ja säilitada juhtelemendi kanali aeguvad kuulajale on aktiivne. Turbeloa aegumise ei mõjuta poolelioleva ühendusi, kuid see põhjustada juhtelemendi kanali loobumist või varsti pärast aegumise vahetu teenus võttis. "Pikendada" žesti on JSON sõnum, mis on kuulajale saavad saata asendamine seotud juhtelemendi kanali, luba juhtelemendi kanali pikaks säilitamiseks nii, et.

### <a name="ping"></a>Ping 

Kui juhtelemendi kanali jääb jõude pikka aega, vahendajate teel, nt laadi soolise või NATs võib langeda TCP ühendus. "Ping" žest aitab vältida mis väike andmete kanal, mis meenutab kõigile ühendus on mõeldud elus ja see ka toimib liveness testi kuulajale võrgu protsessi saatmisega. Ping nurjumisel juhtelemendi kanali tuleks kaaluda kasutuskõlbmatuks ja kuulajale tuleks taastada.

## <a name="sender-interaction"></a>Saatja suhtlust

Saatja on ainult üks suhtluse teenuse, loob see.

### <a name="connect"></a>Ühenduse loomine

"Ühendust" žest avatakse Web olevasse teenust ühenduse hübriid nime ja (valikuline, kuid vaikimisi vajalik) Turbeloa annab päringu stringi "Saada" luba. Teenuse seejärel kuulajale eespool kirjeldatud viisil suhelda ja on kuulajale rendezvous ühendus, mis on ühendatud koos Web pesa loomine. Pärast Web turvasoklite aktsepteerimist, seega on kõik edasise kasutusviisid Web turvasoklite kohta koos ühendatud kuulajale.

## <a name="interaction-summary"></a>Suhtluse Kokkuvõte

Selle suhtluse mudeli tulemus on, et saatja kliendi väljub käepigistus koos "puhas" Web turvasoklite, mis on ühendatud on kuulajale ja mis tuleb pole põhjalikumaks kvalifitseerimisdirektiivi või ettevalmistamine. See võimaldab praktiliselt mis tahes olemasolevaid Web turvasoklite kliendi kergesti ära hübriid ühendused teenuse lihtsalt esitades õigesti arvestusliku URL-i oma Web kliendi turvasoklite kiht.

Rendezvous ühenduse Web turvasoklite, mis on kuulajale hangib Aktsepteeri suhtluse kaudu on ka selge ja mis tahes olemasoleva Web turvasoklite serveri juurutamine minimaalsete eest võtmiseks, mis eristab "Aktsepteeri" toiminguid nende raamistik kohtvõrgu kuulajatele ja hübriid ühendused remote "Aktsepteeri" toimingute abil saate anda.

## <a name="protocol-reference"></a>Viide Protocol (protokoll)

Selles jaotises kirjeldatakse protokolli kasutusviisid, mis on kirjeldatud ülal üksikasju.

Kõik Web turvasoklite ühendused tehakse pordi 443 versioonitäienduse HTTPS 1.1, mis on võetud sagedamini mõned Web turvasoklite framework või API kaudu. Kirjeldus siin säilitatakse rakendamist neutraalne, kasutamata konkreetne raamistik.

## <a name="listener-protocol"></a>Kuulajale Protocol (protokoll)

Kuulajale protokoll koosneb kahe ühenduse žeste ja kolm sõnumi toimingud.

### <a name="listener-control-channel-connection"></a>Kuulajale juhtelemendi kanali ühendust

Juhtelemendi kanali avatakse Web turvasoklite ühenduse loomisel:

*wss: / / {nimeruum-aadress} /* *$servicebus* */* *hybridconnection /**{path}? sb-hc-action =... & sb-hc-id =... & sb-hc-luba =... "*

*Nimeruumi-aadress* on täielik domeeninimi Azure'i Relay nimeruumi, mis hostib hübriid ühendamine, tavaliselt vormi {*myname}. servicebus.windows.net.*

Päringu stringi parameeter suvandid on järgmised

| Param        | See on nõutav? | Kirjeldus                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-toiming | Jah       | Kuulajale rolli parameeter peab olema **sb-hc-action = kuulata**                                                                                                                                |
| {path}       | Jah       | Eelkonfigureeritud hübriid ühenduse registreeruda selle kuulajale URL-kodeeringuga nimeruum tee. See avaldis on lisatud, et fikseeritud * **$servicebus**/**hybridconnection /*** tee osa. |
| SB-hc-luba  | Jah\*     | Kuulajale peab võimaldama on lubatud, URL-kodeeringuga teenuse siini ühiskasutusse Accessi Turbeloa nimeruum või hübriid-ühenduse, mis annab õiguse **kuulata** .                                           |
| SB-hc-id     | Ei        | Kliendi esitatud valikuline ID võimaldab lõpuni diagnostika jälgimine.                                                                                                                             |

Kui Web turvasoklite nurjub tõttu ühenduse hübriid tee pole registreeritud, või mõne sobimatu või puudub luba või mõni muu probleem, antakse tõrge tagasiside näidise tavalise HTTP 1.1 olek tagasiside. Oleku kirjeldus sisaldab viga jälitus-id, mis tuleb edastada Azure'i tugi:

| Kood | Tõrge          | Kirjeldus                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Ei leitud      | Ühenduse hübriid **tee** ei sobi või base URL on vigane |
| 401  | Volitused   | Turbeloa on puudu või vigane või ei sobi                  |
| 403  | Keelatud      | Turbeloa ei sobi selle tee selle toimingu jaoks          |
| 500  | Sisemine tõrge | Midagi läks valesti teenus                                    |

Kui turvasoklite veebiühendus on teadlikult sulgeda teenus võttis pärast seda, kui see on algselt häälestatud, põhjus selleks nii, et see edastatakse vastav Web turvasoklite protokolli tõrkekood koos kirjeldav tõrketeade, mis sisaldab ka jälgimise-ID-d. Teenust ei sulgeda juhtelemendi kanali ilma tõrkeolukorras. Mis tahes clean sulgumist on kliendi kontrollitud.

| OLI olek | Kirjeldus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Ühenduse hübriid tee on kustutatud või keelatud.                           |
| 1008      | Turbeloa on aegunud ja luba poliitika on seega rikutud. |
| 1011      | Midagi läks valesti teenuse sees.                                           |

### <a name="accept-handshake"></a>Aktsepteerige käepigistus 

Aktsepteeri teatis saadetakse teenus võttis kuulajale üle varem loodud juhtelemendi kanali JSON-sõnumina turvasoklite teksti veebilehelt. Ei ole vastust sellele sõnumile.

Sõnum sisaldab JSON objekti nimega "Aktsepteeri", mis määratleb praegu järgmised atribuudid:

-   **aadress** – URL-i stringi kinnitamiseks sissetuleva ühenduse loomise Web turvasoklite teenusega kasutada.

-   **ID-d** – sellega ainuidentifikaator. Kui saatja kliendi tarniti ID-d, see on esitatud saatja, muidu on süsteemi genereeritud väärtus.

-   **connectHeaders** – kõik HTTP päised, mis on antud Relay lõpp-punkti saatja, mis sisaldab ka Sec WebSocket protokoll ja Sec-WebSocket-laiendid päised.

| Aktsepteerimiseks                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

Esitatud JSON sõnumis aadressi URL-i kasutatakse kuulajale Web turvasoklite aktsepteerimine või hülgamine saatjale pesa jaoks luua.

#### <a name="accepting-the-socket"></a>Aktsepteerige pesa

Aktsepteerimiseks kuulajale loob WebSocket ühenduse esitatud aadressile.

Meeles pidada, et eelvaade perioodi URI aadress võib kasutada tühjal IP-aadress ja TLS-i serdi esitatud server antud aadressil valideerimine nurjub. See kõrvaldatakse eelvaates.

Kui sõnum "aktsepteerida" "Sec-WebSocket-protokolli" päis, eeldatakse, et kuulajale ainult aktsepteerib soovitud WebSocket, kui see toetab protokolli ja seda määrab päis on WebSocket sellisena.

Sama kehtib "Sec-WebSocket-laiendid" päis. Kui raamistik toetab laiend, selle *serveri* pool vastust nõutavad "Sec-WebSocket-laiendid" käepigistus pikendamise seada päis.

URL-i tuleb kasutada-on Aktsepteeri turvasoklite loomise, kuid sisaldab järgmisi:

| Param        | See on nõutav? | Kirjeldus                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-toiming | Jah       | Vastu võtmist on turvasoklite peab olema parameetri **sb-hc-action = aktsepteerida**                                                                                                                                                                                                                          |
| {path}       | Jah       | Eelkonfigureeritud hübriid ühenduse registreeruda selle kuulajale URL-kodeeringuga nimeruum tee. See avaldis on lisatud abil fikseeritud * **$servicebus**/**hybridconnection /*** tee osa.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-hc-id | No        | Vt **id** kirjeldus.                                                                                                                                                                                                                                                              |

Kui tekib tõrge, teenuse võivad vastata järgmiselt:

| Kood | Tõrge          | Kirjeldus                         |
|------|----------------|-------------------------------------|
| 403  | Keelatud      | URL-i ei sobi.               |
| 500  | Sisemine tõrge | Midagi läks valesti teenus |

Pärast seda, kui ühendus on loodud, serveri sulgub Web turvasoklite kui saatja Web turvasoklite sulgub alla või järgmised olekuga

| OLI olek | Kirjeldus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Saatja kliendi sulgub ühendus                                        |
| 1001      | Ühenduse hübriid tee on kustutatud või keelatud.                           |
| 1008      | Turbeloa on aegunud ja luba poliitika on seega rikutud. |
| 1011      | Midagi läks valesti teenuse sees.                                           |

#### <a name="rejecting-the-socket"></a>Lükake need tagasi pesa

Lükake need tagasi pesa pärast sõnumi "aktsepteerida" kontrollimise nõuab sarnaseid käepigistus nii, et olek ja oleku kirjeldus suhtlemine tagasilükkamise põhjuse saate flow saatjale.

Protocol (protokoll) design valikut siin on kasutada WebSocket käepigistus (mis on mõeldud määratletud tõrkeolekus lõppu), et kuulajale kliendi rakendusi saate jätkata toetuvad WebSocket klient ja ei pea tööle eest, tühjal HTTP kliendi.

Olevasse hülgamiseks kliendi võtab aadressi URI sõnumist "aktsepteerida" ja lisab selle kaks päringustringi parameetrite.

| Param             | See on nõutav? | Kirjeldus                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Jah       | Arvuline HTTP olekukoodi                |
| statusDescription | Jah       | Inimeste loetav tagasilükkamise põhjuse |

Selle tulemusena tekkivad URI kasutatakse WebSocket ühenduse loomiseks; Klõpsake uuesti arvesse, et TLS-i serdi võib ei vasta aadressi eelvaate, seega valideerimine võib-olla peate olema keelatud.

Õigesti täitmisel selle käepigistus tahtlikult nurjuvad HTTP tõrkekood 410, sest puudub WebSocket on välja töötatud. Kui ilmneb tõrge, on need suvandid.

| Kood | Tõrge          | Kirjeldus                         |
|------|----------------|-------------------------------------|
| 403  | Keelatud      | URL-i ei sobi.               |
| 500  | Sisemine tõrge | Midagi läks valesti teenus |

### <a name="listener-token-renewal"></a>Kuulajale Turbeloa pikendamine

Kui on kuulajale luba on aegumas, seda saate asendada, saates paneeli tekstsõnum kontroll kanali kaudu. Sõnum sisaldab nimega "renewToken", mis määratleb järgmised atribuudi praegu JSON objekti:

-   **Turbeloa** – on lubatud, URL-kodeeringuga teenuse siini ühiskasutusse Accessi Turbeloa nimeruumi või hübriid ühendus, mis annab õiguse **kuulata** .

| renewToken sõnum                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Turbeloa valideerimine nurjub, juurdepääs on keelatud, kui pilveteenuses suletakse juhtelemendi kanali websocket viga, muidu ei ole vastust.

| OLI olek | Kirjeldus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Turbeloa on aegunud ja luba poliitika on seega rikutud. |

<a name="sender-protocol"></a>Saatja Protocol (protokoll)
---------------

Saatja protokoll on tõhus identne, kuidas on kuulajale on loodud. Eesmärk on maksimaalne läbipaistvus-lõpuni Web turvasoklite jaoks. Ühenduse aadress on sama, mis on kuulajale, kuid "toimingu" erineb ja luba vajab eri luba.

*wss: / / {nimeruum-aadress} /* *$servicebus* */* *hybridconnection /**{path}? sb-hc-action =... & sb-hc-id =... \[ja sbc-hc-luba =... \]*

*Nimeruumi-aadress* on täielik domeeninimi Azure'i Relay nimeruumi, mis hostib hübriid ühendamine, tavaliselt vormi {*myname}. servicebus.windows.net.*

Taotlus võib sisaldada suvalise eest HTTP päised, sealhulgas rakenduste määratletud. Kõigi esitatud päiste flow kuulajale ja leiate objektil "connectHeader", "aktsepteerida" kontrolli sõnumi.

Päringu stringi parameeter suvandid on järgmised

| Param        | See on nõutav? | Kirjeldus                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-hc-toiming | Jah       | Kuulajale rolli parameeter peab olema **toimingu = ühenduse**                                                                                                                                                    |
| {path}       | Jah       | Eelkonfigureeritud hübriid ühenduse registreeruda selle kuulajale URL-kodeeringuga nimeruum tee.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-hc-luba | Yes\*     | Kuulajale peab võimaldama on lubatud, URL-kodeeringuga teenuse siini ühiskasutusse Accessi Turbeloa nimeruum või hübriid ühendus, mis annab õiguse **saata** .                                                            | | SB-hc-id | No        | Valikuline ID, mis võimaldab lõpuni diagnostika jälgimine ja tehakse kättesaadavaks kuulajale Aktsepteeri käepigistuse ajal.                                                                                       |

Kui Web turvasoklite nurjub tõttu ühenduse hübriid tee pole registreeritud, või mõne sobimatu või puudub luba või mõni muu probleem, antakse tõrge tagasiside näidise tavalise HTTP 1.1 olek tagasiside. Oleku kirjeldus sisaldab viga jälitus-id, mis tuleb edastada Azure'i tugi:

| Kood | Tõrge          | Kirjeldus                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Ei leitud      | Ühenduse hübriid **tee** ei sobi või base URL on vigane |
| 401  | Volitused   | Turbeloa on puudu või vigane või ei sobi                  |
| 403  | Keelatud      | Turbeloa ei sobi selle tee selle toimingu jaoks          |
| 500  | Sisemine tõrge | Midagi läks valesti teenus                                    |

Kui turvasoklite veebiühendus on teadlikult sulgeda teenus võttis pärast seda, kui see on algselt häälestatud, põhjus selleks nii, et see edastatakse vastav Web turvasoklite protokolli tõrkekood koos kirjeldav tõrketeade, mis sisaldab ka jälgimise-ID-d.

| OLI olek | Kirjeldus                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | Kuulajale sulgeda pesa.                                                 |
| 1001      | Ühenduse hübriid tee on kustutatud või keelatud.                           |
| 1008      | Turbeloa on aegunud ja luba poliitika on seega rikutud. |
| 1011      | Midagi läks valesti teenuse sees.                                           |

## <a name="next-steps"></a>Järgmised toimingud:

- [Relay KKK](relay-faq.md)
- [Nimeruumi loomine](relay-create-namespace-portal.md)
- [.Net-i kasutamise alustamine](relay-hybrid-connections-dotnet-get-started.md)
- [Alustamine sõlm](relay-hybrid-connections-node-get-started.md)