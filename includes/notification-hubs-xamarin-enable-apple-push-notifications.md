

Tõuketeatiste kaudu Apple Push teatise teenuse (APNS) jaoks rakenduse registreerimiseks peate looma uue tõuketeatised sert, rakenduse ID ja projekti ettevalmistamise profiili Apple'i arendaja portaalis. Rakenduse ID sisaldab konfiguratsioonisätted, mis rakenduse saata ja vastu võtta tõuketeatised lubada. Nende sätete sisaldab tõuketeatised teatis serdi autentida koos Apple Push teatise teenuse (APNs-i) kui saatmise ja vastuvõtu Tõuketeatiste vaja. Lisateavet nende mõisted dokumentatsioonist ametlik [Apple Push teavitusteenuse](http://go.microsoft.com/fwlink/p/?LinkId=272584) .


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Luua tõuketeatised sertifikaadi faili serdi allkirjastamise taotlemine

Need juhised sõelub serdi allkirjastamise taotlus loomine. Seda kasutatakse koos APNs-i serdi tõuketeatised loomiseks.

1. Mac-arvutisse, käivitage tööriist võtmekimbu juurdepääsu. Saab avada **Utiliidid** kaust või **muud** kaustast käivitada numbrinuppe.

2. **Võtmekimbu juurdepääs**, laiendage **Serdi Plaanimisabimees,**klõpsake **taotleda serti sertimisorganilt …**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Valige **Kasutaja meiliaadressi** ja **Levinud nimi** , veenduge, et **kettale salvestatud** oleks märgitud, ja seejärel klõpsake nuppu **Jätka**. Jätke väli **CA e-posti aadressi** tühja ei ole vaja.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Tippige serdi allkirjastamise taotlemine (CSR) faili nimi **Salvesta nimega**, valige asukoht, **kuhu**sisse ja seejärel klõpsake nuppu **Salvesta**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    See salvestab CSR faili valitud asukohta; vaikeasukoht on töölaual. Pidage meeles valitud faili asukoht.


####<a name="register-your-app-for-push-notifications"></a>Teie minirakendus Tõuketeatiste registreerimine

Apple uus konkreetsete rakenduse ID rakenduse loomine ja ka Tõuketeatiste konfigureerimine.  

1. Liikuge [iOS-i Provisioning portaali](http://go.microsoft.com/fwlink/p/?LinkId=272456) aadressil Apple'i Arenduskeskus, logige sisse oma Apple'i ID-ga, **identifikaatorite**, klõpsake nuppu ja seejärel klõpsake **Rakenduse ID-d**ja lõpuks klõpsake soovitud **+** logige sisse registreerida uue rakenduse.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Värskendage järgmised kolm välja jaoks uus rakendus ja seejärel klõpsake nuppu **Jätka**.

    * **Nimi**: sisestage kirjeldav nimi oma rakenduse jaotises **Rakenduse ID kirjeldus** väljale **nimi** .

    * **Komplekt identifikaator**: jaotises **konkreetsete rakenduse ID** Sisestage **Komplekt identifikaator** vormi `<Organization Identifier>.<Product Name>` nimetatud [Rakenduse jaotuse juhend](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). See peab vastama, mida kasutatakse ka XCode, Xamarin või Cordova projekti oma rakenduse.

    * **Tõuketeatised**: **Tõuketeatised** ruudu jaotises **Rakenduse teenused** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Rakenduse ID ekraani kinnitamine sätte üle vaadata ja kui olete need klõpsake nuppu **Edasta**

4.  Pärast uue rakenduse ID-d, kuvatakse **täielik registreerimise** ekraani. Klõpsake nuppu **valmis**.

5. Arenduskeskuse linki, klõpsake jaotises rakenduse ID-d, otsige üles rakenduse ID, mille te just loodud, ja klõpsake selle rea. Rakenduse ID rida klõpsamisel kuvatakse rakenduse üksikasjad. Klõpsake allosas nuppu **Redigeeri** .

6. Liikuge ekraani allservas ja nuppu **Loo sert** jaotises **Arengu Push SSL-sert**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    See kuvab abimehe "IOS-i serdi lisamine".

    > [AZURE.NOTE] Selle õpetuse kasutab arengu sert. Sama toimingut saate kasutada registreerimisel tootmise sert. Lihtsalt veenduge, et teatiste saatmisel kasutada sama serdi tüüp.

7. **Valige**fail, liikuge asukohta, kuhu salvestasite soovitud CSR oma tõuketeatised sert. Seejärel klõpsake nuppu **Loo**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Pärast portaali on loodud sert, klõpsake nuppu **Laadi alla** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    See laadib Allkirjaserdi ja salvestab selle oma arvutisse allalaadimise kaustas.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Vaikimisi allalaaditud faili nimi arengu serdi **aps_development.cer**.

9. Topeltklõpsake allalaaditud push serdi **aps_development.cer**. See installib uut serti võtmekimbu, nagu allpool näidatud:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Teie serdi nimi võib olla erinev, kuid see on eesliide **Push teenuste arengu Apple iOS-i:**.

10. Paremklõpsake võtmekimbu Accessis uus tõuketeatised sert, mille äsja loodud **serdid** kategooria. Klõpsake nuppu **ekspordi**, Pange failile nimi, valige **.p12** vorming ja seejärel klõpsake nuppu **Salvesta**.

    Pidage meeles, et faili nimi ja asukoht eksporditud .p12 tõuketeatised sert. Seda kasutatakse autentimine APNs-i lubamiseks laadides Azure'i klassikaline portaalis.



####<a name="create-a-provisioning-profile-for-the-app"></a>Rakenduse ettevalmistamise profiili loomine

1. Tagasi <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS-i Provisioning portaalis</a>valige **Ettevalmistamise profiilid**, valige **Kõik**ja klõpsake soovitud **+** nuppu Loo uus profiil. See käivitab **Provisiong profiili lisamine iOS-i** viisard

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Valige **iOS-i rakenduste arendamiseni** **väljatöötamisel** tüübiks provisiong profiil ja klõpsake nuppu **Jätka**.


3. Seejärel valige loendist **Rakendus ID** drop-down äsja loodud rakenduse ID-d ja siis nuppu **Jätka**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. **Valige serdid** Kuva, valige oma arengu sertifikaadi kood teenusesse ja klõpsake nuppu **Jätka**. See on Allkirjaserdi pole teie loodud tõuketeatised sert.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Järgmiseks valige **seadmete** testimiseks kasutada, ja klõpsake käsku **Jätka**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Lõpetuseks, valige soovitud profiili nimi **Profiili nimi**, klõpsake nuppu **Genereeri**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
