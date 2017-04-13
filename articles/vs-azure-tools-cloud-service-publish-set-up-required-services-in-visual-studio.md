<properties
   pageTitle="Ettevalmistused avaldamine või juurutada Azure rakenduse Visual Studio | Microsoft Azure'i"
   description="Siit saate teada, pilveteenuste ja salvestusruumi konto teenuste häälestada ja konfigureerida Azure rakenduse toiminguid."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Avaldamine või juurutamine Visual Studio Azure'i rakendust ettevalmistamine

## <a name="overview"></a>Ülevaade

Teenus cloud projekti avaldamiseks peate häälestama järgmistest teenustest:

- **Pilveteenuses** käivitamiseks oma rollid Azure keskkonnas

- Mõne **salvestusruumi konto** , mis võimaldab juurdepääsu teenustele bloobimälu, järjekord ja tabel.

Nende teenuste häälestamiseks ja rakenduse konfigureerimiseks tehke järgmist


## <a name="create-a-cloud-service"></a>Looge pilveteenus

Azure'i pilveteenus avaldamiseks, peate esmalt looma pilveteenuses, mis töötab teie rollid Azure keskkonnas. Saate luua pilveteenus [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885), nagu on kirjeldatud jaotises **pilveteenus Azure klassikaline portaali abil luua**, käesoleva teema. Saate luua ka pilveteenus Visual Studio avaldamise viisardi abil.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Visual Studio abil pilveteenus loomiseks

1. Azure'i projekti kiirmenüü avamine ja valige käsk **Avalda**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Kui te pole sisse logitud Microsofti konto või ettevõtte konto, mis on seotud Azure tellimuse oma kasutajanime ja parooliga sisse logida.

