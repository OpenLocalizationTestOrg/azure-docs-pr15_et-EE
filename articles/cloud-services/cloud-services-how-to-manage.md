<properties 
    pageTitle="Levinud cloud teenuste haldamisega seotud toiminguid (klassikaline) | Microsoft Azure'i" 
    description="Saate teada, kuidas hallata pilveteenustega Azure klassikaline portaalis." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Kuidas hallata pilveteenustega

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-manage-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-manage.md)

Azure'i klassikaline portaali alal **Pilveteenustega** saate värskendada teenuse rolli või juurutamine, edendada etapiviisilise juurutamise tootmisele, linkida ressursid oma pilveteenusesse, nii et saate vaadata ressursi sõltuvused ja ressursside koos mastaapimiseks ja pilveteenus või juurutamine kustutamine.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Kuidas: värskendamine pilvepõhise teenuse rolli või juurutamine

Kui peate värskendama oma pilveteenuses rakenduse kood, kasutage armatuurlaud, **Pilveteenustega** lehe või **eksemplari** lehe **värskendada** . Saate värskendada kindla rolliga või kõikide rollide. Peate uue teenuse paketi ja teenuse konfiguratsiooni faili üleslaadimiseks.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)armatuurlaud, **Pilveteenustega** lehe või **eksemplari** lehel klõpsake nuppu **Värskenda**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Juurutamise sildi**, sisestage nimi (nt mycloudservice4) juurutamise tuvastamiseks. Leiate jaotises **Kiirjuhend** juurutamise sildi armatuurlaual.

3. **Paketi**, kasutage **sirvimine** üles laadida alla fail (.cspkg).

4. **Konfiguratsiooni**, abil saate **sirvida** teenuse konfigureerimine faili (.cscfg) üles laadida.

5. **Roll**, valige **Kõik** , kui soovite täiendada kõikide rollide pilveteenusesse. Kindla rolliga update, märkige roll, mida soovite värskendada. Isegi juhul, kui valite kindla rolliga värskendada, värskendusi teenuse konfiguratsioonifailis on rakendatud kõik rollid.

6. Kui värskenduse muudab rollide arvu või mis tahes rolli suurust, märkige ruut **Luba Värskenda rolli suurused või rollide arvu muutumisel** , jätkamiseks värskenduse lubamiseks. 

    Pange tähele, et rolli (ehk teisisõnu öeldes virtuaalse masina rolli eksemplari majutavas suurus) või rollide arvu suuruse muutmisel iga rolli eksemplari (virtuaalne kohapeal) peab olema uuesti imaged ja mis tahes kohalikud andmed lähevad kaotsi.

7. Kui mis tahes teenuse rollid ainult ühel rolli, valige soovitud **värskendada ka siis, kui üks või mitu rolli sisaldavad ühekordsest märkeruudu** jätkamiseks versiooniuuenduse lubamine. 

    Azure'i saate garanteeri ainult 99,95% teenuse kättesaadavus pilvepõhise teenuse värskendamise ajal, kui iga rolli on vähemalt kaks rolli aknad (virtuaalmasinates). Mis võimaldab ühe virtuaalse masina töödelda kliendi taotlusi teise värskendamise ajal.

8. Alustamiseks klõpsake nuppu **OK** (märge) teenuse värskendamine.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Kuidas: Vaheta juurutuste edendada tootmise etapiviisilise juurutamine

Kasutage **Vaheta** edendada lavastus kasutuselevõtu pilveteenus tootmisele. Kui otsustate uue versiooni pilveteenus juurutada, saate etapp ja testida oma uue väljaande pilvepõhise teenuse lavastus keskkond ajal oma klientidele valmistamisel ning praeguse versiooni kasutate. Kui olete valmis edendada tootmise uus versioon, saate **vahetada** URL-id, mis on adresseeritud kahe juurutuse aktiveerimiseks. 

Saate vahetada juurutuste **Pilveteenustega** lehelt või armatuurlaud.

