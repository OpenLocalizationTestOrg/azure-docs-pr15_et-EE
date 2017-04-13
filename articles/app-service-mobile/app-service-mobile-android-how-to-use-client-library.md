<properties
    pageTitle="Kuidas kasutada Androidi Mobile'i rakendused kliendi teek"
    description="Kuidas kasutada Android klienti SDK Azure'i mobiilirakenduste kohta."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Kuidas kasutada Androidi kliendi teek mobiilirakenduste kohta

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Sellest juhendist näitab, kuidas kasutada Androidi SDK mobiilirakenduste rakendada Levinumad stsenaariumid, näiteks:

- Päringu andmete (lisamine, värskendamine ja kustutamine).
- Autentimist.
- Töötlemise tõrked.
- Kliendi kohandamine. 

Ka see suure-dive levinud kliendi koodi kasutatakse kõige mobiilirakendused.

Sellest juhendist keskendub kliendipoolne Android SDK.  Lisateavet serveripoolne SDK-d mobiilirakenduste kohta leiate teemast [töötamine .NET taustväärtus SDK] [ 10] või [Kuidas kasutada Node.js kirjutamata SDK][11].

## <a name="reference-documentation"></a>Dokumentides

Saate otsida [Javadocs API viide] [ 12] Android klienti teegi github.

## <a name="supported-platforms"></a>Toetatud platvormid

Azure'i Mobile'i rakendused Androidi SDK toetab API tasemed 19 kuni 24 (KitKat pähklimass kaudu).  

"Serveri rahavoogude" autentimise kasutab WebView esitatud UI. Kui seade ei ole võimalik esitada WebView UI, siis muid viise autentimine on vaja mis ei hõlma toote.  See SDK sobib ei Vaata-tüüpi või sarnaselt piiratud seadmetes.

## <a name="setup-and-prerequisites"></a>Häälestamise ja eeltingimused

Täitke [mobiilirakenduste Kiirjuhend](app-service-mobile-android-get-started.md) õpetuse.  See toiming tagab kõigi eeltingimused Azure Mobile rakenduste arendamise on täidetud.  Funktsiooni Kiirjuhend aitab teil konfigureerida oma konto ja looge oma esimese Mobile'i rakendus taustväärtus.

Kui te ei soovi lõpuleviimine Kiirjuhend õpetuse, tehke järgmist:

