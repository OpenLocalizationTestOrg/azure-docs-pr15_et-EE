<properties
    pageTitle="Azure'i bloobimälu ressursid salvestusruumi Exploreriga (eelvaade) haldamine | Microsoft Azure'i"
    description="Azure'i bloobimälu ümbriste ja plekid salvestusruumi Exploreriga (eelvaade) haldamine"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Azure'i bloobimälu ressursid salvestusruumi Exploreriga (eelvaade) haldamine

## <a name="overview"></a>Ülevaade

[Azure'i bloobimälu](./storage/storage-dotnet-how-to-use-blobs.md) on teenus salvestamiseks suure hulga struktureerimata andmed, nt teksti või binaarsed andmed, millele pääseb juurde kõikjalt maailmas HTTP- või HTTPS-i kaudu.
Saate bloobimälu avalikult maailma andmete esitamist või privaatselt rakenduse andmete talletamiseks. Selles artiklis saate teada, kuidas kasutada töötamiseks bloobimälu ümbriste salvestusruumi Exploreri (eelvaade) ja plekid.

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks peate järgmist.

- [Laadige alla ja installige salvestusruumi Explorer (eelvaade)](http://www.storageexplorer.com)
- [Ühenduse loomine Azure storage konto või teenuse](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Bloobimälu container loomine

Kõik plekid peab asuma bloobimälu ümbrises, mis on lihtsalt loogiline rühmitamine plekid. Konto võib sisaldada piiramatu arvu ümbriste ja iga saate talletada plekid piiramatu arv.

Järgnevalt kirjeldatakse, kuidas luua bloobimälu container salvestusruumi Explorer (eelvaade).

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi konto, mille jooksul soovite luua bloobimälu ümbrises.
1.  Paremklõpsake **Bloobimälu ümbriste**ja - valige kontekstimenüüst - **Bloobimälu Container loomine**.

    ![Looge bloobimälu ümbriste kontekstimenüü][0]

1.  Tekstivälja kuvatakse **Bloobimälu ümbriste** kausta alla. Sisestage oma bloobimälu container nimi. Jaotisest [Container nime reeglite](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) loendi reeglite ja piirangute nimetamine bloobimälu ümbriste.

    ![Looge bloobimälu ümbriste tekstiväli][1]

1.  Vajutage klahvi **Enter** kui valmis luua bloobimälu container või tühistamiseks **paoklahvi (ESC)** . Kui bloobimälu container on loodud, kuvatakse see **Bloobimälu ümbriste** kausta valitud salvestusruumi konto all.

    ![Bloobimälu Container loodud][2]

## <a name="view-a-blob-containers-contents"></a>Bloobimälu container sisu kuvamine

Bloobimälu ümbriste sisaldavad plekid ja kaustade (mis võib sisaldada ka plekid).

Järgmised juhised selgitavad, kuidas bloobimälu container salvestusruumi Explorer (eelvaade) sisu kuvamine:

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu s.o ümbris, mille soovite vaadata.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Paremklõpsake bloobimälu container, mida soovite vaadata, ja - valige kontekstimenüüst - **Avatud bloobimälu Container redigeerija**.
Võite ka topeltklõpsata bloobimälu container, mida soovite vaadata.

    ![Avatud bloobimälu container editor kontekstimenüü][19]

1.  Peamised paanil kuvatakse bloobimälu container sisu.

    ![Bloobimälu container redaktor][3]

## <a name="delete-a-blob-container"></a>Bloobimälu container kustutamine

Bloobimälu ümbriste saate hõlpsalt loodud ja kustutatud vastavalt vajadusele. (Näha, kuidas kustutada üksikuid plekid, vt jaotises [haldamine plekid bloobimälu ümbrises](./#managing-blobs-in-a-blob-container).)

Järgnevalt kirjeldatakse, kuidas kustutada bloobimälu container salvestusruumi Exploreri (eelvaade)

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu s.o ümbris, mille soovite vaadata.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Paremklõpsake bloobimälu s.o ümbris, mille soovite kustutada, ja - kontekstimenüüst – valige **Kustuta**.
Võite vajutada ka **kustutada** praegu valitud bloobimälu kustutada.

    ![Kustutage bloobimälu container kontekstimenüü][4]

1.  Valige kinnituse dialoogiboksis **Jah** .

    ![Container kinnituse bloobimälu kustutamine][5]

## <a name="copy-a-blob-container"></a>Kopeerige bloobimälu container

Salvestusruumi Explorer (eelvaade) võimaldab teil bloobimälu container kopeerimine lõikelauale ja kleepige see bloobimälu container salvestusruumi teisele kontole. (Kopeerimine üksikute plekid vaatamiseks viidata jaotises [haldamine plekid bloobimälu ümbrises](./#managing-blobs-in-a-blob-container).)

Järgmised juhised selgitavad, kuidas bloobimälu container ühe salvestusruumi konto teise kopeerida.

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu s.o ümbris, mille soovite kopeerida.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Paremklõpsake bloobimälu s.o ümbris, mille soovite kopeerida, ja - kontekstimenüüst – valige **Kopeeri bloobimälu ümbris**.

    ![Kopeeri bloobimälu container kontekstimenüü][6]

1.  Paremklõpsake soovitud "target" salvestusruumi konto, kuhu soovite kleepida bloobimälu ümbris ja - valige kontekstimenüüst - **Bloobimälu Container kleepida**.

    ![Kleebi bloobimälu container kontekstimenüü][7]

## <a name="get-the-sas-for-a-blob-container"></a>Hankida muude bloobimälu container

[Ühiskasutusega juurdepääsu allkirja (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) pakub delegeeritud juurdepääsu teie salvestusruumi konto ressursse.
See tähendab, et saate anda mõnda muud klienti piiratud õiguste objektidele teie salvestusruumi konto määratud perioodi ja määratud kogumi õigused ilma jagada oma konto kiirklahvide.

Järgnevalt kirjeldatakse, kuidas luua SAS, bloobimälu container jaoks:

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu ümbris, mille jaoks soovite saada on SAS.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Paremklõpsake soovitud bloobimälu ümbris ja - valige kontekstimenüüst - **Saada ühiskasutusse Accessi allkirja**.

    ![Saada SAS kontekstimenüü][8]

1.  **Ühiskasutusse antud juurdepääs signatuur** dialoogiboksis Määrake poliitika, algus-ja aegumise, ajavöönd ja juurdepääsu ressursile jaoks soovitud tasemed.

    ![Saate SAS suvandid][9]

1.  Kui olete lõpetanud, SAS suvandite, valige **Loo**.

1.  Seejärel kuvatakse teise **Ühiskasutusse antud juurdepääs signatuur** dialoogiboksis bloobimälu ümbris koos URL-i ja QueryStrings abil saate salvestusruumi ressursile juurdepääsu loendiga.
Valige **Kopeeri** lõikelauale kopeerimist URL-i kõrval.

    ![Kopeerige SAS URL-id][10]

1.  Kui lõpetanud, klõpsake nuppu **Sule**.

## <a name="manage-access-policies-for-a-blob-container"></a>Bloobimälu container Accessi poliitikate haldamine

Järgnevalt kirjeldatakse, kuidas hallata (lisada ja eemaldada) juurde bloobimälu container poliitika:

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu container nime, kelle soovite hallata juurdepääsupoliitikaid.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Valige soovitud bloobimälu ümbris ja - valige kontekstimenüüst - **Juurdepääsu poliitikate haldamine**.

    ![Kontekstimenüü Accessi poliitikate haldamine][11]

1.  Dialoogiboksi **Juurdepääsupoliitikaid** nimekirja mis tahes juurdepääsupoliitikaid valitud bloobimälu container juba loonud.

    ![Accessi poliitika suvandid][12]        

1.  Sõltuvalt Accessi poliitika halduse tööülesande tehke järgmist.

    - **Lisa uue poliitika Accessi** – valige **Lisa**. Pärast loodud, kuvatakse dialoogiboks **Juurdepääsupoliitikaid** äsja lisatud Accessi poliitika (koos vaikesätted).
    - **Accessi poliitika redigeerimine** - tehke soovitud muudatused ja valige **Salvesta**.
    - **Accessi poliitika eemaldamine** – valige **Eemalda** kõrval juurdepääsu poliitika, mida soovite eemaldada.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Bloobimälu container avaliku juurdepääsutase määramine

Vaikimisi on seatud iga bloobimälu container "Avaliku juurdepääsu".

Järgmised toimingud selgitavad, kuidas määrata bloobimälu container avaliku juurdepääsutase.

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu container nime, kelle soovite hallata juurdepääsupoliitikaid.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Valige soovitud bloobimälu ümbris ja - valige kontekstimenüüst - **Seadmine avaliku juurdepääsutase**.

    ![Määramine, avaliku juurdepääsu taseme kontekstimenüü][13]

1.  **Container avaliku juurdepääsu taseme määramine** dialoogiboksis määrake soovitud juurdepääsutase.

    ![Avaliku juurdepääsu taseme suvandite määramine][14]

1.  Valige **Rakenda**.

## <a name="managing-blobs-in-a-blob-container"></a>Plekid bloobimälu ümbrises haldamine

Kui olete loonud bloobimälu container, on bloobimälu üles selle bloobimälu container, on bloobimälu kohalikku arvutisse allalaadimiseks, avage soovitud bloobimälu kohalikus arvutis ja palju muud.

Järgnevalt kirjeldatakse, kuidas hallata plekid (ja kaustad) bloobimälu pakendi sees.

1.  Avatud salvestusruumi Explorer (eelvaade).
1.  Laiendage vasakpoolsel paanil salvestusruumi kontot, mis sisaldab bloobimälu s.o ümbris, mille soovite hallata.
1.  Laiendage salvestusruumi konto **Bloobimälu ümbriste**.
1.  Topeltklõpsake bloobimälu container, mida soovite vaadata.
1.  Peamised paanil kuvatakse bloobimälu container sisu.

    ![Container vaade bloobimälu][3]

1.  Peamised paanil kuvatakse bloobimälu container sisu.

1.  Sõltuvalt tööülesande, mida soovite teha, tehke järgmist.

    - **Failide üleslaadimine bloobimälu container**

        1.  Peamised paanil Valige tööriistaribal **üles laadida**, ja seejärel **Laadige failid** rippmenüüst menüü kaudu.

            ![Laadige failid menüü][15]

        1.  **Failide üleslaadimine** dialoogiboksis nuppu kolmikpunkti (…****) paremas servas kuvatakse **failide** tekstivälja valige failid, mida soovite üles laadida.

            ![Laadige failid suvandid][16]

        1.  Määrake **bloobimälu**tüüpi. [Alustamine Azure'i bloobimälu kasutades .net-i](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) artikkel selgitab bloobimälu erinevate erinevusi.

        1.  Soovi korral saate määrata, kuhu valitud failid laaditakse sihtkausta. Kui kausta pole olemas, luuakse see.

        1.  Märkige **üles laadida**.

    - **Bloobimälu container kausta üles**

        1.  Peamised paanil Valige tööriistaribal **üles laadida**, ja seejärel **Laadida kausta** rippmenüüst menüü kaudu.

            ![Üleslaadimise menüü kaust][17]

        1.  Dialoogiboksis **kaust üles** nuppu kolmikpunkti (…****) paremas servas tekstivälja **kausta** valimine kaust, mille sisu soovite üles laadida.

            ![Laadige Kaustasuvandid][18]

        1.  Määrake **bloobimälu**tüüpi. [Alustamine Azure'i bloobimälu kasutades .net-i](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) artikkel selgitab bloobimälu erinevate erinevusi.

        1.  Soovi korral saate määrata, kuhu valitud kausta sisu laaditakse sihtkausta. Kui kausta pole olemas, luuakse see.

        1.  Märkige **üles laadida**.

    - **Kohalikku arvutisse allalaadimiseks on bloobimälu**

        1.  Valige bloobimälu, mida soovite alla laadida.

        1.  Valige tööriistaribal peamised paani **alla laadida**.

        1.  Määrake dialoogiboksis **määrata, kuhu soovite salvestada allalaaditud bloobimälu** bloobimälu, laaditakse alla soovitud asukoht ja te soovite anda sellele nimi.  

        1.  Valige **Salvesta**.

    - **Avage soovitud bloobimälu oma kohalikus arvutis**

        1.  Valige bloobimälu, mida soovite avada.

        1.  Valige tööriistaribal peamised paani **avamine**.

        1.  Funktsiooni bloobimälu laaditakse alla ja avada selle bloobimälu aluseks oleva faili tüüp seotud rakenduse abil.

    - **Mõne bloobimälu kopeerimine lõikelauale**

        1.  Valige bloobimälu, mida soovite kopeerida.

        1.  Peamised paani, valige tööriistaribal käsk **Kopeeri**.

        1.  Vasakul paanil, liikuge teise bloobimälu container ja topeltklõpsake seda vaadata see peamine paani.

        1.  Valige tööriistaribal peamised paani **Kleebi** soovitud bloobimälu koopia loomiseks.

    - **Kustutamine on bloobimälu**

        1.  Valige bloobimälu, mille soovite kustutada.

        1.  Valige tööriistaribal peamised paani **kustutada**.

        1.  Valige kinnituse dialoogiboksis **Jah** .

## <a name="next-steps"></a>Järgmised sammud

- Saate vaadata [uusima salvestusruumi Explorer (eelvaade) vabastage märkmete ja videod](http://www.storageexplorer.com).
- Siit saate teada, kuidas luua [rakendusi Azure plekid, tabelid, järjekordade ja failide kasutamise](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png