1. Klõpsake [Azure klassikaline portaali](https://manage.windowsazure.com/) **Pilveteenustega**.

2. Klõpsake loendis pilveteenuste pilveteenuses selle valimiseks.

3. Klõpsake nuppu **Vaheta**.

    Kinnituse järgmine viip avatakse.

    ![Cloud Services vahetamine](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Pärast kontrollida juurutamise teavet, klõpsake nuppu **Jah** kasutuselevõttu vahetada.

    Juurutamise Vaheta juhtub kiiresti kuna ainus asi, mis muudab on virtuaalse IP-aadressid (VIP) soovitud juurutuste.

    Arvuta kulud salvestamiseks saate kustutada juurutamise lavastus keskkonnas kui olete kindel, et uus tootmise juurutamise toimib ootuspäraselt.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Kuidas: pilveteenus ressursi linkimine

Kuvada oma pilvepõhise teenuse sõltuvused muud ressursid, saate eksemplari Azure'i SQL-andmebaasi või salvestusruumi kontoga linkida pilveteenusesse. Saate link ja linkimise tühistamine ressursid lehel **Lingitud ressursid** ja seejärel jälgida oma kasutamine armatuurlaual pilvepõhise teenuse. Kui lingitud salvestusruumi konto on jälgimise sisse lülitatud, saate jälgida kokku taotlusi teenus cloud armatuurlaual.

Uue või olemasoleva SQL-andmebaasi eksemplari või salvestusruumi konto linkimine oma pilveteenuses **lingi** kaudu. Seejärel muudate andmebaas koos pilvepõhise teenuse roll, mis kasutab lehel **skaala** . (Salvestusruumi konto skaala automaatselt kui kasutus suureneb.) Lisateavet leiate teemast [mastaapimiseks pilveteenus ja lingitud ressursid](cloud-services-how-to-scale.md). 

Samuti saate jälgida, haldamine ja mastaapimiseks andmebaasi **andmebaaside** sõlme Azure klassikaline portaali. 

"Linkimise" ressursi selles mõttes ei saa ühendust oma rakenduse ressursile. Kui loote uue andmebaasi **lingi**kaudu, peate ühenduse stringide lisamine oma rakenduse koodi ja seejärel soovitud pilveteenuses. Samuti peate ühendusstringi lisada, kui teie rakendus kasutab vahendid lingitud salvestusruumi konto.

Järgnevalt kirjeldatakse, kuidas link uus SQL-andmebaasi eksemplari juurutatud uus SQL-andmebaasi server, pilveteenusesse.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>SQL-andmebaasi eksemplari linkimiseks pilveteenus

1. Klõpsake [Azure klassikaline portaali](http://manage.windowsazure.com/) **Pilveteenustega**. Klõpsake selle nime pilveteenuses avamiseks armatuurlaud.

2. Klõpsake nuppu **lingitud ressursid**.

    Avaneb leht **Lingitud ressursid** .

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klõpsake **linki ressursi** või **lingi**.

    Käivitub **Link ressursi** viisard.

    ![Link Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klõpsake nuppu **Loo uus ressurss** või **lingi mõne olemasoleva ressursi**.

5. Valige link ressursi tüüp. [Azure'i klassikaline portaali](http://manage.windowsazure.com/), klõpsake nuppu **SQL-andmebaasi**. (Preview Azure klassikaline portaali ei toeta linkimist salvestusruumi konto pilveteenusesse.)

6. Andmebaasi konfigureerimine lõpuleviimiseks järgige juhiseid **SQL andmebaase** ala spikker Azure klassikaline portaali.

    Te saate jälgida linkimistoiming sõnumi alale.

    ![Link edenemine](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Kui linkimine on lõpule jõudnud, saate jälgida lingitud ressurssi armatuurlaual pilvepõhise teenuse olekut. Skaleerimist lingitud SQL-andmebaasi kohta leiate teavet teemast [mastaapimiseks pilveteenus ja lingitud ressursid](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Lingitud ressursi link

1. Klõpsake [Azure klassikaline portaali](http://manage.windowsazure.com/) **Pilveteenustega**. Klõpsake selle nime pilveteenuses avamiseks armatuurlaud.

2. Klõpsake **Lingitud ressursid**ja seejärel valige ressurss.

3. Klõpsake nuppu **Tühista linkimine**. Klõpsake kinnitusviibas **Jah** .

    SQL-andmebaasi vahelise lingi ei mõjuta andmebaasi või rakenduse ühendused andmebaasi. Siiski saate hallata andmebaasi Azure klassikaline portaali alal **SQL andmebaase** .



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Kuidas: juurutuste ja pilveteenus kustutamine

Enne kustutamist mõnda pilveteenusesse, siis kustutage iga olemasoleva juurutamise.

Arvuta kulud salvestamiseks saate kustutada lavastus juurutamise pärast kontrollimist juurutamise tootmise töötab ootuspäraselt. Teil on hind Arvuta kulud rolli eksemplaride isegi juhul, kui cloud teenus ei tööta.

Järgmiste toimingute abil saate kustutada juurutamine või oma pilveteenuses. 

1. Klõpsake [Azure klassikaline portaali](http://manage.windowsazure.com/) **Pilveteenustega**.

2. Valige pilveteenusesse ja seejärel klõpsake käsku **Kustuta**. (Pilveteenus avamata armatuurlaua valimiseks klõpsake peale pilvepõhise teenuse kirje nimi.)

    Kui teil on lavastus või juurutamine, näete umbes järgmist akna allosas valikuid menüü. Enne kui saate kustutada pilveteenusesse, kustutage kõik olemasolevad juurutuste.

    ![Menüü kustutamine](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Juurutamine kustutamiseks valige **lavastus juurutamise kustutada**või **tootmise juurutamine** . Klõpsake kinnitusviibas, nuppu **Jah**. 

4. Kui plaanite kustutada pilveteenusesse, korrake juhist 3, vajadusel kustutada teiste juurutamise.

5. Pilveteenusesse kustutamiseks valige **Kustuta pilveteenuses**. Klõpsake kinnitusviibas, nuppu **Jah**.

> [AZURE.NOTE]
> Kui Paljusõnaline jälgimine on konfigureeritud lubama oma pilveteenuses, Azure'i kustutage andmed salvestusruumi kontolt pilveteenusesse kustutamisel. Peate andmed käsitsi kustutada. Tabelite mõõdikute otsimise kohta leiate teemast "kohta: Accessi Paljusõnaline väljaspool Azure klassikaline portaali andmete" kuidas [kuvari pilveteenustega](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Järgmised sammud

 * [Üldine konfigureerimine oma pilveteenuses](cloud-services-how-to-configure.md).
* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy.md).
* Saate konfigureerida [kohandatud domeeni nime](cloud-services-custom-domain-name.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate.md)konfigureerimine.
