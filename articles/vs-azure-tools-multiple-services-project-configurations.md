<properties
   pageTitle="Konfigureerimise abil mitme teenuse konfiguratsioone Azure'i projekti | Microsoft Azure'i"
   description="Saate teada, kuidas konfigureerida on Azure pilveteenuse teenuse projekt, muutes ServiceDefinition.csdef ja ServiceConfiguration.cscfg failid."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Azure'i projekti abil mitme teenuse konfiguratsioone konfigureerimine

Azure'i pilvepõhise teenuse projekt, mis sisaldab kahte failid: ServiceDefinition.csdef ja ServiceConfiguration.cscfg. Need failid on Azure pilveteenuse teenuserakenduse pakkida ja Azure juurutatud.

- **ServiceDefinition.csdef** fail sisaldab metaandmed, mida on vaja Azure keskkonnas oma pilveteenuse rakenduse, sh, mis sisaldab rollid nõuded. See fail sisaldab ka konfiguratsioonisätted, mis kehtivad kõik eksemplarid. Käitusajal Azure'i teenus majutusteenuse käitusaja API abil saab lugeda nende sätete konfigureerimine. Faili ei saa värskendada oma teenuse töötamise Azure.

- **ServiceConfiguration.cscfg** fail määrab väärtuste jaoks määratletud teenuse määratluse faili konfiguratsioonisätted ja määrab iga rolli käivitamiseks eksemplaride arv. Selle faili saab värskendada oma pilveteenuses töötamise Azure.

Azure'i tööriistad Microsoft Visual Studio pakuvad atribuudi lehed, mille abil saate määrata konfiguratsioonisätted, mis on talletatud need failid. Atribuut lehtede juurdepääsu rolli viidet Azure pilveteenuse teenuse project Solution Exploreris all või paremklõpsake rolli viide ja valige **Atribuudid**, nagu on näidatud järgmisel joonisel.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Aluseks oleva skeemide teenuse määratlemise ja teenuse konfigureerimine failide kohta leiate teavet teemast [Skeemi viide](https://msdn.microsoft.com/library/azure/dd179398.aspx). Teenuse konfigureerimise kohta leiate lisateavet teemast [Cloud Services konfigureerimine](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Roll atribuutide konfigureerimine

Atribuut lehtede web rolli ja töötaja roll on sarnased, kuigi järgmistes jaotistes toodud mõned erinevused.

Lehelt **vahemälu** saate konfigureerida Azure, vahemällu teenused.

### <a name="configuration-page"></a>Lehel konfigureerimine

**Konfiguratsiooni** lehele, saate määrata järgmised atribuudid.

**Eksemplari**

Määrake atribuudi **eksemplari** count teenuse peaks töötama selle rolli eksemplaride arv.

Määrake atribuudi **VM suurus** **Eest Small**, **väike**, **Keskmine**, **suur**või **Eest suur**.  Lisateabe saamiseks vt [suurused pilveteenustega](./cloud-services/cloud-services-sizes-specs.md).

**Toimingu käivitamine** (Ainult web roll)

Saate määrata, et Visual Studio käivitama lõpp-punktid HTTP või HTTPS-i lõpp-punktid või mõlema veebibrauseri käivitamisel silumine atribuut.

HTTPS-i lõpp-punkti suvand on saadaval ainult siis, kui olete juba määratlenud HTTPS-i lõpp-punkti oma rolli. Saate määratleda HTTPS-i lõpp-punkti atribuudilehel **lõpp-punktid** .

Kui olete juba lisanud HTTPS-i lõpp-punkti, HTTPS-i lõpp-punkti suvand on vaikimisi ja käivitab Visual Studio brauseris selle lõpp-punkti silumine Lisaks teie HTTP lõpp-punkti brauseri käivitamisel. See eeldab, et nii käivitussuvandite on lubatud.

**Diagnostika**

