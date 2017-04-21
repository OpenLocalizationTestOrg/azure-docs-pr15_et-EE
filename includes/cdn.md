# <a name="using-cdn-for-azure"></a>Azure CDN abil

Azure'i sisu kohaletoimetamise võrk (CDN) pakub arendajate globaalne lahendust latentsusajaga videosisu, vahemällu plekid ja Arvuta eksemplaride veebisaidil füüsilise sõlmed Ameerika Ühendriigid, Euroopa, Aasia, Austraalia ja Lõuna-Ameerika staatiliseks sisuks. CDN sõlm asukohad praeguse loendi leiate [Azure'i CDN sõlm asukohad].

Selles ülesandes sisaldab järgmist:

* [Samm 1: Salvestusruumi konto loomine](#Step1)
* [Samm 2: Looge uus CDN näitaja salvestusruumi konto jaoks](#Step2)
* [Samm 3: CDN sisule juurde pääseda](#Step3)
* [Samm 4: Eemaldamine CDN sisu](#Step4)

Azure'i andmete vahemällu CDN kasutamise eelised on järgmised.

-   Parema jõudluse ja kasutajale kogemus end kasutajatele, kellel ei ole sisuallika ja kasutate rakenduste, kui palju internet reisi peab sisu laadimine
-   Öelge paremini toime hetke suure koormuse, jaotatud suuremahuliste alguses sündmus, näiteks toote käivitamine

Olemasoleva CDN kliendid nüüd saate kasutada Azure CDN [Azure klassikaline portaalis]. Funktsiooni CDN on lisandmooduli funktsioon tellimusele ja on eraldi [arveldustoe leping].

<a id="Step1"> </a>
<h2>Samm 1: Salvestusruumi konto loomine</h2>

Azure'i tellimusele salvestusruumi uue konto loomiseks kasutada järgmist. Salvestusruumi konto annab juurdepääsu Azure storage teenused. Salvestusruumi konto tähistab kõrgeima taseme juurdepääsuks iga komponendi Azure storage teenuse nimeruumi: teenuste, järjekorda teenuste ja tabeli teenuste Bloobivahemälu. Azure'i salvestusruumi teenuste kohta leiate lisateavet teemast [Azure salvestusruumi teenuste kasutamisel](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Salvestusruumi konto loomiseks peate olema kas teenuse administraator või koostöö administraator seotud tellimuse jaoks.

> [AZURE.NOTE] Selle toimingu abil Azure'i teenuse juhtimise API kohta leiate teemast [Salvestusruumi konto loomine](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) viide.

**Azure'i tellimuse salvestusruumi konto loomine**

1.  [Azure'i klassikaline portaali]sisse logida.
2.  Klõpsake vasakus allnurgas nuppu **Uus**. Dialoogiboksis **Uus** valige **Data Services**ja seejärel käsku **salvestusruumi**, seejärel **Kiiresti luua**.

    Kuvatakse dialoogiboks **Loo salvestusruumi konto** .

    ![Salvestusruumi konto loomine][create-new-storage-account]

4. Tippige väljale **URL-i** alamdomeeni nimi. See kirje võib sisaldada 3-24 väiketähti ja numbreid.

    See väärtus muutub hostinimi tellimuse aadress bloobimälu, järjekorda või tabeli ressursid kasutatava URI sees. Container ressursi bloobimälu teenus käsitlema kasutaksite URI järgmises vormingus, kus * &lt;StorageAccountLabel&gt; * sisestatud **URL-i Enter**väärtusel:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Oluline:** URL-i sildi forms salvestusruumi konto URI alamdomeen ja peab olema kordumatu kõik majutatud teenuste Azure vahel.

    Seda väärtust kasutatakse ka selle portaalis või salvestusruumi konto nimi selle konto kasutamisel programmiliselt.

5.  Valige ripploendi **Osaleja/piirkond jaotises** piirkond või osaleja rühma salvestusruumi konto. Osaleja rühma asemel piirkonnas valimine, kui soovite sama andmekeskuse muude Windows Azure'i teenustega, mida kasutate salvestusruumi teenuste. See parandab jõudlust ja sealt tekivad maksud.  

    **Märkus:** Osaleja rühma loomine avamine halduse portaali ala **sätted** , klõpsake **osaleja rühmad**ja seejärel klõpsake käsku **Lisa mõne osaleja rühm** või **Lisa**. Saate luua ja kasutamine Windows Azure teenuse juhtimise API osaleja rühmade haldamiseks. Lisateabe saamiseks vt [toiminguid osaleja rühmade].

6. Valige ripploendist **tellimuse** tellimus, mida kasutatakse koos salvestusruumi konto.
7.  Klõpsake nuppu **Loo konto salvestusruumi**. Salvestusruumi konto loomise protsess võib võtta mitu minutit.
8.  Veenduge, et konto salvestusruumi on loodud, veenduge, et konto kuvatakse üksused, mis on loetletud oleku **Online** **Storage** .

<a id="Step2"> </a>
<h2>Samm 2: Looge uus CDN näitaja salvestusruumi konto jaoks</h2>

Kui CDN salvestusruumi konto juurdepääsu lubamine või teenuse majutatud, avalikult kättesaadavaks kõigi objektide laiene CDN serva vahemällu. Kui muudate objekti, mis on praegu vahemälus on CDN-ID, uue sisu ei ole saadaval kaudu soovitud CDN seni, kuni soovitud CDN värskendab selle sisu aegumisel vahemällu talletatud sisu aja live perioodi.

**On uus CDN lõpp-punkti loomiseks salvestusruumi konto jaoks**

1. [Azure'i klassikaline portaali]navigeerimispaanil, klõpsake **CDN**.

2. Klõpsake lindil nuppu **Uus**. Valige dialoogiboksis **Uus** **Rakendus teenuseid**, siis **CDN-ID**, siis **Kiiresti luua**.

3. Valige rippmenüüst **Origin domeeni** loodud eelmises jaotises loendist konto saadaolevat salvestusruumi konto. 

4. Uue lõpp-punkti loomiseks nuppu **Loo** .

5. Kui lõpp-punkti on loodud, kuvatakse see loendis tellimuse lõpp-punktid. Loendivaate näitab URL-i abil vahemällu talletatud sisu, samuti origin domeeni. 

    Origin domeen on asukohta, kust on CDN vahemälu sisu. Origin domeeni võib olla salvestusruumi konto või pilveteenuses; Käesolevas näites kasutatakse salvestusruumi konto. Salvestusruumi sisu on vahemällu Edge'i serverid vastavalt olla säte, mis teie määratud või vaikimisi heuristika vahemällu võrgu. 


    > [AZURE.NOTE] Lõpp-punkti jaoks loonud konfiguratsiooni kohe on kättesaadavad; võib kuluda kuni 60 minutit registreerimise levitada CDN võrgu kaudu. Kasutajad, kes proovida kasutada CDN domeeninime kohe saada kuni sisu on saadaval on CDN olekukoodi 400 (vigane päring).

<a id="Step3"> </a>
<h2>Samm 3: Accessi CDN sisu</h2> 

Vahemällu talletatud sisu on CDN juurdepääsuks kasutada portaalis esitatud CDN URL. Vahemällu salvestatud bloobimälu aadress on järgmine:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Samm 4: Selle CDN sisu eemaldamine</h2>

Kui te ei soovi enam vahemälu objekti sisse Azure'i sisu kohaletoimetamise võrk (CDN), tehke ühte järgmistest:

-   Jaoks soovitud Azure'i bloobimälu saate kustutada selle bloobimälu avaliku ümbrises.
-   Saate teha ümbris asemel era. Lisateabe saamiseks vaadake [Ümbriste ja plekid juurdepääsu piirata](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Saate keelata või haldusportaali abil CDN lõpp-punkti kustutamine.
-   Saate muuta oma majutatud teenuse vastata enam objekti.

Objekti, mis on juba vahemälus on CDN jääb vahemällu talletatud, kuni aeg live objekti lõpeb. Aja live perioodi aegumisel selle CDN kontrollib kas CDN lõpp-punkti on veel kehtib ja anonüümselt pääseb objekti. Kui pole, siis objekt pole enam salvestatakse vahemällu.

Praegu ei toetata võimalus kohe likvideerite sisu Azure'i haldusportaal. Võtke ühendust [Azure tugiteenuste](https://azure.microsoft.com/support/options/) kui teil on vaja kohe likvideerite sisu. 

## <a name="additional-resources"></a>Lisaressursid

-   [Kuidas Azure osaleja rühma loomine]
-   [Kohta: Azure'i tellimuse salvestusruumi kontod haldamine]
-   [Teenusehaldus API kohta]
-   [Kuidas CDN sisu vastendamiseks kohandatud domeeni]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure'i CDN sõlm asukohad]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure'i klassikaline portaal]: https://manage.windowsazure.com/
  [arvelduse kavandamine]: /pricing/calculator/?scenario=full
  [Kuidas Azure osaleja rühma loomine]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Teenusehaldus API kohta]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Kuidas CDN sisu vastendamiseks kohandatud domeeni]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
