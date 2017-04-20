> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

See artikkel sisaldab üksikasjalikku ülevaadet [Hello World kood] [ lnk-helloworld-sample] [Azure asjade Interneti Gateway SDK] põhikomponente illustreerimiseks[ lnk-gateway-sdk] arhitektuur. Proovi kasutab IoT Rummu Gateway SDK ehitada lihtne värav, logib sõnumi "hello world" faili iga viie sekundi järel.

See teid läbi hõlmab:

- **Mõisted**: kontseptuaalne ülevaade komponendid, mis moodustavad iga gateway loote asjade Interneti Gateway SDK.  
- **Hello World proovi arhitektuur**: kirjeldatakse, kuidas mõisted kehtivad Hello World proovi ja kuidas osad kokku sobivad.
- **Kuidas luua proovi**: ehitada proovi etapid.
- **Kuidas lastakse**: lastakse etapid. 
- **Tüüpiline väljund**: näiteks väljund oodata proovi käivitamisel.
- **Koodilõike**: koodilõike näidata, kuidas Hello World proovi rakendab võti värav komponentide kogum.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure'i asjade Interneti Gateway SDK mõisted

Enne uurida kood või luua oma asjade Interneti Gateway SDK abil välja gateway, sa peaksid mõistma põhimõisteid, millele toetub SDK arhitektuur.

### <a name="modules"></a>Moodulid

Loote värav Azure asjade Interneti Gateway SDK loomise ja kokkupanek *moodulid*. Moodulite abil *sõnumeid* omavahel andmeid vahetada. Moodul teade, sooritab mõne toimingu peal, soovi korral muudab see uue sõnumi ja avaldab see teistes moodulites töötlemiseks. Mõned moodulid võib toota ainult uued sõnumid ja kunagi protsessi sissetulevad sõnumid. Moodulid kett loob andmete töötlemisel torujuhe täita ümberkujundamist torujuhtme ühel hetkel andmed iga mooduliga.

![Kett moodulitest ehitatud Azure asjade Interneti Gateway SDK Gateway][1]
 
SDK sisaldab järgmist:

- Eelnevalt kirjutatud moodulid, mis toimivad ühise gateway.
- Liidesed arendaja abil kohandatud moodulid kirjutate.
- Juurutada ja käivitada mooduleid vajalikku infrastruktuuri.

SDK pakub HAL-kihi, mis võimaldab ehitada sinna panna erinevaid operatsioonisüsteeme ja platvorme.

![Azure'i asjade Interneti Gateway SDK HAL-kihi][2]

### <a name="messages"></a>Sõnumid

Kuigi moodulite läbimise üksteisele mõeldes on mugavam ettekujutus kuidas gateway funktsioone, see ei kajasta täpselt mis juhtub. Moodulite abil suhelda, nad sõnumite avaldamiseks Broker (bussi-, pubsub või sõnumside muster) ja siis lasta suunata sõnum ühendatud moodulid maakler maakler.

On moodulite kasutab funktsiooni **Broker_Publish** avaldada teade maakler. Ta toimetab sõnumeid moodul tuginedes tagasikutse funktsiooni. Sõnum koosneb võtme/väärtuse ja läbinud ploki mälu sisu.

![Azure asjade Interneti Gateway SDK maakleri roll][3]

### <a name="message-routing-and-filtering"></a>Teade marsruudi ja filtreerimine

On kaks võimalust, suunates sõnumid õige moodulid. Kogum linke saab ta edasi nii, et ta teab allikas ja valamu moodulite või sõnumi atribuute saate filtreerida moodul. Moodul tuleks tegutseb ainult sõnumi kui sõnum on mõeldud. Lingid ja meilifiltreering, mida tekitab tõhusalt konveier sõnum.

## <a name="hello-world-sample-architecture"></a>Tere maailma proovi arhitektuur

Hello World proov illustreerib eelmises osas kirjeldatud kontseptsioone. Hello World proovi rakendab gateway, mis on konveier koosneb kahest moodulist:

-   *Tere maailm* moodul loob sõnumi iga viie sekundi järel ning liigub puuraidur moodul.
-   *Puuraidur* moodul kirjutab saadud sõnumid faili.

![Hello World proovi Azure asjade Interneti Gateway SDK ehitatud arhitektuur][4]

Eelmises osas kirjeldatud Hello World moodul ei edasta sõnumeid otse puuraidur moodul iga viie sekundi järel. Selle asemel, ning avaldab teate Broker iga viie sekundi järel.

Puuraidur moodul saab ta sõnumi ja tegude pärast, kirjalikult sisu sõnumi faili.

Puuraidur moodul ainult tarbib ta sõnumeid, see avaldab kunagi uute sõnumite maakler.

![Kuidas ta marsruudib sõnumeid Azure'i asjade Interneti Gateway SDK moodulite vahel][5]

Ülaltoodud joonisel Hello World proovi ja suhtelised teed allikas faile, mida rakendada eri osadest proovi [hoidla]arhitektuuri[lnk-gateway-sdk]. Uurida kood ise või kasutada koodilõike allpool juhend.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk