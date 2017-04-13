<properties
   pageTitle="Avaldamise pilveteenus Azure tööriistade abil | Microsoft Azure'i"
   description="Lugege selle kohta, kuidas avaldada Azure pilveteenuse teenuse projektide Visual Studio abil."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Avaldamise pilveteenus Azure tööriistade abil

Microsoft Visual Studio Azure'i tööriistade abil saate avaldada otse Visual Studio Azure rakenduse. Visual Studio toetab integreeritud avaldamise lavastus või pilveteenus tootmiskeskkonda.

Azure'i rakenduse avaldamiseks peab teil olema Azure tellimuse. Pilvepõhise teenuse ja salvestusruumi konto peab häälestada ka rakenduse kasutamiseks. Saate häälestada need [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Kui avaldate, saate valida oma pilveteenuses juurutamine keskkonnas. Peate valima ka salvestusruumi konto, mis kasutab rakendusepaketi juurutamiseks salvestamiseks. Pärast juurutuse eemaldatakse rakenduse pakett salvestusruumi konto.

Kui arendamine ja testimine Azure rakenduse saate Web juurutada avaldada muudatused sammhaaval oma web rollid. Pärast avaldamist rakenduse juurutamine keskkonnas Web juurutamine võimaldab teil muudatuste juurutamine otse virtual arvutisse, kus töötab web roll. Teil pole paketti ja kogu Azure rakenduse iga kord, kui soovite värskendada oma web rolli katsetada muudatused avaldada. Seda moodust võib olla saadaval web rolli muudatuste testimiseks on rakenduse juurutamine keskkonnas avaldatud ootamata pilveteenuses.

Azure'i rakenduse avaldama ja värskendada web rolli, kasutades Web juurutada, tehke järgmist:

- Avaldamine või paketti Azure rakenduse Visual Studio

- Värskendage web rolli osana arendamise ja testimise tsükkeldiagramm

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Avaldamine või paketti Azure rakenduse Visual Studio

Kui avaldate Azure rakenduse, saate teha järgmisi toiminguid.

- Teenuse paketi loomine: saate selle paketi ja teenuse konfiguratsioonifail avaldada oma rakenduse juurutamine keskkonnas [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885).

- Visual Studio Azure'i projekti avaldamiseks: avaldada oma rakenduse otse Azure, kasutate avaldada viisard. Lisateavet leiate teemast [Azure rakendus viisard avaldamine](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Visual Studio teenuse paketi loomiseks

1. Kui olete rakenduse avaldada, avage Solution Exploreris, Azure'i projekti, mis sisaldab teie rollid kiirmenüü avamine ja valige käsk Avalda.

1. Teenuste paketti ainult loomiseks tehke järgmist.  

  1. Valige otseteemenüü Azure'i projekti **paketi**.

  1. Dialoogiboksis **Paketi Azure taotluse** valige teenuse konfiguratsioon, mille jaoks soovite luua paketi ja valige Koosta konfigureerimine.

  1. (valikuline) Sisselülitamiseks kaugtöölaua pilveteenusesse pärast selle avaldamist, märkige ruut **Luba kaugtöölaua kõigi rollide** ja valige **sätete** konfigureerimiseks Kaugtöölaud. Kui soovite oma pilveteenuses silumine pärast avaldamist, lülitage Kaug silumine, valides **Kaug lubamiseks kõikide rollide jaoks**.

      Lisateabe saamiseks vt [Kaugtöölaua abil Azure'i rollid](vs-azure-tools-remote-desktop-roles.md).

  1. Paketi loomiseks valige **paketi** link.

      File Explorer kuvatakse äsja loodud paketi faili asukoht. Saate kopeerida selle asukoha nii, et saate seda kasutada [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Avaldada selle paketi juurutamise keskkonnas, kasutage selle asukoha asukohaks paketi loomisel pilveteenus ja selle paketi juurutada [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885)keskkonnas.

1. (Valikuline) Juurutamise protsessi tegevuste Logi rea üksuse kiirmenüü tühistamiseks valige **tühistamine ja eemaldada**. See peatab juurutamise töö ja kustutab Azure juurutamine keskkonnas.

    >[AZURE.NOTE] Selles juurutamise keskkonnas kui see on juurutatud eemaldamiseks peate kasutama [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Valikuline) Pärast algama oma rolli aknad, kuvatakse Visual Studio juurutamine keskkonnas **Pilveteenustega** sõlm Server Explorer automaatselt. Siin saate vaadata üksikuid rolli aknad olekut. Leiate [Azure'i haldamise ressurssidest Cloud Exploreriga](vs-azure-tools-resources-managing-with-cloud-explorer.md). Järgmisel joonisel on kujutatud rolli aknad, kui need on endiselt Initializing riigis:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Värskendage Web rolli osana arendamise ja testimise tsükkeldiagramm

Kui teie rakendus kirjutamata taristu säilib, kuid web rollid vaja sagedasemad värskendamine, saate Web juurutamine värskendada ainult web roll projektis. See on mugav juhul, kui te ei soovi taastada ja ümberkorraldamine kirjutamata töötaja rollid, või kui teil on mitu web rollid ja soovite värskendada ainult ühte web rollid.

### <a name="requirements"></a>Nõuded

Siin on Web juurutada saate värskendada oma web rolli nõuetele.

- **Katsetamine eesmärgil ainult:** Muudatused on tehtud otse virtuaalse masina kus töötab web roll. Kui see virtuaalse masina taaskasutada, muudatused lähevad kaotsi, kuna algse paketi, mis on avaldatud kasutatakse uuesti luua virtuaalse masina rolli. Rakenduse web roll viimaste muudatuste saamiseks tuleb uuesti avaldada.

- **Saab värskendada ainult web rollid:** Töötaja rolli ei saa värskendada. Lisaks ei saa värskendada RoleEntryPoint web role.cs sisse.

- **Toetab ainult ühe eksemplari web rolli:** Mis tahes web rolli mitu eksemplari ei saa teie juurutamise keskkonnas. Mitme web rollide iga ainult üks eksemplar on toetatud.

- **Tuleb lubada Kaugtöölaua ühendused:** See on nõutav, et Web juurutada kasutada kasutaja ja parooli ühenduse, virtuaalse masina juurutada muudatused serveris, kus töötab Internet Information Services (IIS). Lisaks peate ühenduse, virtuaalse masina virtual selles arvutis IIS-i usaldusväärsete serdi lisamiseks. (See tagab, et IIS Web juurutada kasutatavat kaugühenduse on turvaline.)

Järgnev toiming eeldab, et kasutate **Azure teenuserakenduse avaldamine** viisardit.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Lubamiseks Web juurutada rakenduse avaldamisel

1. Funktsiooni **Lubamiseks Web juurutada** kõikide rollide ruut lubamiseks tuleb esmalt konfigureerida Kaugtöölaua ühendused. Suvand **Luba kaugtöölaud** kõigi rollide ja seejärel lisage identimisteavet, mida kasutatakse ühenduse kaugühenduse teel **Remote Desktop konfiguratsiooni** kuvatavas dialoogiboksis. Lisateavet leiate [Azure'i rollid Kaugtöölaud abil](vs-azure-tools-remote-desktop-roles.md) .

1. Web rollid rakenduse lubamiseks Web juurutada, valige **Luba Web juurutada kõik web rollid**.

    Kollane kolmnurk hoiatus kuvatakse. Web Deploy kasutab vaikimisi, mis ei ole soovitatav tundliku loomuga andmete üleslaadimine on ebausaldusväärsete, iseallkirjastatud sert. Kui teil on vaja selle protsessi tundliku loomuga andmete turvaline, saate lisada SSL-serdi Web juurutada ühenduste jaoks kasutada. Selle serdi peab olema usaldusväärne sert. Lisateavet selle kohta, kuidas seda teha, jaotisest **Teha Web juurutada tagada** käesoleva teema.

1. Valige **Järgmine** **Kokkuvõte** ekraani kuvamiseks ja seejärel valige **Avalda** juurutamiseks pilveteenusesse.

    Pilveteenusesse on avaldatud. Virtuaalse masina, mis on loodud on lubatud nii, et Web juurutada saab värskendada oma web rollid, ilma neid uuestipublitseerimist IIS-i allalaadimise.

    >[AZURE.NOTE] Kui teil on rohkem kui ühe eksemplari web rolli konfigureeritud, kuvatakse hoiatusteade, selle kohta, et iga web rolli piiratakse ühe eksemplari ainult pakendis, mis on loodud rakenduse avaldada. Valige jätkamiseks **OK** . Nagu jaotises nõuetele, saate määrata rohkem kui üks web roll, kuid ainult üks eksemplar iga rolli.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Värskendada oma Web rolli, kasutades Web juurutamine

1. Web juurutada kasutamiseks koodi muudatusi teha projekti ühte oma web rollid, Visual Studios, mille soovite avaldada, ja seejärel paremklõpsake teie lahendus selle projekti sõlm ja valige käsk **Avalda**. Kuvatakse dialoogiboks **Veebis avaldamine** .

1. (Valikuline) Kui lisasite usaldusväärse SSL-serdi allalaadimise IIS-i jaoks kasutada, kustutada ruut **Luba ebausaldusväärsete sert** . Lisateavet selle kohta, kuidas lisada serdi Web juurutada turvalisemaks muuta jaotisest **Abil teha Web juurutada Secure** käesoleva teema.

1. Web juurutada kasutamiseks peab avalda süsteem kasutajanimi ja parool, mida saate seadistada oma kaugtöölaua ühendus klõpsamisel esmalt avaldatud paketi.

  1. Sisestage väljale **kasutajanimi**kasutajanimi.

  1. Sisestage väljale **parool**parool.

  1. (Valikuline) Kui soovite seda profiili Salvesta see parool, valige **Salvesta parool**.

1. Oma web rolli muudatused avaldada, valige **Avalda**.

    Olekuribal kuvatakse **Avalda alustamine**. Kui avaldamine on lõpule jõudnud, **Avalda õnnestus** kuvatakse. Muudatused on nüüd kasutusele võetud web roll virtual teie arvutis. Nüüd saate alustada Azure rakenduse Azure keskkonnas muudatuste kontrollimiseks.

### <a name="to-make-web-deploy-secure"></a>Teha turvaline Web juurutamine

1. Web Deploy kasutab vaikimisi, mis ei ole soovitatav tundliku loomuga andmete üleslaadimine on ebausaldusväärsete, iseallkirjastatud sert. Kui teil on vaja selle protsessi tundliku loomuga andmete turvaline, saate lisada SSL-serdi Web juurutada ühenduste jaoks kasutada. Selle serdi peab olema usaldusväärne sert, mida te saada sertimiskeskus (CA).

    Oleks Web juurutada turvalist iga virtuaalse masina iga teie web rollid, tuleb üles laadida usaldusväärne sert, mida soovite kasutada web juurutada [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885). See tagab, et virtuaalse masina, mis on loodud web rolli rakenduse avaldamisel lisatakse sert.

1. IIS-i jaoks allalaadimise usaldusväärse SSL-serdi lisamiseks tehke järgmist.

  1. Ühenduse, virtuaalse masina, mis töötab web roll, valige web roll **Cloud Explorer** või **Server Explorer**ja valige siis käsk **Loo ühendus kaugtöölaua** . Üksikasjalikud juhised selle kohta, kuidas ühenduse, virtuaalse masina leiate [Azure'i rollid Kaugtöölaud abil](vs-azure-tools-remote-desktop-roles.md).

      Brauseri palub teil alla laadida ka. RDP-faili.

  1. SSL-serdi lisamine, avage halduse teenuse IIS-i. IIS-i lubamine SSL-i, avades **sidumiste** lingi **toimingu** paanil. Kuvatakse dialoogiboks **Lisada saidi siduv** . Valige **Lisa**ja seejärel valige rippmenüüst **Tüüp** HTTPS. Valige loendis **SSL-serdi** SSL-sert, et oli CA allkirjastanud ja üles [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885). Lisateavet leiate teemast [Halduse teenuse konfigureerimine ühenduse sätted](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Kui lisate usaldusväärse SSL-sert, kuvatakse hoiatus kollane kolmnurk enam **Avaldada viisard**.

## <a name="include-files-in-the-service-package"></a>Failide kaasamine teenuse paketi

Võib juhtuda, et need oleksid saadaval virtual arvutisse, mis on loodud rolli teie pakett kindlat tüüpi failidega kaasamiseks. Näiteks võite ka .exe või msi-faili, mida kasutatakse teie teenuse paketi käivitamine skripti lisada. Või web rolli või töötaja rolli projekti jaoks on vaja komplekti lisama. Need tuleb lisada lahendus Azure rakenduse faile lisada.

### <a name="to-include-files-in-the-service-package"></a>Teenuse paketi failide kaasamiseks

1. Saate lisada teenuse paketi kogum, siis tehke järgmist:

  1. Avage **Solution** Exploreris projekti sõlm projekti, millel pole soovitatud paketti.

  1. Selle komplekti lisamiseks projekti **Viited** kausta kiirmenüü avamine ja valige **Lisada viide**. Kuvatakse dialoogiboks lisamine viide.

  1. Valige teatmematerjalid, mida soovite lisada, ja seejärel klõpsake nuppu **OK** .

      Viide on lisatud loendis kausta **Viited** .

  1. Kiirmenüü avamine komplekti, mille olete lisanud, ja valige **Atribuudid**. Kuvatakse aken **Atribuudid** .

      Kaasamiseks valige selle komplekti teenuse pakkimine, **kopeerige kohaliku loendi** **True**.

1. Avage **Solution** Exploreris projekti sõlm projekti, millel pole soovitatud paketti.

1. Selle komplekti lisamiseks projekti **Viited** kausta kiirmenüü avamine ja valige **Lisada viide**. Kuvatakse dialoogiboks **Lisamine viide** .

1. Valige teatmematerjalid, mida soovite lisada, ja seejärel klõpsake nuppu **OK** .

    Viide on lisatud loendis kausta **Viited** .

1. Kiirmenüü avamine komplekti, mille olete lisanud, ja valige **Atribuudid**. Kuvatakse aken Atribuudid.

1. Teenuse paketi, **Kohalik eksemplar** loendis selle komplekti kaasamiseks valige **tõene**.

1. Teenuse pakett, mis on lisatud web rolli projekti faile lisada faili kiirmenüü avamine ja siis nuppu **Atribuudid**. Aken **Atribuudid** nuppu **sisu** loendiboksist **Koostada toiming** .

1. Teenuse pakett, mis on lisatud töötaja rolli projekti faile lisada faili kiirmenüü avamine ja siis nuppu **Atribuudid**. Valige aken **Atribuudid** väljal **Kopeeri asukohta väljundkausta** **kopeerimine kui uuemat versiooni** .

## <a name="next-steps"></a>Järgmised sammud

Kohta leiate lisateavet avaldamise Azure Visual Studio leiate [Azure'i rakendus viisard avaldamine](vs-azure-tools-publish-azure-application-wizard.md).
