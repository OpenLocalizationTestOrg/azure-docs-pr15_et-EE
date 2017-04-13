<properties
    pageTitle="Azure'i salvestusruumi teenuse krüptimise ülejäänud andmete | Microsoft Azure'i"
    description="Azure'i salvestusruumi teenuse krüptimisfunktsiooni abil krüptida Azure'i bloobimälu teenuse pool, kui andmete säilitamise ja dekrüptida andmete allalaadimisel."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure'i salvestusruumi teenuse krüptimine ülejäänud andmed

Azure'i salvestusruumi teenuse krüptimise (SSE) andmete ülejäänud aitab teil kaitsta ja kaitsta oma andmeid ja vastavad teie ettevõtte turvalisus ja nõuetele vastavus kohustusi. Selle funktsiooniga Azure'i salvestusruumi automaatselt krüptib andmeid enne püsib salvestusruumi ja dekrüptib enne otsing. Krüptimist, dekrüptimine ja võtme haldamise on täiesti läbipaistev kasutajatele.

Järgmistes jaotistes üksikasjalikud juhised, kuidas kasutada salvestusruumi teenuse krüptimise funktsioonid samuti toetatud stsenaariumid ja kasutajale kogemusi.

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi pakub turvalisus funktsioonid, mis koos võimaldab arendajatel luua turvalist rakendusi täielik kogum. Andmeid saab turvatud vahelise rakendus ja Azure [Kliendipoolne krüptimist](storage-client-side-encryption.md), HTTPs või SMB 3.0 abil. Salvestusruumi teenuse krüptimise pakub krüptimist ja ülejäänud, käsitsemise krüptimist, dekrüptimine ja võtme haldamise täiesti läbipaistvaid viisil. Kõik andmed on krüptitud 256-bit [AES krüptimist](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), üks tugevam Blokeeri šifrid saadaval.

SSE toimib krüptimist, kui kirjutatakse Azure Storage, ja saab kasutada Blokeeri plekid, lehe plekid ja lisa plekid. See toimib järgmiselt:

-   Üldine otstarve salvestusruumi kontod ja bloobimälu salvestusruumi kontod
-   Standard ja Premium 
-   Kõik koondamise tase (LRS ZRS GRS, RA-GRS)
-   Azure'i ressursihaldur salvestusruumi kontod (kuid mitte klassikaline) 
-   Kõigi piirkondade

