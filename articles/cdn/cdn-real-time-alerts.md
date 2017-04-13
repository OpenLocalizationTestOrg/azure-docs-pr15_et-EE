<properties
    pageTitle="Azure'i CDN reaalajas teatised | Microsoft Azure'i"
    description="Microsoft Azure'i CDN reaalajas teatised. Reaalajas teated esitada teatised lõpp-punktid profiili CDN jõudluse kohta."
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
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Microsoft Azure'i CDN reaalajas teatised

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Ülevaade

Selles dokumendis selgitatakse Microsoft Azure'i CDN reaalajas teatised. See funktsioon pakub reaalajas teatised lõpp-punktid profiili CDN jõudluse kohta.  Saate häälestada meili või HTTP teatiste põhjal:

* Läbilaskevõime
* Olekukoodi
* Vahemälu olekud
* Ühendused

## <a name="creating-a-real-time-alert"></a>Reaalajas teatise loomine

1. Sirvige [Azure portaali](https://portal.azure.com)CDN profiili.

    ![CDN profiili blade](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

3. Menüü **analüüsi** kursoriga ja seejärel kursoriga **Reaalajas statistika** hüpik.  Klõpsake **reaalajas teatised**.

    ![CDN haldusportaal](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Kuvatakse loend olemasoleva teatiste konfiguratsiooni (vajaduse korral).

4. Klõpsake nuppu **Lisa teatis** .

    ![Teatiste nupp Lisa](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Kuvatakse uus teatis loomise vorm.

    ![Uus teatis vorm](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Kui soovite selle teatise aktiivsetena, kui klõpsate käsku **Salvesta**, märkige ruut **Teavita lubatud** .

6. Sisestage oma teatise kirjeldav nimi väljale **nimi** .

7. Valige rippmenüüst **Media tüüp** **HTTP suur objekt**.

    ![Meediumi tüüp valitud HTTP suur objekt](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Valige **HTTP suur objekti** **Meediumi tüüp**.  **Azure'i CDN Verizon**ei kasuta muid võimalusi.  Tõrge **HTTP suur objekti** valimiseks, põhjustab teie teatise kunagi käivitub.

8. Saate luua **Avaldise** jälgimiseks, valides **meetermõõdustik**, **tehtemärk**, ja **päästik väärtus**.

    - Valige tingimus, mida soovite jälgida tüüp **väärtuseks meetermõõdustik**.  **Läbilaskevõime Mbps** on läbilaskevõime kasutuse ülekandekiirus sisse.  **Kokku ühendused** on meie Edge'i serverid samaaegseid HTTP ühenduste arv.  Erinevate vahemälu olekud ja olekukoodi määratlusi, leiate [Azure'i CDN vahemälu olek koodid](https://msdn.microsoft.com/library/mt759237.aspx) ja [Azure CDN HTTP olek](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Tehtemärk** on tehtemärk, mis loob mõõdiku ja päästik väärtus seos.
    - **Päästik väärtus** on lävi väärtus, mis peavad olema täidetud enne saadetakse teile teatis.

    Klõpsake selle all näiteks avaldis on loodud näitab ma soovite kohaletoimetamisest 404 olekukoodi arv on suurem kui 25.

    ![Reaalajas teatis näidisavaldis](./media/cdn-real-time-alerts/cdn-expression.png)

9. Sisestage **intervall**, kui sageli soovite avaldist hinnata.

10. Valige **Teavita klõpsake** rippmenüü, kui soovite teada, kui avaldis on tõene.
    
    - **Tingimuse käivitamine** näitab, et saadetakse teatis, kui määratud tingimus on avastatud esimest.
    - **Tingimus Lõpeta** näitab, et saadetakse teatis, kui määratud tingimus enam ei tuvasta. See teatis saab käivitada ainult pärast meie võrgu jälgimine süsteemi tuvastatud ilmnenud määratud tingimus.
    - **Pidev** näitab, et teatis saadetakse iga kord, kas võrgu jälgimine süsteemi tuvastab määratud tingimus. Pidage meeles, mis võrgu jälgimine süsteemi kontrollib ainult intervalli jaoks määratud tingimusele kohta üks kord.
    - **Tingimuse algus ja lõpp** näitab, et teatis saadetakse esimest korda, et määratud tingimus on avastatud ja veel kord, kui tingimus pole enam avastatakse.

11. Kui soovite saada teatisi e-posti teel, märkige ruut **Teavita, e-posti teel** .  

    ![Teavitage e-posti vorm](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    Sisestage väljal **Adressaat** meiliaadress, kuhu soovite teatisi saadetud. **Teema** ja **keha**, jätate vaikimisi või kohandate loendis **Saadaolevad märksõnade** abil dünaamiliselt teatise andmete lisamine, kui sõnum on saadetud sõnumi.

    > [AZURE.NOTE] E-posti teate saate kontrollida, klõpsates nuppu **Teatis testida** , kuid ainult siis, kui teatiste konfigureerimine on salvestatud.

12. Kui soovite teatised sisestatakse veebiserverisse, märkige ruut **Teavita HTTP postiga** .

    ![Teavitage HTTP Post vorm](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Sisestage väljale **URL-i** teie soovitud sõnumi HTTP postitatud URL-i. Sisestage tekstiväljale **päised** kutse saatmist HTTP päised.  **Keha** võib kohandada loendis **Saadaolevad märksõnade** abil dünaamiliselt teatise andmete lisamine, kui sõnum on saadetud sõnumi.  **Päise** ja **keha** vaikimisi XML-i last, mis on sarnane on näide.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] HTTP Post teatise saate kontrollida, klõpsates nuppu **Teatis testida** , kuid ainult siis, kui teatiste konfigureerimine on salvestatud.

13. Klõpsake nuppu **Salvesta** salvestada oma teatiste konfigureerimine.  Kui märgitud **Teatise lubatud** juhis 5, oma teatise on nüüd aktiivne.

## <a name="next-steps"></a>Järgmised sammud

- [Reaalajas argument statistikud on Azure CDN](cdn-real-time-stats.md) analüüs
- Tõrkeid süvitsi [Täpsemalt HTTP](cdn-advanced-http-reports.md) aruannetega
- Analüüsi [mustreid](cdn-analyze-usage-patterns.md)

