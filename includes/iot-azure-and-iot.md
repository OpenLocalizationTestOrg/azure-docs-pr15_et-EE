
# <a name="azure-and-internet-of-things"></a>Azure ja asjade Internet

Tere tulemast Microsoft Azure ja asjade Internet. Artiklis tutvustatakse IoT lahendust arhitektuur, mis kirjeldab IoT lahendust, võib kasutada Azure'i teenuste kasutamise ühised tunnused. Asjade Interneti lahendused nõuavad turvaline, kahesuunaline side seadmete, nummerdamine võimalik miljoneid ja lahendus tagasi lõpuks. Näiteks lahendus tagasi lõpuks kasutada automatiseeritud, ennustava analytics avastada teadmisi seadme pilve juhul oja.

Azure'i asjade Interneti jaotur on võtmetähtsusega, kui rakendate IoT lahendust arhitektuur Azure'i teenuste kasutamine. Lot Suite pakub see arhitektuur konkreetsete asjade stsenaariumide täielik, end-to-end rakendusi. Näiteks: 

- *Kaugseire* lahendus võimaldab teil jälgida seadmete nagu müügiautomaadid. 
- *Ennetava hoolduse* lahendus aitab ennetada seadmete, näiteks pumpade remote pumbajaamad hoolduskulud ja vältida planeerimata seisakuid.

## <a name="iot-solution-architecture"></a>IoT lahendust arhitektuur

Järgmine diagramm näitab tüüpiline asjade Interneti lahenduse arhitektuuri. Diagramm ei sisalda Azure'i eriteenuste nimed, kuid kirjeldab üldiste asjade Interneti lahenduse arhitektuuri peamised elemendid. See arhitektuur IoT seadmete koguda andmeid, mis nad saada pilve värav. Pilve gateway tagab andmete saadaval teisi lõppfaasi teenuseid kui kohaletoimetamis muud line-of-business rakendused või inimeste ettevõtjatele armatuurlaua või muu esitluse seadme töötlemine.

![IoT lahendust arhitektuur][img-solution-architecture]

> [AZURE.NOTE] Asjade Interneti arhitektuuri põhjalik arutelu, vaadake [Microsoft Azure IoT viide arhitektuuri][lnk-refarch].

### <a name="device-connectivity"></a>Seadme funktsioon

Asjade Interneti lahenduse arhitektuuri, seadmete saada telemeetria, näiteks andurite lugemeid PUMBAMAJA, salvestamise ja töötlemise pilve lõpp-punkti. Ennustav hooldus stsenaariumi kolp abil andurite andmeid stream kindlaks, millal konkreetsete pump vajab hooldust. Saavad ka vastu võtta ja vastata pilve seadme käsud lugedes sõnumeid pilve lõpp. Nt ennustav hooldus stsenaariumi lahendus tagasi lõpuks võib käsud saada teiste pumpade suunamine voolab vahetult enne hooldus on kavas alustada hooldus insener saab alustada, kui ta saabub veenduge alustada PUMBAMAJA.

Üks suurimaid väljakutseid IoT projektide on usaldusväärselt ja kindlalt ühendamise seadmete lahendus tagasi lõpuks. Asjade Interneti seadmed on erinevad omadused, võrreldes teiste klientide nagu brauserid ja mobiilirakendused. Asjade Interneti seadmed:

- Sageli pole inimeste tehtemärki manussüsteemid.
- Loovad kõrvalistes kohtades, kus ligipääs on kallis.
- Võib lahendus kolp kaudu kättesaadav. Ei ole muul viisil suhelda seade.
- Võivad olla piiratud võimu ja ressursside töötlemine.
- Võib olla vahelduv, aeglane või kallis võrguühenduse.
- Võib tekkida vajadus kasutada varaliste, kohandatud või -tööstusliku rakendus protokollid.
- Saab luua kasutades suure hulga populaarsem riist- ja tarkvara platvormid.

Lisaks eespool esitatud nõuetele peab oma IoT lahendust ka skaala, turvalisust ja usaldusväärsust. Saadud ühenduse nõuete kogum on raske ja aeganõudev rakendada traditsioonilisi abil, nagu web konteinerid ja maaklerite jaoks. Azure'i IoT keskus ja asjade Interneti seadme SDK-d oleks lihtsam lahendusi, mis vastavad nendele nõudmistele.

Seade saab suhelda otse pilve gateway lõpp-punkti või kui seade ei saa kasutada sideprotokolle, pilve gateway toetab, see ühenduse vahe lüüsi kaudu. Näiteks [Azure asjade Interneti protokolli gateway] [ lnk-protocol-gateway] protokolli tõlke saate sooritada, kui seadmed ei saa kasutada mida IoT Keskus toetab.

### <a name="data-processing-and-analytics"></a>Andmete töötlemise ja analüüsi

Pilv, on lahendus tagasi end IoT on enamik andmetöötluse toimumise, nagu filtreerimine ja kokkuvõtmise telemeetria ja muude teenuste saadetaks. End IoT lahendust tagasi:

- Telemeetria alal saab seadmetega ja määrab, kuidas töödelda ja säilitada andmeid. 
- Võimaldab saata käske pilvest kindla seadme.
- Pakub seadme registreerimise võimalustest, mille abil säte ning kontrolli seadmed võivad ühendada oma infrastruktuuri.
- Võimaldab jälgida seadmete seisukorra ja nende tegevust kontrollima.

Ennustav hooldus stsenaariumi lahendus kolp talletab ajaloolise koguda andmeid. Tagasi lõpuks võib neid andmeid kasutada tähistama mustrid, mis näitavad ülalpidamist tuleb konkreetse pump.

Asjade Interneti lahendused võivad sisaldada automaatne tagasiside saamine praktikas. Näiteks analytics-moodul tagasi lõpuks tunda: telemeetria, mis kindla seadme temperatuur on normaalne tegevus tase üle. Lahus võib seejärel saata seade, juhendades korrigeerivate meetmete võtmiseks.

### <a name="presentation-and-business-connectivity"></a>Esitlus ja äri

Esitlus ja business connectivity kiht võimaldab suhelda IoT lahendust ja seadmed. See võimaldab kasutajatel vaadata ja analüüsida nende seadmete kogutud andmeid. Neid seisukohti saab olla armatuurlauad või BI aruanded, mida saab kuvada nii piisavalt andmeid või peaaegu reaalajas andmeid. Näiteks ettevõtja saab kontrollida staatuse eriti PUMBAMAJA ja näha teatisi süsteemi poolt. See võimaldab integratsiooni IoT lahendust kolp olemasolevate line-of-business rakendused siduda ettevõtte äriprotsesse või töövoogusid. Näiteks ennetava hoolduse lahendus integreerida planeerimise süsteem, mis raamatud insener külastada on PUMBAMAJA kui lahendus tuvastab pump vajab hooldust.

![IoT lahendust armatuurlaud][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
