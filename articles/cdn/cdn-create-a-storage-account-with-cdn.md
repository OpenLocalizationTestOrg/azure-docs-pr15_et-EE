<properties
    pageTitle="Salvestusruumi konto integreerida CDN | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure sisu kohaletoimetamise võrk (CDN) esitamisel läbilaskevõimega sisu, vahemällu plekid Azure salvestusruumist."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Salvestusruumi konto integreerida CDN-ID

CDN saab lubada vahemälu sisu oma Azure salvestusruumist. See pakub arendajate globaalne lahendust latentsusajaga videosisu, vahemällu plekid ja Arvuta eksemplaride veebisaidil füüsilise sõlmed Ameerika Ühendriigid, Euroopa, Aasia, Austraalia ja Lõuna-Ameerika staatiliseks sisuks.


## <a name="step-1-create-a-storage-account"></a>Samm 1: Salvestusruumi konto loomine

Azure'i tellimusele salvestusruumi uue konto loomiseks kasutada järgmist. Salvestusruumi konto annab juurdepääsu Azure storage teenused. Salvestusruumi konto tähistab kõrgeima taseme juurdepääsuks iga komponendi Azure storage teenuse nimeruumi: teenuste, järjekorda teenuste ja tabeli teenuste Bloobivahemälu. Lisateabe saamiseks vaadake [Microsoft Azure'i Tabelimäluga tutvustus](../storage/storage-introduction.md).

Salvestusruumi konto loomiseks peate olema kas teenuse administraator või koostöö administraator seotud tellimuse jaoks.

> [AZURE.NOTE] On mitu võimalust, mille abil saate salvestusruumi konto, sh PowerShelli ja Azure portaali loomine.  Selles õpetuses mõeldud me kasutame Azure'i portaal.  

**Azure'i tellimuse salvestusruumi konto loomine**