1. Valige eelnevalt lehel **sätted** nuppu **edasi** .

    ![Publishing Wizard levinud sätted](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Valige loendis **Pilveteenustega** **Loo uus**. Kuvatakse dialoogiboks **Azure teenuste loomine** .

1. Sisestage nimi oma pilveteenuses. Nime osa URL-i teenust ja seega peab olema kordumatu globaalselt. Funktsiooni nimi pole tõstutundlik.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Loomiseks pilveteenus Azure klassikaline portaali abil

1. Logige sisse veebisaidil Microsoft [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (valikuline) Pilveteenuste, mille olete juba loonud loendi kuvamiseks valige lehe vasakus servas pilveteenustega link.

1. Valige soovitud **+** ikooni alumises vasakus nurgas ja seejärel klõpsake kuvatavas menüüs valige **Pilveteenuses** . Kaks võimalust, **Kiire** ja **Kohandatud loomine**, teisel ekraanil kuvatakse. Kui otsustate, et **Kiiresti luua**, saate luua ainult määrates selle URL-i ja piirkond, kuhu see on füüsilise majutatud mõnda pilveteenusesse. Kui otsustate, et **Luua kohandatud**, võite kohe avaldada pilveteenus, määrates pakett (.cspkg fail), konfiguratsioonifail (.cscfg) ja serdi. Kohandatud loomine pole nõutav, kui kavatsete avaldada oma pilveteenuses Azure'i projekti käsuga **Avalda** . Käsk **Avalda** on saadaval, klõpsake kiirmenüü käsku Azure'i projekti.

1. Valige hiljem avaldada oma pilveteenuses Visual Studio abil **Kiiresti luua** .

1. Määrake oma pilveteenuses nimi. Täielik URL kuvatakse nime kõrval.

1. Valige loendis piirkond, kus asub enamik kasutajaid.

1. Valige akna allosas linki **Loomine pilveteenuses** .

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

Salvestusruumi konto pakub juurdepääsu teenustele bloobimälu, järjekord ja tabel. Visual Studio või [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkId=253103)abil saate luua salvestusruumi konto.

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Salvestusruumi konto loomiseks Visual Studio abil

1. **Solution Exploreris** **salvestusruumi** sõlme kiirmenüü avamine ja valige **Salvestusruumi konto luua**.

    ![Azure'i salvestusruumi uue konto loomine](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Valige või sisestage järgmine teave salvestusruumi uue konto **Loomine salvestusruumi konto** dialoogiboksis.
    - Azure'i tellimus, millele soovite lisada salvestusruumi konto.
    - Uue salvestusruumi konto jaoks soovitud nimi.
    - Piirkonna või osaleja rühma (nt Lääne USA või Ida-Aasia).
    - Tüüp, mida soovite kasutada konto salvestusruumi, nt geograafilise liigne dispersioonanalüüs.

1. Kui olete lõpetanud, klõpsake käsku **Loo**. Uue salvestusruumi konto kuvatakse loendis **salvestusruumi** **Server**Explorer.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Salvestusruumi konto loomiseks Azure klassikaline portaali abil

1. Logige sisse veebisaidil Microsoft [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (Valikuline) Salvestusruumi kontode vaatamiseks valige lehe vasakus servas paanil **salvestusruumi** link.

1. Valige lehe vasakus ülanurgas olevat **+** ikooni.

1. Klõpsake kuvatavas menüüs valige **salvestusruumi**ja valige **Kiiresti luua**.

1. Sisestage salvestusruumi konto nime, mille tulemuseks on kordumatu URL-i.

1. Pange oma pilveteenuses nimi. Täielik URL kuvatakse nime kõrval.

1. Valige piirkondade loendiga, kus enamik kasutajaid asuvad piirkonnas.

1. Määrake, kas soovite lubada geo-kopeerimine. Kui lubate geo-dispersioonanalüüs, salvestatakse teie andmete füüsilise mitmest vähendada kaotsimineku võimalust. See funktsioon võimaldab salvestusruumist rohkem, kuid vähendada, võimaldades geograafilise asukoha asemel lisamise funktsiooni hiljem salvestusruumi konto loomisel. Lisateavet leiate teemast [Geo-dispersioonanalüüs](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Valige akna allosas linki **Loo salvestusruumi konto** .

Pärast teie salvestusruumi konto loomist kuvatakse abil saate kasutada ressursse Azure storage teenused ja esmaseid ja teiseseid kiirklahvide teie konto jaoks URL-id. Järgmiste klahvide abil autentida taotlusi salvestusruumi teenuste eest.

>[AZURE.NOTE] Teisene kiirklahv pakub sama juurdepääsu konto salvestusruumi esmane kiirklahv ja luuakse varukoopiana teie peamine kiirklahv kahjustada. Lisaks on soovitatav, et te taastada oma kiirklahvide regulaarselt. Saate muuta ühenduse string sätet kasutada teisese võtme ajal saate taastada primaarvõtme, siis saate muuta selle kasutamise ajal saate taastada teisese võtme regenereeritud primaarvõti.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Salvestusruumi konto pakutavaid teenuseid kasutada rakenduse konfigureerimine

Mis tahes rolli, mis kasutab salvestusruumi teenuseid kasutada Azure storage teenused, mida olete loonud tuleb konfigureerida. Selleks saate kasutada mitme teenuse konfiguratsioone Azure'i projekti jaoks. Vaikimisi luuakse kaks Azure'i projekti. Mitme teenuse konfiguratsioone abil saate kasutada sama ühendusstringi koodi, kuid on ühendusstringi muu väärtuse iga teenuse konfigureerimine. Näiteks saate kasutada ühe teenuse konfigureerimine käivitamiseks ja silumine kohalik abil Azure storage emulaator rakenduse ja muu teenuse konfigureerimine rakenduse Azure avaldada. Teenuse konfiguratsioone kohta leiate lisateavet teemast [Konfigureerimine oma Azure'i projekti abil mitme teenuse konfiguratsioone](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Rakenduse kasutamiseks salvestusruumi konto sisaldab konfigureerimine

1. Avage Visual Studio teie Azure lahendus. Solution Exploreris iga rolli kohta kiirmenüü avamine Azure projektis, mis kasutab salvestusruumi teenuseid ja valige **Atribuudid**. Lehe roll nimi kuvatakse Visual Studio redaktoris. Kuvatakse menüü **konfiguratsiooni** väljad.

1. Atribuut lehtede rolli, valige **sätted**.

1. Valige loendist **Teenuse** nime teenuse konfiguratsiooni, mida soovite redigeerida. Kui soovite muuta kõik selle rolli jaoks teenuse konfiguratsiooni, saate valida **Kõik konfiguratsioone**.  Teenuse konfiguratsioone värskendamise kohta leiate lisateavet jaotisest **Haldamine ühendusstringi salvestusruumi kontod** teemas [konfigureerimine rollide jaoks Azure'i pilveteenuses, Visual Studio abil](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Mis tahes ühenduse stringi sätete muutmiseks valige **…** selle sätte kõrval olevat nuppu. Kuvatakse dialoogiboks **Talletusmahu ühendusstringi loomine** .

1. Klõpsake jaotises **Loo ühendus abil**, valige suvand **tellimuse** .

1. Valige loendis **tellimus** oma tellimuse. Kui tellimuste loendis ei sisalda seda, mida soovite, valige **Alla laadida avalda sätete** link.

1. **Kas konto nime** loendis Valige oma salvestusruumikonto nimi. Azure'i tööriistad hangib salvestusruumi konto identimisteave automaatselt .publishsettings-faili abil. Salvestusruumi konto identimisteave käsitsi määrata, valige suvand **käsitsi sisestada mandaat** ja seejärel jätkake selle toiminguga. [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/p/?LinkID=213885)saate avada oma salvestusruumikonto nimi ja primaarvõti. Kui te ei soovi määrata salvestusruumi Kontosätted käsitsi, valige dialoogiboksi sulgemiseks nuppu **OK** .

1. Valige link **Enter salvestusruumi konto** identimisteave.

1. Tippige väljale **konto nimi** oma salvestusruumi konto nimi.

    >[AZURE.NOTE] [Azure'i klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)sisse logida ja seejärel klõpsake nuppu **talletusmahu** . Portaali kuvatakse salvestusruumi kontode loend. Kui valite konto, avatakse seda lehte. Sellel lehel saate kopeerida salvestusruumi konto nimi. Kui kasutate eelmise versiooni klassikaline portaali konto salvestusruumi nimi kuvatakse vaates **Salvestusruumi kontod** . Kopeerige see nimi, esile see vaade aknas **Atribuudid** ja valige seejärel klahvikombinatsiooni Ctrl + c. võtmed. Kleebi nimi Visual Studio, valige väljale **konto nimi** ja valige seejärel klahve Ctrl + V.

1. Väljale **konto võti** , sisestage oma primaarvõti, või kopeerida ja kleepida [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).
    See võti kopeerimine

    1. Asjakohased konto lehe allosas nuppu **Halda võtmed** .

    1. **Klahvid juurdepääsu haldamine** lehel Valige esmane kiirklahv tekst ja valige klahve Ctrl + C.

    1. Azure'i tööriistad, kleepige väljale **konto võti** võti.

    1. Valige üks järgmistest variantidest määrata, kuidas teenuse pääsevad salvestusruumi konto:
        - **Kasutage HTTP**. See on standardne valik. Näiteks `http://<account name>.blob.core.windows.net`.
        - **Kasuta HTTPS** turvalist ühendust. Näiteks `https://<accountname>.blob.core.windows.net`.
        - **Saate määrata kohandatud lõpp-punktid** kõigi kolme teenuste. Kindla teenuse väljale saate tippida siis nende lõpp-punktid.

        >[AZURE.NOTE] Kui loote kohandatud lõpp-punktid, saate luua keerukamaid ühendusstring. Kui kasutate seda stringi vormingut, saate määrata salvestusruumi teenuse lõpp-punktid, mis sisaldavad kohandatud domeeni nime, mille olete registreerunud bloobimälu teenuse konto salvestusruumi. Samuti saate anda juurdepääsu ainult ühe ümbrises kaudu ühiskasutusse antud juurdepääs signatuuri bloobimälu ressursid. Lisateavet selle kohta, kuidas luua kohandatud lõpp-punktid, vt [Konfigureerimine Azure'i salvestusruumi ühendusstringi](storage-configure-connection-string.md).

1. Ühenduse stringi muudatuste salvestamiseks klõpsake nuppu **OK** ja seejärel valige tööriistaribal nuppu **Salvesta** . Pärast nende muudatuste salvestamiseks saate avada selle ühendusstringi väärtus koodi [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx)abil. Azure'i rakenduse avaldamisel valige Azure storage konto jaoks ühendusstringi sisaldava teenuse konfiguratsiooni. Pärast seda, kui teie taotlus on avaldatud, veenduge, et rakendus töötab ootuspäraselt Azure storage teenuste eest

## <a name="next-steps"></a>Järgmised sammud

Rakenduste avaldamine Azure'i Visual Studio kohta lisateabe saamiseks lugege teemat [avaldamise pilveteenus Azure tööriistade abil](vs-azure-tools-publishing-a-cloud-service.md).