Lubada või keelata salvestusruumi teenuse krüptimise salvestusruumi konto, [Azure portaali](https://azure.portal.com) sisse logida, seejärel valige salvestusruumi. Enne sätted, vaadake jaotist bloobimälu teenuse, nagu on näidatud selles kuvatõmmis ja klõpsake krüptimise.

![Portaali kuvatõmmis kuvav krüptimise suvand](./media/storage-service-encryption/image1.png)

Pärast seda, kui klõpsate krüptimise säte, te saate lubada või keelata salvestusruumi teenuse krüptimine.

![Portaali kuvatõmmis kuvav krüptimise atribuudid](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Krüptimise stsenaariumid

Salvestusruumi teenuse krüptimise saab lubada salvestusruumi konto tasemel. See toetab kliendi järgmistel juhtudel:

-   Blokeeri plekid, krüptimise lisamiseks plekid ja lehe plekid.

-   Krüptimise arhiivitud VHDs ja kohapealse Azure tuuakse mallid.

-   Aluseks oleva OS- ja ketast krüptimise abil oma VHDs loodud IaaS vms.

SSE on järgmised piirangud.

-   Klassikaline salvestusruumi kontod krüptimist ei toetata.

-   Klassikaline salvestusruumi kontod viiakse üle ressursihaldur salvestusruumi kontod krüptimise ei toetata.

-   Olemasolevate andmete - SSE krüptib ainult äsja loodud andmete pärast krüptimine on lubatud. Kui näiteks ressursihaldur salvestusruumi uue konto loomine, kuid ei sisselülitamine krüptimise ja seejärel saate üles laadida plekid või arhiivitud VHDs salvestusruumi konto ja seejärel lülitage SSE, krüptitakse need plekid juhul, kui nad ümber või kopeerida.

-   Turuplatsi tugi - luba krüptimist VMs loodud [Azure portaali](https://portal.azure.com), PowerShelli ja Azure CLI turuplatsilt. VHD pilti jääb krüptimata; siiski, mis tahes kirjutab valmis pärast VM on kedratud üles krüptitud.

-   Tabel, järjekordade ja failide andmeid ei saa krüptida.

##<a name="getting-started"></a>Alustamine

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Samm 1: [Loo uus konto salvestusruumi](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Samm 2: Lubada krüptimine.

Saate lubada [Azure portaali](https://portal.azure.com)krüptimine.

> [AZURE.NOTE] Kui soovite programmiliselt lubamiseks või keelamiseks salvestusruumi teenuse krüptimise salvestusruumi konto, saate [Azure Storage ressursi pakkuja REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx), [Salvestusruumi ressursi pakkuja kliendi teek .net-i](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShelli](../powershell-install-configure.md)või [Azure CLI](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Samm 3: Andmete kopeerimine salvestusruumi konto

Kui lubate SSE salvestusruumi konto ja seejärel kirjutamise plekid salvestusruumi konto, plekid krüptitud. Mis tahes plekid juba asub salvestusruumi konto pole krüptitud seni, kuni ta kirjutatakse. Saate kopeerida andmed ühe salvestusruumi konto ühe kasutaja krüptitud, SSE või isegi SSE lubamine ja plekid ühe container teise kopeerida kindel, et eelmise andmed on krüptitud. Järgmised tööriistade abil saate saavutamiseks.

#### <a name="using-azcopy"></a>AzCopy abil

AzCopy on mõeldud andmete kopeerimise ja sealt Microsoft Azure'i bloobimälu, failide ja tabeli salvestusruumi optimaalse jõudluse tagamiseks lihtsa käskude kasutamine Windows käsurea kasuliku. Saate selle kopeerida oma plekid salvestusruumi ühelt kontolt teisele, mis on lubatud SSE. 

Lisateabe saamiseks külastage [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Salvestusruumi kliendi teekide abil

Saate kopeerida bloobimälu andmeid ja bloobimälu kaudu või meie suurel hulgal erinevaid salvestusruumi kliendi teegid, sh .net-i, C++, Java, Android, Node.js, PHP, Python ja Ruby salvestusruumi kontode vahel.

Lisateabe saamiseks külastage meie [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Salvestusruumi Explorerit

Saate luua salvestusruumi kontod, üles laadida ja andmete allalaadimine, plekid sisu vaadata ja liikuda kataloogide salvestusruumi explorer. Üks neist abil saate üles laadida konto salvestusruumi plekid lubatud krüptimise abil. Mõned salvestusruumi maadeavastajad, kus te saate ka andmete kopeerimine olemasoleva bloobimälu teine ümbris salvestusruumi konto või uue salvestusruumi kontoga, millel on lubatud SSE.

Lisateabe saamiseks külastage [Azure'i salvestusruumi maadeavastajad](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Samm 4: Päringu oleku krüptitud andmed.

Salvestusruumi kliendi teegid värskendatud versioon on juurutatud saate päringu oleku objekti kindlaks teha, kui see on krüptitud või mitte. Näiteid selle dokumendi lisatakse lähitulevikus.

Vahepeal saate helistada [Saada konto atribuudid](https://msdn.microsoft.com/library/azure/mt163553.aspx) , veenduge, et konto salvestusruumi on lubatud krüptimise või salvestusruumi konto atribuutide vaatamiseks Azure'i portaalis.

##<a name="encryption-and-decryption-workflow"></a>Krüptimine ja dekrüptimine töövoog

Siin on krüptimine/dekrüptimine töövoo Lühikirjeldus:

-   Kliendi võimaldab salvestusruumi kontol.

-   Kui kliendi kirjutab uute andmete (pane bloobimälu, pane Blokeeri, panna lehe jne) bloobimälu; iga kirjutamine on krüptitud 256-bit [AES krüptimist](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), üks tugevam Blokeeri šifrid saadaval.

-   Kui kliendi vajab juurdepääsu andmetele (HANGI bloobimälu jne), andmed on automaatselt lahtikrüptitud kasutaja tagasi.

-   Kui krüptimine on keelatud, pole enam krüptitud uue kirjutab ja olemasoleva krüptitud andmed jäävad krüptitud, kuni kasutaja ümber. Kuigi krüptimine on lubatud, kirjutab bloobimälu krüptitud. Andmete olek ei muuda kasutaja lülitamise lubamine ja keelamine krüptimise salvestusruumi konto vahel.

-   Kõik krüptimise võtmed on salvestatud, krüptitud ja haldab Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Korduma kippuvad küsimused salvestusruumi teenuse krüptimise ülejäänud andmete

**K: on klassikaline salvestusruumi konto. Lubada SSE seda?**

A: ei SSE toetatakse ainult ressursihaldur salvestusruumi kontod.

**Q: kuidas krüptida andmete klassikaline salvestusruumi minu konto?**

A: ressursihaldur salvestusruumi uue konto loomine ja kopeerige andmed [AzCopy](storage-use-azcopy.md) kaudu oma kontot klassikaline salvestusruumi vastloodud ressursihaldur salvestusruumi kontole. 

Teine võimalus on ressursi Halda salvestusruumi konto klassikaline salvestusruumi kontole migreerida. Lisateabe saamiseks vaadake [Platvormi toetatud migreerimise, IaaS materjale klassikalises ressursside halduri kaudu](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**K: on ressursihaldur salvestusruumi konto. Lubada SSE seda?**

V: Jah, kuid ainult äsja kirjutada plekid krüptitud. See uuesti ja Krüpti andmeid, mis oli juba kohal. 

**K: soovite krüptida praeguse andmed olemasoleva ressursihaldur salvestusruumi konto?**

V: saate lubada SSE ressursihaldur salvestusruumi konto igal ajal. Siiski plekid, mis olid juba olemas pole krüptitud. Need plekid krüptimiseks saate kopeerimiseks teise nime või mõne muu container ja seejärel eemaldage krüptimata versioonid.

**K: Ma kasutan Premium salvestusruumi; SSE saab kasutada?**

V: SSE on toetatud nii Standard ja Premium.

**Q: kas salvestusruumi uue konto loomine ja lubamine SSE ja seejärel luua uue kontoga salvestusruumi VM, tähendab minu VM on krüptitud?**

V: Jah. Mis tahes ketast loonud uue konto salvestusruumi kasutavate krüptitud, kui nad on loodud pärast SSE on lubatud. Kui VM on loodud Azure'i turuplats, VHD pilti jääb krüptimata; siiski, mis tahes kirjutab valmis pärast VM on kedratud üles krüptitud.

**Q: kas SSE lubatud Azure PowerShelli ja Azure CLI abil luua uue salvestusruumi kontod?**

V: Jah.

**Q: kuidas palju maksab Azure Storage kui SSE on lubatud?**

V: on tasuta.

**Q: kes haldab krüptimise võtmed?**

V: võtmed haldab Microsoft.

**Q: saab kasutada oma krüptimise võtmed?**

V: me tegeleme mis pakub võimalusi klientide tuua oma krüptimise võtmed.

**Q: Kas ma krüptimise võtmed juurdepääsu tühistada?**

V: pole praegu; klahvid täielikult haldab Microsoft.

**Q: on vaikimisi sisse lülitatud SSE salvestusruumi uue konto loomisel?**

V:-SSE pole lubatud vaikimisi; Azure portaali abil saate selle taas lubada. See funktsioon salvestusruumi ressursi pakkuja REST API abil saate ka programmiliselt lubada.

**Q: Kuidas erineb see Azure'i draivi krüptimine?**

V: seda funktsiooni kasutatakse andmete Azure'i bloobimälu krüptimiseks. Azure'i ketta krüptimise kasutatakse OS- ja ketta jaotise IaaS VMs krüptimiseks. Lisateabe saamiseks külastage [Salvestusruumi turvalisuse juhend](storage-security-guide.md).

**K: kui SSE, ja seejärel avage ja lubada Azure ketta krüptimise lubamine selle kettale?**

V: see töötab sujuvalt. Allpool on mõlema meetodi krüptitud andmete.

**K: salvestusruumi konto on häälestatud geo redundantly korrata. Kui lubamiseks SSE minu üleliigne koopia ka krüptitud?**

V: kõik eksemplarid salvestusruumi konto krüptitud ja kõik koondamise suvandid – kohalikult liigsete salvestusruumi (LRS), Zone-tarbetud mälu (ZRS), Geo-tarbetud salvestusruumi (GRS) ja lugemisõigus Geo-tarbetud salvestusruumi (RA GRS) – on toetatud.

**K: ei saa kasutada krüptimise salvestusruumi minu konto.**

V: on ressursihaldur salvestusruumi konto? Klassikaline salvestusruumi kontod ei toetata. 

**Q: SSE lubatud ainult teatud piirkondades?**

V: Lõuna on saadaval kõikides piirkondades. 

**Q: kuidas ühendust keegi, kui mul on probleeme või soovite anda tagasisidet?**

V: võtke ühendust [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) mis tahes salvestusruumi teenuse krüptimise seotud probleemide kohta.

##<a name="next-steps"></a>Järgmised sammud

Azure'i salvestusruumi pakub turvalisus funktsioonid, mis koos võimaldab arendajatel luua turvalist rakendusi täielik kogum. Lisateabe saamiseks külastage [Salvestusruumi turvalisuse juhend](storage-security-guide.md).