1.  [Azure'i portaali](https://portal.azure.com)sisse logima.
2.  Valige vasakus ülanurgas **Uus**. Dialoogiboksis **Uus** valik **andmete + salvestusruumi**ja siis nuppu **salvestusruumi konto**.

    **Loo salvestusruumi konto** tera kuvatakse.

    ![Salvestusruumi konto loomine][create-new-storage-account]

4. Tippige väljale **nimi** alamdomeeni nimi. See kirje võib sisaldada 3-24 väiketähti ja numbreid.

    See väärtus muutub hostinimi tellimuse aadress bloobimälu, järjekorda või tabeli ressursid kasutatava URI sees. Container ressursi bloobimälu teenus käsitlema kasutaksite URI järgmises vormingus, kus * &lt;StorageAccountLabel&gt; * sisestatud **URL-i Enter**väärtusel:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Oluline:** URL-i sildi forms salvestusruumi konto URI alamdomeen ja peab olema kordumatu kõik majutatud teenuste Azure vahel.

    Seda väärtust kasutatakse ka selle portaalis või salvestusruumi konto nimi selle konto kasutamisel programmiliselt.

5. Jätke vaikesätted **juurutamise mudeli**, **konto tüüpi**, **jõudlus**ja **kopeerimine**. 

6. Valige **tellimuse** salvestusruumi konto kasutatakse koos.

7. Valige või looge **Ressursirühma**.  Ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Valige konto salvestusruumi asukoht.

8. Klõpsake nuppu **Loo**. Salvestusruumi konto loomise protsess võib võtta mitu minutit.


## <a name="step-2-create-a-new-cdn-profile"></a>Samm 2: Looge uus profiil CDN-ID

CDN profiil on kogumi CDN lõpp-punktid.  Iga profiil sisaldab ühe või mitme CDN lõpp-punktid.  Soovi korral võite kasutada mitut profiili CDN lõpp-punktide Interneti-domeeni, veebirakenduse või mõnda muude kriteeriumide järgi korraldada.

> [AZURE.TIP] Kui teil on juba CDN profiil, mida soovite kasutada selles õpetuses, jätkake [3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Samm 3: Loo uus CDN lõpp-punkti

**On uus CDN lõpp-punkti loomiseks salvestusruumi konto jaoks**

1. [Azure'i haldusportaal](https://portal.azure.com), liikuge CDN profiili.  Teil võib-olla on kinnitatud selle armatuurlaua eelmises etapis.  Kui te pole, leiate selle nuppu **Sirvi**ja seejärel **CDN profiilid**ja soovite lisada oma lõpp-punkt profiili.

    CDN profiil tera kuvatakse.

    ![CDN profiil][cdn-profile-settings]

2. Klõpsake nuppu **Lisa lõpp-punkti** .

    ![Lõpp-punkti nupp Lisa][cdn-new-endpoint-button]

    **Lisa lõpp** tera kuvatakse.

    ![Lõpp-punkti blade lisamine][cdn-add-endpoint]

3. Sisestage selle CDN lõpp-punkti **nimi** .  Selle nime kasutatakse juurdepääsu oma domeeni vahemällu talletatud ressursside `<endpointname>.azureedge.net`.

4. Valige rippmenüüst **Origin tüüp** *salvestusruumi*.  

5. Valige rippmenüüst **Origin hostname** konto salvestusruumi.

6. Jätke vaikesätted **Origin tee**, **Origin hosti päise**ja **protokoll/päritolu port**.  Määrake vähemalt üks protocol (HTTP- või HTTPS).

    > [AZURE.NOTE] Selle konfiguratsiooni lubab kõigi oma avalikult nähtav ümbriste teie salvestusruumi konto on CDN vahemällu.  Kui soovite piirata ühe container abil, kasutage **Origin tee**.  Pange tähele, et ümbris peab olema seatud riiklike Valikupaan.

7. Klõpsake nuppu **Lisa** uus lõpp-punkti loomiseks.

8. Kui lõpp-punkti on loodud, kuvatakse see loendis profiili lõpp-punktid. Loendivaate näitab URL-i abil vahemällu talletatud sisu, samuti origin domeeni.

    ![CDN lõpp-punkti][cdn-endpoint-success]

    > [AZURE.NOTE] Lõpp-punkti kohe ei kasutamiseks saadaval.  Võib kuluda kuni 90 minutit registreerimise levitada CDN võrgu kaudu. Kasutajad, kes proovida kasutada CDN domeeninime kohe saada kuni sisu on saadaval on CDN olekukoodi 404.


## <a name="step-4-access-cdn-content"></a>Samm 4: Accessi CDN sisu

Vahemällu talletatud sisu on CDN juurdepääsuks kasutada portaalis esitatud CDN URL. Vahemällu salvestatud bloobimälu aadress on järgmine:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Kui CDN salvestusruumi konto juurdepääsu lubamine või teenuse majutatud, avalikult kättesaadavaks kõigi objektide laiene CDN serva vahemällu. Kui muudate objekti, mis on praegu vahemälus on CDN, uue sisu ei ole saadaval kaudu on CDN seni, kuni soovitud CDN värskendab selle sisu aegumisel vahemällu talletatud sisu aja live perioodi.

## <a name="step-5-remove-content-from-the-cdn"></a>Juhis 5: Sisu eemaldamine on CDN-ID

Kui te ei soovi enam vahemälu objekti sisse Azure'i sisu kohaletoimetamise võrk (CDN), tehke ühte järgmistest:

-   Saate teha ümbris asemel era. Lisateabe saamiseks vaadake [haldamine anonüümse ümbriste ja plekid lugemisõigus](../storage/storage-manage-access-to-resources.md) .
-   Saate keelata või haldusportaali abil CDN lõpp-punkti kustutamine.
-   Saate muuta oma majutatud teenuse vastata enam objekti.

Objekti, mis on juba vahemälus on CDN jäävad vahemällu talletatud kuni aja live objekti lõpeb või langetatakse lõpp-punkti. Aja live perioodi aegumisel selle CDN kontrollib kas CDN lõpp-punkti on veel kehtib ja anonüümselt pääseb objekti. Kui pole, siis objekt pole enam salvestatakse vahemällu.


## <a name="additional-resources"></a>Lisaressursid

-   [Kuidas CDN sisu vastendamiseks kohandatud domeeni](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