- [luua mobiilirakenduse taustväärtus] [ 13] oma Androidi rakendust kasutada.
- Androidi Studios [Gradle Koosta failid värskendada](#gradle-build).
- [Lubamine internet õigus](#enable-internet).

###<a name="gradle-build"></a>Faili Gradle koostamine

Mõlemad **build.gradle** failide muutmiseks tehke järgmist.

1. *Projekti* taseme **build.gradle** faili sees *buildscript* sildi järgmine kood lisamiseks tehke järgmist.

        buildscript {
            repositories {
                jcenter()
            }
        }

2. *Mooduli rakenduse* taseme **build.gradle** faili sees *sõltuvused* sildi järgmine kood lisamiseks tehke järgmist.

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Uusim versioon on praegu 3.1.0. Toetatud versioonid on loetletud [siin][14].

###<a name="enable-internet"></a>Lubage internet õigus

Azure'i juurdepääsu teie rakendus peab olema Interneti õigus, mis on lubatud. Kui see pole veel lubatud, lisage järgmine rida koodi **AndroidManifest.xml** faili:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Sügav sukelduda põhialused

Selles jaotises kirjeldatakse Kiirjuhend rakenduse saab Azure'i Mobile'i rakenduste kasutamise seotud koodi.  

###<a name="data-object"></a>Kliendi andmete tunnid määratlemine

SQL Azure'i tabelist pääsevad andmetele juurde, saate määrata kliendi andmete tunnid, mis vastavad tabelid Mobile'i rakendus taustväärtus. Selles teemas näited eeldavad tabelis nimega **ToDoItem**, mis sisaldab järgmisi veerge:

- ID
- teksti
- lõpuleviimine

Kliendipoolne objekti vastava tipitud:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Kood asub faili nimega **ToDoItem.java**.

Kui teie SQL Azure'i tabeli sisaldab rohkem veerge, tuleks vastavaid välju lisada see tund.  Näiteks kui soovitud DTO (andmete edastamine objekt) oli mõne täisarvu prioriteet veeru, siis võite lisada see väli koos oma juurde ja kinnitamiseks meetodite:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Saate teada, kuidas luua oma mobiilirakenduste kirjutamata täiendavaid tabeleid, lugege teemat [kohta: määratlemine tabeli domeenikontrolleri] [ 15] (.NET kirjutamata) või [määratleda tabelite abil dünaamiline skeemi] [ 16] (Node.js kirjutamata). Jaoks Node.js kirjutamata, saate kasutada **lihtne tabelite** säte [Azure portaali].

###<a name="create-client"></a>Kuidas: kliendi konteksti loomine

Järgmine kood tekitab **MobileServiceClient** objekti, mida kasutatakse teie mobiilirakenduse kirjutamata juurdepääsuks. Kood läheb selle `onCreate` **tegevuse** klassi **põhi** toimingu ja **KÄIVITI** kategooria *AndroidManifest.xml* määratud meetodit. Kiirjuhend kood, läheb **ToDoActivity.java** faili.

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

Asendage kood, `MobileAppUrl` oma Mobile'i rakendus kirjutamata URL, mis leiate [Azure'i portaalis] oma Mobile'i rakendus kirjutamata höövlitera. Selle rea koodi kompileerida, ka peate lisama **importimine** järgmine tekst:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Kuidas: tabeli viite loomine

Lihtsaim viis päringu või muuta selle kirjutamata andmed on abil *tipitud programmeerimise mudeli*sest Java on tugevalt tipitud keel. See mudel annab sujuvalt JSON seriaalsus, kasutades [ePoeg] [ 3] teegi taustväärtus Azure SQL-i kliendi objektide ja tabelite vahel andmete saatmisel.

Tabeli avamiseks kõigepealt looma on [MobileServiceTable] [ 8] objekti helistades on **getTable** meetodi [MobileServiceClient][9].  Sellel meetodil on kaks ülekoormuse.

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

Järgmine kood on **mClient** MobileServiceClient objekti viide.  Esimese ülekoormuse kasutatakse, kus selle tunni nime ja tabeli nimi on samad ja kasutatakse üks on Kiirjuhend:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Teine ülekoormuse kasutatakse tabelinime erineb klassinimi: esimene parameeter on tabeli nime.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Kuidas: andmete sidumiseks kasutajaliides

Andmetega sidumine koosneb kolmest osast:

- Andmeallikas
- Kuva paigutus
- Adapterit, mis ühendab kaks.

Meie proovi kood, saame tagastada andmed Mobile'i rakendused SQL Azure'i tabelist **ToDoItem** massiivi. See on levinud mustri andmete rakenduste.  Andmebaasi omapäringute tagastada sageli kogumi ridu, mida kliendi saab loendi või massiivist. Selles näites on massiiv andmeallika.

Koodi määrab ekraani paigutus, mis määratleb andmeid, mis kuvatakse vaate seadmes.  Kahe on seotud koos adapterit, mille järgmine kood laiendamine, on **ArrayAdapter&lt;ToDoItem&gt; ** klassi.

#### <a name="layout"></a>Kuidas: paigutuse määratlemine

Paigutuse on määratletud mitu pikad XML-i kood. Järgmine kood antud olemasoleva paigutuse, tähistab **loendivaade** soovime meie serveri andmetega asustada.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Eelmise koodi *listitem* atribuut määrab loendis kuvatakse üksikute rida paigutuse ID-d. Järgmine kood määrab märkeruudu ja seostatud teksti ja saab kord käivitatud loendis iga üksuse jaoks. See paigutus ei kuva välja **id** ja keerukamaid paigutuse täpsustab lisaväljade kuvamise. Järgmine kood on **row_list_to_do.xml** faili.

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Kuidas: selle adapterit määratlemine

Vaade meie andmeallika massiivi **ToDoItem**, kuna me alamklassi meie võrguadapteri kaudu on **ArrayAdapter&lt;ToDoItem&gt; ** klassi. See alamklassi toodab vaate iga **ToDoItem** **row_list_to_do** paigutus abil.

Meie koodi saate määratleda järgmised ainekursus laiendamine on **ArrayAdapter&lt;E&gt; ** klassi:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Alistada adapterit **getView** meetod. Näiteks:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Saame luua selle klassi eksemplar on meie järgmiselt:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Teine parameeter ToDoItemAdapter ehitaja on viide paigutust. Nüüd saame väärtustada **loendivaade** ja määrata funktsiooni-adapterit, **loendivaade**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>API struktuur

Mobiilirakenduste tabeli toimingud ja kohandatud API kõned on asünkroonne. Kasutage objektide [tulevikus] ja [AsyncTask] asünkroonne meetodid, päringute, lisab, värskendused ja kustutab. Tulevikku abil on hõlpsam ilma tegeleda mitme pesastatud kontekstiatribuuti tausta teemat mitme toimingu.

Vaadake üle **ToDoActivity.java** faili Androidi Kiirjuhend projekti [Azure portaali] näide.

#### <a name="use-adapter"></a>Kuidas: kasutada adapterit

Nüüd olete valmis kasutama andmete sidumine. Järgmine kood kuvatakse tabeli üksuste hankimine ja täidab kohaliku adapterit tagastatud üksuste.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Helistage-adapterit, et iga kord, kui muudate tabeli **ToDoItem** . Kuna kirje, kirje alusel on teinud muudatusi, mida toime ühe rea asemel kogumi. Kui lisate üksuse, kõne **lisamine** meetod adapterit; Kui kustutamine, helistage **eemaldamine** meetod.

##<a name="querying"></a>Kuidas: andmepäringuid oma kirjutamata Mobile'i rakendus

Selles jaotises kirjeldatakse, kuidas päringute anda kirjutamata Mobile'i rakendus, mis sisaldab järgmisi toiminguid:

- [Kõigi üksuste tagastamine]
- [Tagastatud andmete filtreerimine]
- [Tagastatud andmete sortimine]
- [Tagastab andmed lehtedel]
- [Valige soovitud veerud]
- [CONCATENATE päringu meetodid](#chaining)

### <a name="showAll"></a>Kuidas: tulu tabeli kõik üksused

Järgmine päring tagastab kõigi üksuste **ToDoItem** tabelis.

    List<ToDoItem> results = mToDoTable.execute().get();

*Tulemite* muutuja tagastab tulemi määrata päringu loendina.

### <a name="filtering"></a>Kuidas: filtri tagastatud andmeid

Järgmised päringu täitmise tagastab kõigi üksuste **ToDoItem** tabelist, kus **täieliku** väärtus on **false**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** on varem loodud mobiilsideteenuse tabeli viide.

Määratleda filtri, **kus** meetod kõne kasutamine tabeli viide. **Kui** meetodit järgneb **välja** meetodi, millele järgneb meetod, mis määrab loogika predikaat. Põhiliste võimalikust kaasata **eq** (võrdub), **ne** (ei võrdu), **gt** (suurem kui), **ge** (suurem kui või võrdne), **lt** (alla), **le** (väiksem või võrdne). Nende meetodite abil saate võrrelda kindlate väärtustega väljad arv ja string.

Saate filtreerida kuupäevad. Järgmistest meetoditest võimaldavad teil võrrelda kogu Kuupäevaväli või osade kuupäeva: **aasta**, **kuu**, **päev**, **tunni**, **minuti**ja **teine**. Järgmises näites liidetakse üksused, mille *tähtaeg* võrdub 2013 filter.

    mToDoTable.where().year("due").eq(2013).execute().get();

Järgmistest meetoditest keerukate filtrid toetavad stringi väljade: **startsWith**, **endsWith**, **concat**, **alamstringi**, **indexOf**, **asendada**, **toLower**, **toUpper**, **trimmimine**ja **pikkus**. Järgmises näites filtreeritakse tabeli read, kus *tekstiveeru* algab selgitava lausega "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Tehtemärk järgmistest toetatud arv väljade: **lisamine**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**ja **ümardada**. Järgmises näites filtreerib tabeli ridade kui **kestus** on paarisarv.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Saate kombineerida predicates loogilise viiside: **ja**, **või** ja **ei**. Järgmises näites ühendab kaks eelmistes näidetes.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Rühmitamine ja pesastada loogika tehtemärke.

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Täpsema teabe ja filtreerimise näited leiate teemast [uurimine rikkust Androidi kliendi päringu mudel](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Kuidas: sortimine, tagastatakse andmed

Järgmine kood tagastab kõiki üksusi tabelist **ToDoItems** sorditud tõusvas järjestuses *teksti* välja alusel. *mToDoTable* on varem loodud taustväärtus tabeli viide:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Esimese parameetrina **orderBy** meetodi on string, mis on võrdne sortimise aluseks välja nimi. Teine parameeter kasutab **QueryOrder** loendamine, et määrata, kas soovite sortida tõusvas või laskuvas järjestuses.  Kui filtreerite, ***kus*** meetodi abil, ***kus*** meetodit tuleb kasutada enne ***orderBy*** meetod.

### <a name="paging"></a>Kuidas: andmeid tagastada lehtedel

Esimese näites kujutatakse ülemise viis üksuste valimiseks tabelist. Päring tagastab üksused, **ToDoItems**tabelist. **mToDoTable** on varem loodud taustväärtus tabeli viide:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Siin on päring, mis ignoreerib esimesel viiel üksused ja tagastab järgmise viie.

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Kuidas: valige soovitud veerud

Järgmine kood on näidatud, kuidas tagasi kõigi üksuste **ToDoItems**tabelisse, kuid kuvatakse ainult need väljad **lõpule viidud** ja **tekst** . **mToDoTable** on varem loodud taustväärtus tabeli viide.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Valige funktsiooni parameetrid on string nimed tabeli veerud, mida soovite taastada.

**Valige** meetod peab meetodid, näiteks **kui** ja **orderBy**. Saate järgida Piipar viisil nagu **algusse**.

### <a name="chaining"></a>Kuidas: Concatenate päringu meetodid

Päringute taustväärtus tabelite moodust saab liitsõnumeid. Päringu meetodite Aheldamise võimaldab valida teatud Välisandmeveergude filtreeritavaid ridu, mis on sorditud ja leheküljed. Saate luua keerukate loogilise filtrid.
Iga meetodi päring tagastab päringu objekti. Võimalused sarja lõpetada ja tegelikult päringut, helistage **käivitada** meetod. Näiteks:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Aheldatud päringu meetodid tuleks tellida järgmiselt:

1. Filtreerimine (**kus**) viise.
2. Sortimise viise (**orderBy**).
3. (**Valige**) valiku meetodid.
4. Piipar (**vahele jätta** ja **üleval**).

##<a name="inserting"></a>Kuidas: andmete lisamiseks soovitud taustväärtus

Väärtustada *ToDoItem* klassi eksemplar ja määrake selle atribuute.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Klõpsake objekti lisamine **insert()** abil:

    ToDoItem entity = mToDoTable.insert(item).get();

Tagastatud üksuse vasteid kirjutamata tabelisse lisatud andmed sisalduv ID-d ja muud väärtused, mis on määratud selle taustväärtus.

Mobiilirakenduste tabelite jaoks on vaja mõnda primaarvõtme veerg nimega **id**. Vaikimisi on see veerg string. Veeru ID vaikeväärtus on GUID.  Saate sisestada muude kordumatuid väärtusi, nt meiliaadressid või kasutajanimed. Kui stringiväärtus ID-d ei ole ette on lisatud kirje, kuvatakse taustväärtus loob uue GUID.

Stringi ID väärtused pakuvad järgmised eelised:

+ ID-d saab luua andmebaasi edasi-tagasi tegemata.
+ Kirjete on lihtsam ühendada erinevate tabelite või andmebaasidest.
+ Paremini integreerida rakenduse loogika ID väärtused.

Stringi ID väärtused on **nõutav** võrguühenduseta sünkroonimise tugi.

##<a name="updating"></a>Kuidas: mobiilirakenduses andmete värskendamine

Tabeli andmete värskendamiseks läheb uuele objektile **kutsuda** meetodit.

    mToDoTable.update(item).get();

Selles näites on *üksuse* viite rea *ToDoItem* tabelis, mis on olnud teatud muudatustele.
Sama **id** -ga rida on värskendatud.

##<a name="deleting"></a>Kuidas: mobiilirakenduses andmete kustutamine

Järgmine kood kuvatakse andmete kustutamine tabelist, määrates andmete objekti.

    mToDoTable.delete(item);

Saate ka kustutada üksuse, määrates **id** -välja rea kustutamine.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Kuidas: otsida mõnda kindlat üksust

Otsida kindlate **id** väljad üksuse **lookUp()** meetod:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Kuidas: untyped andmetega töötamine

Untyped programmeerimise mudel annab teile JSON sariväljaanne täpne kontroll.  Mõne levinud stsenaariumi, kus võiksite kasutada untyped programmeerimise mudelit, mis on. Näiteks kui teie taustväärtus tabel sisaldab palju veerge ja soovite viidata veergude alamhulga.  Tipitud mudel nõuab teil määrata kõik mobiilirakendused tabeli veergude oma andmeid tunni.  

Enamik API kõned andmetele juurdepääsuks on sarnane tipitud programmeerimise kõnesid. Peamine erinevus on untyped mudeli saate autonoomsest meetodite objektil **MobileServiceJsonTable** asemel **MobileServiceTable** objekti.

### <a name="json_instance"></a>Kuidas: eksemplari untyped tabeli loomine

Sarnane tipitud mudeli alustamist saada tabeli viide, kuid sel juhul on **MobileServicesJsonTable** objekti. Saada viide helistades on **getTable** meetod, näiteks kliendi:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Kui olete loonud **MobileServiceJsonTable**eksemplari, on peaaegu sama API tipitud programmeerimise mudeliga saadaval. Mõnel juhul teha meetodite asemel tipitud parameeter on untyped parameeter.

### <a name="json_insert"></a>Kuidas: untyped tabelisse lisamine

Järgmine kood näitab, kuidas lisada. Kõigepealt tuleb luua mõne [JsonObject][1], mis on osa [ePoeg] [ 3] teek.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Seejärel kasutage **insert()** untyped objekti lisamine tabelisse.

    mJsonToDoTable.insert(jsonItem).get();

Kui vajate lisatud objekti ID-d, kasutage **getAsJsonPrimitive()** meetodit.

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Kuidas: untyped tabelist kustutamine

Järgmine kood näitab, kuidas kustutada eksemplari, sel juhul **JsonObject** näide eelnevate *lisada* loodud samas eksemplaris. Kood on sama mis tipitud juhtum, kuid meetod on erineva signatuuri, kuna see viitab mõni **JsonObject**.

         mToDoTable.delete(jsonItem);

Näiteks saate kustutada ka otse oma ID abil:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Kuidas: tagastada untyped tabelist kõik read

Järgmine kood näitab, kuidas tuua terve tabeli. Kuna kasutate JSON tabeli, saate valikuliselt vaid mõne tabeli veergude tuua.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Sama valiku filtreerimine, filtreerimine ja Piipar viise, mida on saadaval tipitud mudeli on saadaval untyped mudeli.

##<a name="custom-api"></a>Kuidas: kohandatud API kõne

Kohandatud API võimaldab määratleda kohandatud lõpp-punktid mis edastavad serveri funktsioonid, mis pole vastendamine lisada, värskendada, kustutada või lugeda toiming. Kohandatud API abil saate määrata rohkem kontrolli sõnumside, sh lugemine ja HTTP sõnumipäiste seadmine ja sõnumi keha vorming peale JSON määratlemine.

Androidi klientrakenduses helistate **invokeApi** meetod helistamiseks kohandatud API lõpp-punkti. Järgmises näites kujutatakse API lõpp-punkti **completeAll**, mis tagastab saidikogumi klassi nimega **MarkAllResult**nimega helistada.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

**InvokeApi** meetodit nimetatakse klient, mis saadab postituse uue kohandatud API. Kohandatud API Tagastatud tulem kuvatakse sõnumi dialoogiboksis on vigu. Muude versioonide **invokeApi** abil saate soovi korral objekti saata koosolekukutse kehasse, määrake HTTP meetod ja saatke kutse päringu parameetrite. Saadaval on ka **invokeApi** untyped versioone.

##<a name="authentication"></a>Kuidas: autentimine lisamiseks rakenduse

Õpetused juba kirjeldavad üksikasjalikult nende funktsioonide lisamine.

Teenuse rakendus toetab [rakenduse kasutajad autentimist](app-service-mobile-android-get-started-users.md) kasutades erinevaid välise identiteedipakkujad: Facebook, Google, Microsoft Account, Twitteri ja Azure Active Directory. Õigusi saab seada tabelites piirata juurdepääsu teatud toimingute ainult autenditud kasutajad. Autenditud kasutaja ID abil rakendada autoriseerimine reeglid oma taustväärtus.

Kahe autentimise puhul on toetatud: **serveri** kulgemist ja **Kliendi** kulgemist. Serveri voogu pakub lihtsaim autentimise versioon, kuna see põhineb identiteedi pakkujate web kasutajaliidese.  Meilivoo autentimine rakendamiseks vajalikke pole täiendavad SDK-d. Meilivoo autentimine ei paku sügav integreerimine mobiilsideseadme või on soovitatav ainult tõendada mõistet stsenaariumid.

Kliendi voogu võimaldab põhjalikult integreerimine seadme võimalusi, nt ühekordse sisselogimise see sõltub identiteedi pakkuja SDK-d.  Näiteks saate Facebooki SDK integreerida mobiilsideseadmete rakenduse.  Mobiilside kliendi vahetustehingute rakendusse Facebooki ja kinnitab teie sisselogimise enne tagasi, et teie mobiilirakenduse vahetamine.

Rakenduse autentimise on nõutav neli toimingut:

- Identiteedi pakkuja rakenduse autentimise registreerida.
- Konfigureerige oma rakenduse teenuse taustväärtus.
- Rakenduse teenuse kirjutamata ainult autenditud kasutajad tabeli õiguste piiramine.
- Rakenduse autentimise koodi lisada.

Õigusi saab seada tabelites piirata juurdepääsu teatud toimingute ainult autenditud kasutajad. Autenditud kasutaja SID abil saate muuta kutsed.  Lisateabe saamiseks vaadake [Alustamine autentimist] ja serveri SDK juhendid dokumentatsiooni kohta.

### <a name="caching"></a>Kuidas: rakenduse autentimise koodi lisamine

Järgmine kood alguses serveri meilivoo login protsessi Google abil.

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Sisselogitud kasutaja ID, hangib **MobileServiceUser** **getUserId** meetodi abil. Näide tulevikku abil saate helistada asünkroonne login API-de kohta teemast [Alustamine autentimine].

### <a name="caching"></a>Kuidas: autentimine märkide vahemälu

Vahemällu autentimine märkide peab teil salvestada kasutaja ID ja autentimise luba seade. Järgmine kord, kui rakendus käivitub, saate vaadata vahemälu ja kui need väärtused on olemas, saate log in toiming vahele jätta ja rehydrate kliendi andmetega. Andmed on siiski tundliku ja hoida krüptitud turvalisuse huvides juhuks, kui telefon varastatakse.

Näete täieliku näide sellest, kuidas soovite vahemälu autentimine märkide [vahemälu autentimise sõned]jaotises[7].

Kui proovite kasutada on aegunud luba, saate *401 puuduvad volitused* vastuse. Te saate vigade autentimise filtrite abil.  Filtrite intercept taotluste rakenduse teenuse taustväärtus. Filtri koodi kontrollib 401 vastus, käivitab protsessi Logi sisse ja seejärel uuesti soovitud 401 genereerinud taotluse. 

## <a name="adal"></a>Kuidas: autentida Active Directory Authentication Library kasutajad

Active Directory autentimise Raamatukogu (ADAL) abil saate sisse logida kasutades Azure Active Directory rakenduse kasutajad. Kliendi meilivoo sisselogimise kasutamine on sageli kasutatakse funktsiooni `loginAsync()` meetodid, sest see pakub rohkem kohalikke Kasutuskogemuse olemus ja võimaldab täiendav kohandamine.

1. Järgides, [Kuidas konfigureerida rakenduse teenus Active Directory login](app-service-mobile-how-to-configure-active-directory-authentication.md) õpetuse konfigureerimine oma mobiilirakenduse kirjutamata jaoks AAD Logi sisse. Veenduge, et kohustuslik toiming omakliendi rakenduse registreerimisel.

2. Installige ADAL, muutes oma build.gradle faili kaasata järgmisi.

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Rakenduse, tehes järgmist asendused järgmine kood lisamiseks tehke järgmist.

* Asendage **Lisa-asutus – siin** rentniku, kus te ette valmistatud rakenduse nimi. Vormingu peaks olema https://login.windows.net/contoso.onmicrosoft.com. See väärtus saab kopeerida domeeni menüüs Azure Active Directory [Azure klassikaline portaalis].

* Asendage **Lisa-RESSURSI ID siin** kliendi ID jaoks oma mobiilirakenduse taustväärtus. Kliendi ID saate hankida mõnest portaalis jaotises **Azure Active Directory sätted** vahekaarti **Täpsemalt** .

* Asendage **Lisa-kliendi ID siin** omakliendi rakendusest kopeeritud kliendi ID-ga.

* Asendage **Lisa-REDIRECT URI siin** oma saidi _/.auth/login/done_ lõpp-punkti, kasutades HTTPS värviskeemi. See väärtus peab olema _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Kuidas: tõuketeatised teatis lisamiseks rakenduse

Saate [siit ülevaate] [ 6] , mis kirjeldab, kuidas Microsoft Azure'i teatis jaoturi toetab mitmesuguseid Tõuketeatiste.  [Selles]õpetuses[5], saadetakse teatis tõuketeatised kõigis seadmetes iga kord, kui kirje lisamisel.

## <a name="how-to-add-offline-sync-to-your-app"></a>Kuidas: ühenduseta sünkroonimine lisamiseks rakenduse

Kiirjuhend õppeteema sisaldab koodi, mis rakendab ühenduseta sünkroonimine. Otsige koodi eesliide kommentaarid.

    // Offline Sync

Järgmised koodiread uncommenting saaksite ühenduseta sünkroonimine ja sarnase kood saate lisada muid mobiilirakenduste kood.

##<a name="customizing"></a>Kuidas: kliendi kohandamine

On teil kohandada vaikekäitumine kliendi jaoks mitu võimalust.

### <a name="headers"></a>Kuidas: kohandada päiste taotlemine

Saate konfigureerida kohandatud HTTP päise lisamiseks iga taotluse **ServiceFilter** .

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Kuidas: sariväljaanne kohandamine

Kliendi eeldab, et andmete tabelite nimed, veerunimede ja tipib klõpsake soovitud kirjutamata kõik match täpselt andmeobjektid määratletud klientrakenduses. Võib olla mis tahes arv põhjust, miks ei kattu serveri ja kliendi nimed. Teie stsenaarium, võite teha järgmist tüüpi kohandused:

- Rakenduse teenuse tabeli veergude nimed ei ühti kasutate kliendi nimed.
- Kasutage rakendust Service tabeli, mis on mõni muu nimi, kui see kaardid klientrakenduses ainekursust.
- Suurtähestuse automaatne atribuudi sisse lülitada.
- Keerukate atribuutide lisamine objektile.

### <a name="columns"></a>Kuidas: kaart erinevad kliendi- ja nimed

Oletame, et teie Java kliendi kood kasutab standard Java laadis nimede **ToDoItem** objekti atribuudid, näiteks järgmised atribuudid:

- mId
- mText
- mComplete
- mDuration

Kliendi nimed serialiseerida JSON nimed, mis vastavad serveris **ToDoItem** tabeli veergude nimed sisse. Järgmine kood kasutab [ePoeg] [ 3] atribuutide marginaalide lisamiseks teeki:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Kuidas: vastendamine erinevate tabelinimede kliendi ja selle taustväärtus vahel

Vastendage kliendi tabeli nime erinevate mobiilsideseadmete teenuste tabeli nimi, kasutades mõnda alistada, [getTable()] [ 4] meetod:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Kuidas: automatiseerida veeruvastenduste nimi

Saate määrata teisendamise strateegia, mis kehtib iga veeru abil [ePoeg] [ 3] API. Androidi kliendi teek kasutab [ePoeg] [ 3] serialiseerida JSON andmete Java objektide enne andmete saadetakse Azure'i rakendust Service taustal.  Järgmine kood kasutab strateegia **setFieldNamingStrategy()** meetod. Selles näites kustutab algse märgi ("m"), ja seejärel väljal järgmisele märgile, iga välja nime. Näiteks see muutuks "mId" "id".

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Järgmine kood tuleb käivitada enne **MobileServiceClient**abil.

### <a name="complex"></a>Kuidas: talletamiseks soovitud objekti või massiivi atribuudi tabelisse

Seni meie sariväljaanne näidetes on seotud lihtsad tüübid, nt täisarvude ja stringide.  Lihtsad tüüpi hõlpsalt serialiseerida JSON sisse.  Kui soovime lisada keerulise objekti, mis ei automaatselt serialiseerida JSON, läheb vaja JSON sariväljaanne meetodit.  Näide sellest, kuidas kohandatud JSON sariväljaanne pakkuda vaatamiseks vaadake ajaveebipostituse [määramine sariväljaanne ePoeg teegi Mobile Androidi Services kliendi][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Kõigi üksuste tagastamine]: #showAll
[Tagastatud andmete filtreerimine]: #filtering
[Tagastatud andmete sortimine]: #sorting
[Tagastab andmed lehtedel]: #paging
[Valige soovitud veerud]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure'i portaal]: https://portal.azure.com
[Alustamine autentimine]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Tulevikus]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html