Vaikimisi on lubatud diagnostika Web roll. Azure pilveteenuse teenuse project ja salvestusruumi konto on seatud kasutama kohalikku emulaator. Kui olete valmis kasutama Azure'i, saate valida Koosturi nuppu (**…**) värskendamiseks salvestusruumi konto, mida soovite kasutada Azure salvestusruumi pilveteenuses. Nõudmisel või automaatselt ajastatud intervalliga saate edastada salvestusruumi konto diagnostika andmed. Azure'i diagnostika kohta leiate lisateavet teemast [Azure pilveteenustega ja Virtuaalmasinates diagnostika lubamine](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Lehe sätted

Klõpsake lehe **sätted** saate lisada konfiguratsioonisätted teie teenuse jaoks. Otsingukonfiguratsiooni sätetest on nime ja väärtuse paarideks. Roll koodi saate lugeda väärtuste kasutamine [Azure hallatavate teegi](http://go.microsoft.com/fwlink?LinkID=171026)esitatud tunnid käitusajal sätete konfigureerimine. Täpsemalt [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) meetod tagastab nimega konfiguratsioon sätte käitusajal väärtus.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Ühendusstringi salvestusruumi konto konfigureerimine

Ühendusstringi on konfiguratsiooni säte, mis pakub ühendust ja autentimise teavet salvestusruumi emulaator või Azure storage konto. Iga kord, kui teie kood peab pääseda koodi, mis töötab roll Azure storage teenused andmetele – st bloobimälu, järjekorda või tabeli andmed –, on teil määrata ühendusstringi salvestusruumi konto jaoks.

Ühendusstringi, mis viitab Azure storage konto peate kasutama määratletud vorming. Ühendusstringi loomise kohta leiate lisateavet teemast [Konfigureerimine Azure'i salvestusruumi ühendusstringi](./storage/storage-configure-connection-string.md).

Kui olete valmis, et testida teenust Azure storage teenuste eest, või kui olete valmis kasutama oma pilveteenuses Azure, saate muuta mis tahes ühendusstringi osutamiseks Azure storage konto väärtus. Valige (**…**), valige **Enter salvestusruumi konto identimisteave**. Sisestage oma konto teave, mis sisaldab teie konto nimi ja konto võti. Dialoogiboksis **Salvestusruumi konto ühendusstringi** saate märkida, kas soovite kasutada vaikimisi HTTPS-i lõpp-punktid (vaikimisi valik), vaikimisi HTTP lõpp-punktid või kohandatud lõpp-punktid. Kui otsustate kasutada kohandatud lõpp-punktid, kui olete registreerunud teie teenuse jaoks kohandatud domeeninime, nagu on kirjeldatud [konfigureerimine Azure storage konto bloobimälu andmete jaoks kohandatud domeeninime](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Peate muutma oma ühendusstringi osutamiseks Azure storage konto enne juurutamist teenust. Toiming ei võib põhjustada teie roll pole alustamiseks või lähtestamisel, hõivatud ja peatamine olekus liikumiseks.

## <a name="endpoints-page"></a>Lõpp-punktid leht

Töötaja roll võib olla mis tahes arv HTTP, HTTPS või TCP lõpp-punktid. Lõpp-punktid saab Sisestuskeel lõpp-punktid, mis on saadaval välise klientidele, või sisemise lõpp-punktid, mis on saadaval teiste rollide omast, kus töötab teenus.

- Kättesaadavaks lõpp HTTP väliste klientide ja veebibrauserid, muuta lõpp-punkti tüüp sisestada ja määrake nimi ja avaliku pordinumber.

- Kättesaadavaks HTTPS-i lõpp-punkti väliste klientide ja veebibrauserid, muuta lõpp-punkti tüüp **Sisestuskeel**ja määrake nimi, avaliku pordinumber ja haldus serdi nimi.

    Pange tähele, et saate määrata serdi haldus, peate määrama serdi atribuudilehel **serdid** .

- Lõpp-punkti teiste rollide omast pilveteenusesse, sisemine juurdepääsu kättesaadavaks teha muuta lõpp-punkti tüüp sisemine ja määrake nimi ja võimalike privaatne pordid selle lõpp-punkti.

## <a name="local-storage-page"></a>Kohaliku lehe

Saate reserveerida ühe või mitme kohalikku ressursid rolli **Kohalikku** atribuudileht. Kohaliku ressursi on reserveeritud kataloog Azure virtuaalse masina, kus töötab eksemplari rolli failisüsteemis.

## <a name="certificates-page"></a>Lehe serdid

Klõpsake lehel **serdid** saate seostada oma rolli serdid. Lisamist serdid saab konfigureerida HTTPS-i lõpp-punktide atribuudilehel **lõpp-punktid** .

**Serdid** atribuudileht sertide teave lisatakse teie teenuse konfigureerimine. Pange tähele, et teie serdid on pakitud teie teenusega; teil tuleb üles laadida sertide eraldi Azure'i [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)kaudu.

Serdi seostada oma rolli, sisestage nimi serti. Selle nime abil viidata serdi HTTPS-i lõpp-punkti konfigureerimisel atribuudileht **lõpp-punktid** . Järgmiseks määrata, kas certificate store on **Kohalikus arvutis** või **Praeguse kasutaja** ja nimi salvestada. Lõpetuseks, sisestage serdi sõrmejälje. Kui sert on praegune User\Personal (minu) poes, saate sisestada serdi sõrmejälje, valides serdi täidetud loendist. Kui see asub mõnes teises asukohas, sisestage väärtus sõrmejälje käsitsi.

Kui lisate poest serdi sert, mis tahes vahe serdid lisatakse automaatselt teile sätete konfigureerimine. Nende vahe serdid tuleb ka üles laadida Azure'i selleks, et teie teenuse jaoks SSL-i õigesti konfigureerimine.

Mis tahes teenust seostada halduse serdid rakendada teenust ainult siis, kui see töötab pilveteenuses. Kui teenust töötab kohaliku arenduskeskkond, kasutab see standardne sert, mida haldab Arvuta emulaator.

## <a name="configuring-the-azure-cloud-service-project"></a>Azure'i pilvepõhise teenuse projekti seadistamine

Rakendada kogu Azure pilveteenuse teenuse projekti sätete konfigureerimiseks avate esimest kiirmenüü selle projekti sõlm ja valige Atribuudid avamiseks selle atribuudi lehti. Järgmine tabel näitab need lehed.

|Atribuudileht|Kirjeldus|
|---|---|
|Rakenduse|Sellelt lehelt, saate kuvada teavet Azure'i tööriistu, mis kasutab seda pilvepõhise teenuse project ja tööriistade praeguse versiooni täiendamist versiooni.|
|Sündmuste koostamine|Sellel lehel saate seada koostamine enne ja pärast koostamine sündmused.|
|Arengu|Sellelt lehelt, saate määrata Koosta konfigureerimise juhised ja tingimused, mis käivitatakse pärast Koosta sündmusi.|
|Web|Sellel lehel saate konfigureerida sätteid, mis on seotud veebiserver.|
