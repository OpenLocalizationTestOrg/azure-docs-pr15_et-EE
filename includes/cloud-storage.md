#<a name="data-management-and-business-analytics"></a>Andmete haldamine ja ärianalüüsi

Haldamine ja pilveteenuses andmete analüüsimine on sama oluline, et see on mujal. Selleks, et teil seda teha, Azure pakub mitmesuguseid tehnoloogiad relatsiooniline ja relatsiooniline andmetega töötamiseks. Selles artiklis tutvustatakse valikute kohta. 

##<a name="table-of-contents"></a>Sisukord      

- [Bloobimälu](#blob)
- [Virtuaalse masina DBMS töötab?](#dbinvm)
- [SQL-andmebaas](#sqldb)
    - [SQL-i andmete sünkroonimine](#datasync)
    - [SQL-i andmete aruannete abil Virtuaalmasinates](#datarpt)
- [Tabelimälu](#tblstor)
- [Hadoopi](#hadoop)

## <a name="blob"></a>Bloobimälu

On sõna "bloobimälu" lühike "kahendarvu suur objekt" ja see kirjeldab täpselt mis bloobimälu on: kahendarvu teabe kogumine. Veel isegi juhul, kui need on lihtne, plekid on väga kasulik. [Joonisel 1](#Fig1) on näidatud Azure'i bloobimälu põhitõdesid.

<a name="Fig1"></a>![Diagrammi plekid][blobs]
 
**Joonis 1: Azure'i bloobimälu talletatakse binaarandmeid - plekid - ümbriste.**

Plekid kasutamiseks loomisel Azure *storage konto*. Osa, saate määrata Azure andmekeskuses, mis salvestab objektide loomist, kasutades selle konto. Kõikjal, kus see asub, kuulub iga bloobimälu loote mõned container salvestusruumi konto. Mõne bloobimälu juurdepääsu rakenduse pakub URL-i vorm.

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*Container*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; on määratud salvestusruumi uue konto loomisel kordumatu ajal &lt; *Container* &gt; ja &lt; *BlobName* &gt; on teatud container ja selle pakendi sees on bloobimälu nimed. 

Azure'i pakub kahte erinevat tüüpi plekid. Valikud on:

- *Blokeeri* plekid, mis võib sisaldada kuni 200 GB andmeid. See nimi viitab, Blokeeri bloobimälu on jagatud teatud arvu plokid. Tõrke ilmnemisel edastamise blokeerimine bloobimälu ajal jätkamiseks originaaldokumendi viimase blokeerimine, mitte kogu bloobimälu uuesti saata. Blokeeri plekid on üsna üldine lähenemine salvestusruumi ja need on kõige sagedamini kasutatav bloobimälu tüüp täna.

- *Lehe* plekid, mis võib olla võimalikult suur ühe Teratavu juures. Lehe plekid on mõeldud muutmälu ja seega iga on mõned lehekülgede arv jagatud. Rakendus on tasuta lugeda ja kirjutada üksikuid lehti juhusliku funktsiooni bloobimälu. Azure'i Virtuaalmasinates näiteks VMs loomisel kasutage lehe plekid püsiv hoidla OS ketast nii andmete ketast.

Kas valida Blokeeri plekid või lehe plekid, pääsevad rakendused bloobimälu andmete mitmel erineval viisil. Järgmised suvandid.

- Otse soovitud RESTful (st HTTP-põhine) juurdepääsu protokolli. Azure'i rakenduste nii välistes rakendustes, sh rakenduste kohapealne, kus töötab saate kasutada seda suvandit.
- Kasutades Azure salvestusruumi kliendi teek, mis pakub rohkem arendaja sõbralik kasutajaliidese peal töötlemata rahulik bloobimälu access protocol. Veel kord nii Azure rakendused ja välistes rakendustes juurdepääsu plekid selle teegi abil.
- Azure'i draivid suvand, mis võimaldab lehe bloobimälu käsitleda kohalikule kettale abil failisüsteemi NTFS Azure rakenduse kasutamine. Rakenduse, et lehe bloobimälu näeb tavapärase Windowsi failisüsteemis, standard faili I/O abil. Tegelikult loeb ja kirjutab saadetakse aluseks oleva lehe bloobimälu, mis rakendab Azure'i ketas. 

Kaitsevad riistvara tõrkeid ja parandada kättesaadavus, on iga bloobimälu kopeeritud kolme arvutisse on Azure andmekeskuses. Kirjalikult on bloobimälu värskendab kõik kolm eksemplari, nii, et uuemad loeb ei kuvata tabelites sisalduvas tulemused. Saate määrata, et on bloobimälu andmed kopeerida teise Azure andmekeskuse sama geo, kuid vähemalt 500 miili. Selle kopeerimist, nimetatakse *geo-dispersioonanalüüs*, juhtub värskendust mõne minuti jooksul soovitud bloobimälu ja see on kasulik avariitaastet.

Andmete plekid teha ka Azure *Sisu kohaletoimetamise võrk (CDN)*kaudu. Vahemällu bloobimälu andmete serverid kogu maailmas kümneid koopiad, on CDN saate kiirendada juurdepääsu teabele, mis on mitu korda tabeldusklahvi. 

Need on lihtne, plekid on õige valik paljudel juhtudel. Salvestamise ja streaming video ja heli on selge näidetes on varukoopiate ja muud tüüpi andmete arhiivimine. Arendajad saate kasutada ka struktureerimata andmed neile meeldib igasuguse plekid. Kas teil on lihtne viis talletada ja juurdepääs binaarandmeid võib olla kasulik üllatavalt.


## <a name="dbinvm"></a>Virtuaalse masina DBMS töötab?

Paljud rakendused sõltuvad täna mingi andmebaasi süsteemi (DBMS). Relatsiooniline nt SQL Server on kõige sagedamini kasutatavaid valik, kuid -relatsiooniline lähenemisel, *NoSQL* tehnoloogiate tuntakse saada populaarsemaks iga päev. Selleks, et pilv rakendusi kasutada andmete haldamise suvanditest, Azure'i Virtuaalmasinates võimaldab käivitada DBMS (relatsiooniline või NoSQL) VM. [Joonis 2](#Fig2) näitab, kuidas see näeb välja SQL serveri.

<a name="Fig2"></a>![SQL Server virtuaalse masina skeem][SQLSvr-vm]
 
**Joonis 2: Azure'i Virtuaalmasinates võimaldab töötab DBMS VM, mida plekid püsimine.**

Arendajad ja andmebaasi administraatorid sel juhul näeb palju töötavad sama tarkvara oma andmekeskuse. Siinse näide, näiteks saab kasutada peaaegu kõigi SQL serveri võimaluste ja teil on täielik juurdepääs süsteem. Teil on ka haldamise andmebaasiserveriga muidugi, just nagu siis, kui see ei tööta kohalikult.

[Joonis 2](#Fig2) näitab, kuvatakse teie andmebaasidest talletamise kohalikule kettale VM server töötab. All hõlmab, kuid iga nende ketast kirjutatakse mõne Azure'i bloobimälu. (See on sarnane funktsiooniga on SAN on toimides palju, nagu on LUN bloobimälu oma andmekeskuses.) Nagu mis tahes Azure'i bloobimälu, kus see sisaldab andmeid on kopeeritud kolm korda sees andmekeskuses, ja, kui seda geo kopeeritud teise andmekeskuse piirkonna. Samuti on võimalik kasutada näiteks SQL serveri andmebaasi peegeldus jaoks täiustatud töökindlus. 

Teine võimalus kasutada SQL serveri VM on luua hübriid rakenduse, kus andmed elu Azure ajal loogika rakendus töötab kohapealse. Näiteks see võib mõistlik kui rakenduste töötamine mitmes asukohas või erinevate mobiilsideseadmete jaoks peab andmete ühiskasutusse anda. Suhtlemine cloud andmebaas ja kohapealse loogika lihtsam teha ettevõtte saate kasutada Azure virtuaalse võrgu on Azure andmekeskuse ja oma kohapealse andmekeskuse vahelise virtuaalse privaatvõrgu (VPN) ühenduse loomiseks.


## <a name="sqldb"></a>SQL-andmebaas

Paljude inimeste jaoks, töötavad DBMS VM on esimene suvand, mis tuleb arvesse pilveteenuses Liigendatud andmete haldamise. See on mitte ainult valik küll, samuti on alati parim valik. Mõnel juhul Service (PaaS) lähenemisviisi platvormi abil andmete haldamine muudab loogilisem. Azure'i pakub PaaS tehnoloogia SQL-andmebaasiga, mis võimaldab teil seda teha relatsiooniliste andmete jaoks. [Joonis 3](#Fig3) illustreerib see suvand. 

<a name="Fig3"></a>![SQL-andmebaasi diagramm][SQL-db]
 
**Joonis 3: SQL-andmebaasi pakub ühiskasutusega PaaS relatsiooniline salvestusteenus.**

SQL-andmebaasi ei anna iga kliendi oma füüsilise SQL serveri eksemplar. Selle asemel pakub mitme rentniku teenus, loogilise SQL-andmebaasi server iga kliendi jaoks. Kõik kliendid jagada Arvuta ja salvestusruumi võimsus, mille pakutav teenus. Ja sarnaselt bloobimälu, SQL-andmebaasis kõik andmed on salvestatud kolm eraldi arvutitesse Azure andmekeskuses, andes teie andmebaasidest sisseehitatud kõrge kättesaadavus (HA).

Rakendus SQL-andmebaasi näeb palju SQL serveri. Rakenduste probleemi vastu relatsiooniline tabelid SQL-päringud, kasutage T-SQL-i salvestatud toimingute ja käivitada tehingud üle mitme tabeli. Ning kuna rakenduste juurde SQL-andmebaasi tabeli voo (TDS) protokolli, SQL Server, kasutada sama protokolli abil saate üksuse raames, ADO.net-i, JDBC ja muud tuttavad andmed Accessi kasutajaliideste kasutamine andmete. 

Kuid kuna SQL-andmebaas on pilveteenus Azure andmekeskuste töötab, ei pea haldamiseks, mis tahes süsteemi füüsilise aspekte, näiteks kettaruumi kasutus. Ka ei pea muretsema tarkvara värskendamine või muude madala haldustoiminguid töötlemise. Iga kliendi ettevõtte juhtelementide endiselt oma andmebaasid muidugi skeeme ja kasutajale sisselogimise, kuid paljude Ilmalik haldustoiminguid on tehtud. 

Kui SQL-andmebaasi näeb palju SQL serveri rakendustele, seda ei käitu täpselt sama, mis töötab füüsilise või virtuaalse masina DBMS. Kuna see töötab ühiskasutusega riistvara, erinevad selle täitmine selle riistvara paigutatakse kõik klientide laadi. See tähendab, et täitmise, öelge salvestatud protseduur SQL-andmebaasis võib erineda üks päev järgmise. 

Täna, SQL-andmebaasi saate luua kuni 150 gigabaiti hoidke andmebaasi. Kui teil on vaja suuremat andmebaasidega töötamine võimaldab teenuse suvandi *Federation*. Selle tegemiseks oma andmebaasi administraatorilt loob kahe või enama *federation liikmed*, millest igaüks on oma skeemi eraldi andmebaasi. Andmed on laiali midagi, mida sageli nimetatakse *sharding*, koos iga liikme määratud kordumatu *federation klahvi*liikmed. Rakenduse probleemide SQL-i päringute suhtes, määrates federation klahvi, mis tuvastab federation liikme päringu andmed tuleks suunata. See võimaldab suurte andmehulkade traditsiooniline relatsiooniline lähenemisviisi abil. Nagu alati, on kompromisse; päringute ega tehingud võib hõlmata federation liikmed, näiteks. Kuid kui relatsiooniline PaaS teenus on parim valik ja nende kompromisse on lubatud, kasutades SQL-i Federation saab hea lahendus.

SQL-andmebaasi saate kasutada rakenduste töötamine Azure'i või mujal, näiteks teie asutusesisese andmekeskuse. See muudab pilve rakendused, mida on vaja relatsiooniandmete kasulik, kui ka kohapealse rakendused, mida saab kasutada andmete salvestamine pilves. Mobiilirakenduse võib toetuvad SQL-andmebaasi juhtida ühiskasutuses relatsiooniliste andmete, näiteks võib varude rakendus, mis töötab mitu edasimüüjate kogu maailmas.

SQL-andmebaasi mõtlema tõstatab küsimuse selge (ja olulised): peaks käivitamisel SQL serveri VM ja millal on SQL-andmebaasi parem valik? Nagu tavaliselt, kompromisse on ja millist lähenemisviisi on parem nii sõltub teie vajadustele. 

Üks lihtne viis mõelda on SQL-andmebaasi on uue rakenduste, SQL Server VM on parem valik olemasoleva kohapealse rakenduse pilveteenusesse liikudes kuvamiseks. See võib olla kasulik vaadata seda otsust veel kohandatud viisil, kuid. Näiteks SQL-andmebaasi lihtsam on kasutada, kuna suhteliselt vähesele häälestamine ja haldamine. Kuid töötab SQL serveri VM võib olla nõuaksid vähem lisatööd jõudlus – pole ühiskasutusega teenuse - ja samuti toetab suurem kui SQL-andmebaasi andmebaaside ühendamata. Siiski, SQL-andmebaasi pakub sisseehitatud dispersioonanalüüs nii andmete ja töötlemine, tõhus annab teile kõrge-saadavus DBMS väga vähe tööd. Kui SQL serveri annab teile rohkem võimalusi ja mõnevõrra laiemast suvandid, SQL-andmebaasi on seadistatud haldamiseks ja oluliselt vähem tööd.

Lõpetuseks, on oluline märkida, et SQL-andmebaasi pole ainult PaaS andmeteenuse Azure saadaval. Microsofti partnerite pakuvad ka muid võimalusi. Näiteks ClearDB pakub MySQL-i PaaS, mis pakub, samas Cloudant müüb NoSQL suvand. PaaS andmeteenuste on õige lahendus paljudel juhtudel ja nii andmehalduse See lähenemine on oluline osa Azure.


### <a name="datasync"></a>SQL-i andmete sünkroonimine

Ajal SQL-andmebaasi haldamine kolm eksemplari iga andmebaasi ühe Azure andmekeskuse sees, ei korrata andmete automaatselt Azure andmekeskuste vahel. Selle asemel pakub SQL-i andmete sünkroonimine, teenuse, mille abil saate seda teha. [Joonisel 4](#Fig4) on näidatud, kuidas see välja näeb.

<a name="Fig4"></a>![SQL-i andmete sünkroonimine skeem][SQL-datasync]
 
**Joonis 4: SQL-i andmete sünkroonimine andmete muude Azure SQL andmebaasis asuvate andmete sünkroonib ja kohapealse andmekeskuste.**

Nagu joonisel on kujutatud, SQL-i andmete sünkroonimine saab sünkroonida andmeid mitmest eri kohti. Oletame, et teie arvutis rakenduse mitme Azure'i andmekeskuste, näiteks SQL-andmebaasis talletatud andmed. SQL-i andmete sünkroonimine abil saate sünkroonida andmeid hoida. SQL-i andmete sünkroonimine saab sünkroonida ka andmeid on Azure andmekeskuse ja töötab ka asutusesisese andmekeskuse SQL serveri eksemplar. See võib olla kasulik säilitada nii andmete kohapealse rakendused kohaliku eksemplari ja rakendused töötavate Azure pilveteenuse koopia. Ja kuigi see on näidatud joonisel, SQL-i andmete sünkroonimine saate kasutada ka SQL-andmebaas ja SQL Server töötab VM Azure või mujalt vaheline andmete sünkroonimine.

Sünkroonimine võib olla kahesuunalise ja olete kindlaks teinud, täpselt, millised andmed on sünkroonitud ja kui sageli seda. (Andmebaaside sünkroonimiseks pole aatomi, aga - on alati vähemalt mõned viivitus) Aga seda kasutatakse, SQL-i andmete sünkroonimine sünkroonimise häälestamise on täiesti konfiguratsiooni põhinev; ei ole koodi kirjutamist.


### <a name="datarpt"></a>SQL-i andmete aruannete abil Virtuaalmasinates

Kui andmebaas sisaldab andmeid, keegi tõenäoliselt soovite andmeid kasutava aruannete loomine. Azure'i käitamise SQL serveri aruandlusteenused (SSRS) Azure'i Virtuaalmasinates mis on taastatud võrdväärse asutusesisese SQL serveri aruandlusteenuste töötab. Seejärel saate SSRS aruannete käitamist Azure SQL-andmebaasis talletatud andmed.  [Joonis 5](#Fig5) näitab, kuidas toimub.

<a name="Fig5"></a>![SQL-i aruandluse skeem][SQL-report]
 
**Joonis 5: SQL serveri aruandlusteenuste töötab rakenduses on Azure'i Virtuaalmasinates pakub aruandlusteenuste SQL-andmebaasis andmete jaoks. .**

Enne, kui kasutaja saab aruande vaatamiseks kellegi määratleb, mis see aruanne peaks välja nägema (samm 1). SSRS VM kohta, kus saate seda teha ühte kahest tööriistade abil: SQL Server Data Tools, SQL Server 2012 osa või kaasab Business Intelligence (BI) arengu Studio. Sarnaselt SSRS, on need aruande määratlused väljendatud rakenduses aruande definitsiooni keele (RDL). Pärast aruande RDL failid on loodud, nad on üles laaditud VM pilves (samm 2). Aruande definitsiooni on kasutamiseks valmis.

Järgmiseks rakenduse kasutaja pääseb juurde aruanne (samm 3). Rakendus edastab selle taotluse SSRS VM (samm 4), mis loob ühenduse SQL-andmebaasi või muude andmeallikate andmed saada (juhis 5). SSRS kasutab neid andmeid ja oluline RDL failide renderdamiseks aruande (juhist 6), siis tagastab aruande rakendusse (juhis 7), mis kuvab selle kasutaja (samm 8).

Stsenaarium, mis on näidatud siin, manustamine rakenduses aruande, ei ole see ainus viis. Samuti on võimalik vaadata aruandeid aruanne aruandehalduri, SharePointi VM VM või muul viisil. Aruandeid saab kombineerida ka, ühe aruande, mis sisaldab lingi teise.

SSRS on Azure VM annab teile täisfunktsionaalsuse aruandlusteenuste lahendusena pilveteenuses. Aruandeid saate kasutada mis tahes andmeallika SSRS ei toeta. Ja aruandeid saate kaasata manustatud koodi või assemblereid toetamiseks kohandatud käitumist. Aruande täitmise ja visualiseerimine on kiire, kuna serveri sisust teatamine ja mootor töötab koos sama virtuaalserveri.



## <a name="tblstor"></a>Tabelimälu

Relatsiooniliste andmete on kasulik paljudes olukordades, kuid see ei ole alati õige valik. Kui teie rakendus peab kiire ja lihtne juurdepääs väga suurte andmehulkade lõdvalt struktureeritud, näiteks relatsiooniandmebaasist ei pruugi hästi. NoSQL tehnoloogia tõenäoliselt parem variant.

Azure'i Tabelimälu on kujutatud NoSQL lähenemine selline. Hoolimata oma nimest Tabelimälu ei toeta standard relatsiooniline tabelid. Selle asemel pakub nn */-väärtuse talletada*, seostamise andmete kindla võti ja seejärel lastes Accessi rakenduste andmeid esitada võti. [Joonis 6](#Fig6) iseloomustab leiate altpoolt.

<a name="Fig6"></a>![Skeem tabelimäluga][SQL-tblstor]
 
**Joonis 6: Azure'i Tabelimälu on poe /-väärtuse, mis pakub kiire ja lihtne juurdepääsu suurte andmehulkade.**

Plekid, nagu on iga tabeli seotud konto Azure salvestusruumi. Tabelid on ka nimega palju nagu plekid, mille URL on kujul

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*TableName*&gt;

Nagu joonisel on näidatud, on iga tabeli jagatud teatud arvu sektsioonid, millest igaüks saab salvestada eraldi arvutisse. (See on vormi sharding, nagu ka SQL-i Federation.) Nii Azure ja taotluse töötab mujal pääsevad tabeli rahulik OData Protocol (protokoll) või Azure salvestusruumi kliendi teek.

Tabeli iga sektsiooni hoiab teatud arvu *üksuste*kumbki sisaldab nii palju 255 *Atribuudid*. Iga on nimi, tüüp (nt kahendarvuks, Bool, kuupäev ja kellaaeg, Int või stringi) ja väärtus. Relatsiooniline erinevalt, nendes on fikseeritud skeemi pole ja nii erinevate üksuste samas tabelis võib sisaldada eri tüüpi atribuudid. Ühe üksuse võib olla lihtsalt sisaldavate nimi, näiteks mõne muu üksuse samas tabelis on kaks Int atribuudid kliendi ID-number ja krediidi reiting sisaldavad stringi atribuut.

Konkreetse üksuse tabelit tuvastamiseks rakenduse pakub selle üksuse võti. Võti on kaks osa: *partition klahvi* , mis tuvastab kindlasse sektsiooni ja *rea klahvi* , mis tuvastab sektsiooni üksus. [Joonis 6](#Fig6), näiteks taotleb kliendi ettevõte koos partition A ja rea võti 3 ja Tabelimälu tagastab selle üksuse, sh kõik see sisaldab atribuudid.

See struktuur võimaldab tabelite suur – üks tabel võib sisaldada kuni 100 TB andmeid - ja see võimaldab sisalduvate andmete kiire juurdepääs. See toob kaasa ka piirangud, kuid. Näiteks ei toetata tabelite või isegi sektsioonid ühe tabeli ulatuvad selgituseks värskendusi. Sarja värskendusi tabelisse ainult saate rühmitada atomic toimingu kui kõik seotud üksused on sama sektsiooni. Olemas on ka kuidagi päringu tabeli atribuudi väärtuse ega on ei toeta ühenduste üle mitme tabeli. Ja erinevalt relatsioonandmebaasidest tabelid on salvestatud toimingute ei toeta.

Azure'i Tabelimälu on hea valik rakendusi, mis on vaja kiiresti, odavad juurdepääsu suurte andmehulkade lõdvalt struktureeritud. Näiteks võite Interneti rakendus, mis talletab profiiliteavet paljude kasutajate kasutada tabelid. Kiire juurdepääs on oluline sellisel juhul ja rakendus ilmselt ei pea täielik õigus SQL-i. See funktsioon loobunud saada kiiruse ja suurus võib mõnikord teha mõttes ja nii Tabelimälu on vaid mõned probleemid õige lahendus.


## <a name="hadoop"></a>Hadoopi

Ettevõtted on koostamise andmete ladu aastat. Nende saidikogumite teavet kõige sagedamini talletatud relatsiooniline tabelid, andke töötamise ja õppida andmete mitmel erineval viisil. SQL serveri, näiteks on levinud näiteks SQL Serveri analüüsiteenuste abil tehke järgmist.

Kuid Oletame, et soovite teha analüüsi-relatsiooniliste andmete põhjal. Andmete võib võtta palju vorme: teabe anduritest või raadiosagedustuvastuse, logifailid serveri parkides, Click Stream andmed veebirakendusi, pilte diagnostika seadmete ja palju muud. Andmed ka võib olla väga suur, liiga suur tõhusaks kasutamiseks koos traditsiooniline andmebaas. Suur andmete probleeme nagu siin, harvaesinevate veel mõne aasta muutunud üsna levinud.

Seda tüüpi suur andmete analüüsimiseks suures osas on meie valdkonna lähenesid ühe lahenduse: avatud lähtekoodi tehnoloogia Hadoopi. Hadoopi käivitatakse klaster füüsilise või virtuaalmasinates, levitada andmeid see töötab nende arvutites ja selle paralleelselt töötlemine. Veel masinad Hadoopi on kasutada, kiiremini saavad teostada, mis tahes töö see teeb.

Selline probleem on loomulikus sobivad avaliku pilve. Selle asemel, säilitades on armee kohapealse serverid, mis võivad jõudeolekus palju aega, töötab Hadoopi pilves, saate luua istuda (ja maksma) VMs ainult siis, kui neid vajate. Veelgi paremini üha suur andmed, mida soovite analüüsida Hadoopi abil luuakse pilves, säästab vaeva ringi liikumine. Selleks et saaksite neid saavutada, pakub Microsoft Azure Hadoopi teenust. [Joonis 7](#Fig7) näitab kõige olulisem seda teenust.

<a name="Fig7"></a>![Hadoopi skeem][hadoop]

**Joonis 7: Azure'i Hadoopi käivitatakse MapReduce tööd, mis samaaegselt mitu virtuaalmasinates kasutamine andmete töötlemiseks.**

Azure'i Hadoopi kasutamiseks esmalt palute selle Pilve platvormi loomine Hadoopi kobar, mis määrab, mitu peate VMs. Hadoopi kobar ise häälestada on ravisaajate tööülesande ja seega lastes Azure'i seda teha on mõistlik. Kui olete lõpetanud klaster kasutamisel saate välja lülitada. Ei ole vaja maksma Arvuta ressursid, mida te ei kasuta.

Hadoopi rakendus, mida nimetatakse *töö*kasutab tuntud *MapReduce*programmeerimise mudel. Nagu joonisel on näidatud, loogika MapReduce töö töötab korraga palju VMs üle. Samal ajal andmete töötlemiseks Hadoopi saate andmeid analüüsida palju kiiremini ühe-seadme lahendusi.

Azure, andmed on töö töötab MapReduce on tavaliselt hoida bloobimälu. Hadoopi, siiski MapReduce töö oodatud andmed talletatakse *Hadoopi jaotatud faili süsteemi (HDFS)*. HDFS sarnaneb bloobimälu mõnel viisil; See tiražeerib andmete mitme füüsilise serverites, näiteks. Selle asemel, et seda funktsiooni Dubleeri seab Hadoopi Azure'i bloobimälu läbi HDFS API, selle asemel nagu joonisel näha. Ajal MapReduce töö loogika arvab, et see on juurdepääs tavapärase HDFS failide töö on tegelikult andmetega töötamine voona see plekid kaudu. Ja juhul, kui on mitme töö kestab samad andmed toetamiseks Hadoopi Azure ka kopeerida andmete põhjal plekid täielik HDFS VMs töötab. 

MapReduce tööd on tavaliselt kirjutatud Java täna, lähenemisviisi, mis Hadoopi Azure toetab. Microsoft on lisatud ka MapReduce töökohtade loomine muudes keeltes, sh C#, F # ja JavaScripti tugi. Eesmärk on teha see suur andmete tehnoloogia kergemini kättesaadavaks suurema rühma arendajatele.

Koos HDFS ja MapReduce, Hadoop sisaldab muid tehnoloogiaid, mis andke kirjutamata MapReduce töö ise andmete analüüsiks. Näiteks siga on mõeldud analüüsida suurt andmeid, taru on saadaval SQL-like keeles nimetatakse HiveQL üksikasjalik keel. Nii siga ja taru tegelikult saavad MapReduce töö, HDFS andmeid, kuid need see keerukus nende kasutajate eest peita. Mõlemad on varustatud Hadoopi Azure.

Microsoft pakub Excel HiveQL draiver. Exceli lisandmooduli kasutamisel ärianalüütikud saate luua HiveQL päringute (ja MapReduce tööde haldamine) otse Excelist, siis töötlemine ja Powerpivoti ja Exceli muude tööriistade abil tulemused visualiseerida. Hadoopi Azure sisaldab muid tehnoloogiaid, nt Õppekeskuse teekide Mahout, Graphi andmehankimise süsteem Pegasus ja muu seade.

Suur Andmeanalüüs on oluline ja seega Hadoopi on oluline. Hadoopi hallatav teenus koos linkidega tuttavaid tööriistu, nt Excel Azure, andes Microsoft soovitakse see tehnoloogia ja kasutajatele kättesaadavaks muuta.

Laiemalt igasuguseid andmed on oluline. Sellepärast, et Azure'i sisaldab mitmesuguseid võimalusi andmete haldamise ja business Analyticsi. Mis tahes rakendus, mida proovite luua, on tõenäoline, et leiate midagi selle Pilve platvormi, mis töötavad teie jaoks.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
