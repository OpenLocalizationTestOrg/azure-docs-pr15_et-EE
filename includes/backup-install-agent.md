## <a name="download-install-and-register-the-azure-backup-agent"></a>Allalaadimine, installimine ja Azure varukoopia agent registreerimine

Pärast Azure varukoopia vault loomist peaks olema installitud agenti iga teie Windowsi masinad (Windows Server, Windows klient, süsteemi andmete kaitse Manager server või Azure varukoopia Serveri), mis võimaldab Varundage andmed ja rakenduste Azure.

1. Logige sisse [haldusportaal](https://manage.windowsazure.com/)

2. Klõpsake **Taastamise teenused**ja valige varukoopiate hoidla, soovitud serveri registreerida. Kuvatakse selle varukoopiate hoidla Kiirkäivituse leht.

    ![Lühijuhend](./media/backup-install-agent/quickstart.png)

3. Klõpsake lehel kiirjuhendi **Windows Server või süsteemi andmed kaitse Manager või Windowsi kliendi** jaotises suvandit **Agent alla laadida**. Klõpsake nuppu **Salvesta** kopeerimiseks kohalikus arvutis.

    ![Salvestage agent](./media/backup-install-agent/agent.png)

4. Kui agent on installitud, topeltklõpsake MARSAgentInstaller.exe käivitada installi agent Azure varukoopia. Valige Installikaust ja nullist kausta agent nõutav. Määratud vahemälu asukoht peab olema vaba ruumi, mis on vähemalt 5% andmete varukoopia.

5.  Kui kasutate puhverserverit, Interneti-ühenduse **Puhverserveri konfiguratsiooni** ekraani, Sisestage puhverserveri server üksikasjad. Kui te ei kasuta autenditud puhverserverit, sisestage kasutaja nimi ja parool üksikasjad Kuva.

6.  Azure'i varundus agent installib Windows PowerShelli .NET Framework 4.5 ja (kui see pole juba saadaval) installimise lõpuleviimiseks.

7.  Kui agent on installitud, nuppu **Jätka registreerimise** jätkamiseks töövoog.

    ![Registreeru](./media/backup-install-agent/register.png)

8. Kuva vault identimisteabe otsige sirvides üles ja valige vault identimisteabe faili, mille eelnevalt alla laadida.

    ![Hoidla mandaat](./media/backup-install-agent/vc.png)

    Vault identimisteabe faili kehtib ainult 48 tundi (kui see on alla laaditud portaali). Kui mis tahes tõrke see ekraani (nt "Vault identimisteabe faili ette aegus"), sisselogimine Azure portaali sisse ja vault identimisteabe faili uuesti alla laadida.

    Veenduge, et vault identimisteabe fail on saadaval kohas, kuhu pääseb installi rakenduse. Kui teil tekib juurdepääs seotud tõrgete, kopeerige vault identimisteabe fail ajutist asukohta selle arvuti ja proovige uuesti.

    Kui tõrke sobimatu vault mandaadi (nt "lubamatud hoidla mandaat esitatud") fail on kas rikutud või ei ole on uusim identimisteabe teenusega seotud taastamine. Pärast allalaadimist vault mandaati uue faili portaali toimingut korrata. See tõrge on tavaliselt näha, kui kasutaja klõpsab Azure'i portaalis kiiresti järgnevaid suvandi **allalaadimine vault mandaati** . Sel juhul kehtib ainult teise vault mandaati faili.

9. **Krüptimise säte** ekraanil, saate luua parooli või sisestage parool (vähemalt 16 märki). Ärge unustage Salvesta parool turvalises asukohas.

    ![Krüptimine](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Kui parool on kaotanud või unustanud; Microsoft ei saa aidata taastada varundatud andmed. Lõppkasutaja kuulub krüptimine parooli ja Microsoft ei ole nähtavus kasutatavaid lõppkasutaja parool. Salvestage fail turvalises asukohas, et see on nõutav taastamine töötamise ajal.

10. Kui klõpsate nuppu **valmis** , seade on edukalt registreeritud vault ja nüüd olete valmis alustama Microsoft Azure'i varukoopiat.

11. Microsoft Azure varukoopia autonoomse kasutamisel saate muuta, klõpsates nuppu **Muuda atribuute** valik Azure varukoopia mmc Snap sisse registreerimise töövoo käigus määratud sätted.

    ![Atribuutide muutmine](./media/backup-install-agent/change.png)

    Teise võimalusena andmete kaitse Manager kasutamisel saate muuta, klõpsates nuppu **Konfigureeri** suvandi, valides **Online'i** vahekaardil **haldamine** registreerimise töövoo käigus määratud sätted.

    ![Azure'i varukoopiad konfigureerimine](./media/backup-install-agent/configure.png)
