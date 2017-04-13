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

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Data Service avaldamise juhend Azure'i turuplats

>[AZURE.IMPORTANT] **Sel ajal me ei ole enam kasutuselevõtt mis tahes uute andmete teenuse tootjad. Uue dataservices ei saada heaks kirjet.** Kui teil on SaaS ärirakendusi soovite avaldada AppSource leiate lisateavet [siin](https://appsource.microsoft.com/partners). Kui teil on mõni IaaS rakendusi või arendaja teenuse soovite avaldatava Azure'i turuplatsi võite leida rohkem teavet [siin](https://azure.microsoft.com/marketplace/programs/certified/).

Pärast punkti 1; [konto loomine ja registreerimine](marketplace-publishing-accounts-creation-registration.md), saame juhendamisega saate läbi [Üldine mittetehnilistest](marketplace-publishing-pre-requisites.md) ja andmete teenuse pakkumine [Tehnilised nõuded](marketplace-publishing-data-service-creation-prerequisites.md) Azure'i turuplatsilt. Nüüd kuvatakse tutvustame teile juhised loomine andmete teenuse pakkumine [Avaldamisportaal] [ link-pubportal] Azure'i turuplatsil.

## <a name="1---login-to-the-publishing-portal"></a>1. avalda Logi portaali.

Minge [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Esimest korda sisse logida Avaldamisportaal, kasutage sama konto, millega teie ettevõtte müüja profiili registrisse Arenduskeskus.**  (Hiljem saate lisada mis tahes teie ettevõtte töötaja co admin avaldamise portaalis).

Klõpsake paani **Avalda Data Services** , kui see on pärast esimest sisselogimist avaldamise portaali sisse.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. Klõpsake vasakpoolses navigeerimismenüüs **Data Services** .

  ![Joonis](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. Looge uus andmete teenus

Täitke oma uute andmete teenuse pakkumine pealkiri ja klõpsake nuppu "+" paremal.

  ![Joonis](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Vaadake jaotises navigeerimismenüüs vastloodud andmeteenuse alammenüü.

Klõpsake vahekaarti **kiirtutvustus** ja vaadake üle kõik vajalikud juhised avaldamiseks õigesti Data Service Azure'i turuplatsilt.

> [AZURE.TIP] Alati saate lingid "Kiirtutvustus" lehel klõpsake nuppu või menüüd kasutamine andmete teenuse pakkumine alammenüü vasakus servas.

## <a name="5---create-a-new-plan"></a>5. Loo uus leping.

### <a name="offers-plans-transactions"></a>Pakub, lepingud, tehingud.

Iga pakkumine võib olla mitu lepingud, kuid peab olema vähemalt üks (1) leping. Kui teie pakkumise tellimine lõppkasutajad need tellige ühe pakkumise leping. Iga leping määratleb, kuidas lõppkasutajad saab kasutada teenust.

Praegu Azure'i turuplatsi toetavad ainult kuu tellimuse tehingu vastavalt mudeli Data Services, st lõppkasutajad maksate kuutasu vastavalt kindla kava need tellinud hind ja saab kasutada iga kuu arvuga määratletud leping.

Iga tehingu tavaliselt määratletud teie andmete teenus tagastab kirjete arvu põhjal teenuse saadetud päringu. Vaikimisi on 100 eurot. Iga päringu tagastatud tehingute arv on arv kirjeid jagatuna 100 ja ümardatud üles lähima täisarvuni.

Vastutab Azure'i turuplatsi teenus kiht (meter) arvu tehingud tarbitud iga päringu jälgimiseks.

> [AZURE.IMPORTANT] Lõppkasutajad, mille limiit kuu jooksul blokeeritud jätkuva teenust kasutada nende kuu jooksul tellimuse lõpuni.

> Leping või lepingud üks saate (kuid mitte peab) kaasata piiramatu arv tehinguid.

### <a name="create-a-plan"></a>Plaani loomine.
1. Klõpsake **"+"** "Lisada uue lepingu" kõrval.

2. Valige üks suvanditest: **piiramatu** või **piiratud** kasutamine selle lepingu raames.  Kui seejärel andke piiratud arv tehingust võimaldab leping kasutamine kuus.

    ![Joonis](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Avaldamise portaali ka soovitada "Leping identifikaator", mille kasutatakse lõppkasutajate suhelda plaani UI nime ja kasutada ka turuplatsil teenuse leping tuvastamiseks. Kui soovite, saate muuta "Leping identifikaator".

    > [AZURE.NOTE] "Leping identifikaator" peab olema kordumatu iga pakkumise piires. Paljude muude identifikaatorite kasutatud identifikaator avaldamise portaali leping ei saa pärast esimest avaldamise tootmisele ja te ei saa muuta selle identifikaator.

3. Klõpsake oma valiku kinnitamiseks.

4. Siis teil palutakse mõni Lisaküsimuste kohta teie äsja loodud leping.

    ![Joonis](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Küsimus|Tähendus|
|----|----|
|**See on tasuta ja saadaval kogu maailmas?**|Saate luua täiesti – tasuta leping. Kui see on ainult kavandamine selle pakkumise – see tähendab, et teil on avaldamise "Tasuta pakkuda" kuvatakse Marketplace'ist. Kui see on ainult üks (vähe) lepingut, see pakub lõppkasutajad Lisateavet suhteliselt väike arv tehinguid kuus teenust pakub võimalust.  Kui vastus on "Jah", siis pole veel küsitakse.|

> [AZURE.NOTE] Lõppkasutajad saavad uuendada alati makstud lepingud.

|Küsimus|Tähendus|
|----|----|
|**On tasuta prooviversioon?**|Saate valida "No prooviversiooni" üldse või anda võimalus kasutada oma lepingu "Kuu". Selle suvandi abil saate pakkuda võimalust mõista pakkumine tasuta üks kuu eelised nagu tootjad.|

> [AZURE.IMPORTANT] Lõppkasutajad saab ainult tasuta prooviversiooni ostmiseks, kui nad on makseviisi nt krediitkaart ettevõtteleping.

> Pärast ühe kuu tasuta prooviversioon, Azure'i turuplatsi hakkavad laadimise kliendid hind päevast tellimus, kui kliendi algatatud tellimuse tühistamine. Lõppkasutajate antakse teisiti toosti.

|Küsimus|Tähendus|
|----|----|
|**See leping nõuab edendamine kood osta?**| Tootjad on võimalus piirata juurdepääsu teenuse lepingud esitada teisiti koodi nimega "A Promocode" teatud klientidele. Lõppkasutajad, mis on selles Promocode saab tellida leping. Kui valite "Ei", siis nõustute, et igaüks piirkonnast, kus pakkumine on saadaval (vt [Turuplatsi turundusmaterjalide sisu juhend](marketplace-publishing-push-to-staging.md) Lisateavet) saab selle lepingu tellimine. Ei ole veel küsimustele.|
|**Peita ka selle lepingu keegi, kes pole kehtiv edendamine kood?**|Kui vastus eelmisele küsimusele on "Jah" on võimalus täielikult eemaldada see leping ei kuvataks kuvatakse Marketplace'ist UI. See tähendab, et külastajad ei kuvata pakkumise üksikasjade lehel see leping. Lõppkasutajad, mis saadetakse promocode, ostmiseks, saab see promocode abil tellida.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. luua oma turuplatsi turundustegevuste sisu
Kuidas sisestage nõutav teave **turundus, tugi ja hinnakirjad kategooriate** vahekaartidel palun külastada [Turuplatsi turundusmaterjalide sisu juhend](marketplace-publishing-push-to-staging.md) , mis on levinud kõik esemeid avaldatud Azure'i turuplatsilt.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. teie pakkumise ühenduse teenust (SQL Azure'i vastavalt või veebiteenus vastavalt).

Klõpsake menüü **Data Services** all.

Klõpsake lehe ülaservas pooles palutakse teil sisestada pakkumise **Namespace**.  

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.png)

Selle küsimuse all kuvatakse määratlete, kuidas avaldaja jätke vastloodud pakkumine Azure'i turuplatsi. (Lisateabe saamiseks vt [Andmete teenuste tehniline eelnevalt nõutud juhend](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Andmebaasi vastavalt teenuse avaldamine**

Klõpsake **andmebaasi**. Järgmisel lehel kuvatakse:

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.3.png)

Vastenduse andmekogumi aluseks on alla CSDL loomiseks SQL Azure'i DB:

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.4.png)

Ja seejärel iga tabeli jaoks

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.6.png)

Kui Web teenus

  ![Joonis](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Üksikasjalikud juhised ja näited loomine veebiteenuse alla CSDL lugeda [vastendamise olemasoleva OData kaudu alla CSDL veebiteenusest](marketplace-publishing-data-service-creation-odata-mapping.md) .

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete loonud oma andmete teenuse pakkumine, veenduge, et enne testimine [Teie andmete teenuse lavastus](marketplace-publishing-data-service-test-in-staging.md)lõpule [Turuplatsi turundusmaterjalide sisu juhendi](marketplace-publishing-push-to-staging.md) juhiseid.

## <a name="see-also"></a>Vt ka
- [Alustamine: Kuidas Azure'i turuplatsi pakkumise avaldamine](marketplace-publishing-getting-started.md)
- Kui olete huvitatud üldine OData vastenduse protsess ja otstarve, lugege seda artiklit [Andmete teenuse OData vastenduse](marketplace-publishing-data-service-creation-odata-mapping.md) üle vaadata määratlused, struktuurid ja juhiseid.
- Kui olete huvitatud Õppekeskuse ja teatud sõlmed ja nende parameetrid, lugege seda artiklit [Andmete teenuse OData vastendamise sõlmed](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) määratlused ja selgitused, näited ja kasutada juhul kontekstis.
- Kui olete huvitatud läbivaatamise näited, lugege seda artiklit [Andmete teenuse OData vastendamise näited](marketplace-publishing-data-service-creation-odata-mapping-examples.md) näha proovi kood ja koodi süntaksit ja konteksti mõistmine.


[link-pubportal]:https://publish.windowsazure.com
