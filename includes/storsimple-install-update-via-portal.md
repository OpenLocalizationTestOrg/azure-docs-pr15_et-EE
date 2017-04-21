<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Update 1.2 installida Azure klassikaline portaalis

1. Lehel StorSimple, valige oma seade. Liikuge **seadmete** > **hooldustööd**.

2. Klõpsake lehe allosas nuppu **Skannida värskendused**. Töö luuakse saadaval värskendusi. Kui töö on lõpule viidud, teavitatakse teid sellest.

3. Sama lehe jaotises **Tarkvaravärskendusi** näete, et uus tarkvara värskendused on saadaval. Soovitame teil üle vaadata selle väljalaskemärkmed enne Update 1.2 oma seadmes.

    ![Tarkvara värskenduste installimine](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Klõpsake lehe allosas nuppu **Installi värskendused**.

5. Teil palutakse kinnitust. Klõpsake nuppu **OK**.

6. Kuvatakse dialoogiboks **Värskenduste installimine** esitatakse. Seadme peaksid vastama, mis on loetletud selles dialoogiboksis. Need juhised olid täidetud enne Värskenda. Valige **olen valmis uuendada oma seadme ja mõistmiseks ülaltoodud nõue**. Klõpsake ikooni Otsi.

    ![Kinnitusteate](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Nüüd hakkavad kogumi eelse automaatne kontrollimine. Järgmised:

    - **Selle domeenikontrolleri seisund kontrollib** veenduge, et seadme kontrollerid on terve ja võrgus.
    
    - **Riistvara komponent tervise kontrollib** veenduge, et kõik riistvara StorSimple seadmes on terve.
    
    - **Andmete 0 kontrollib** veenduge, et teie seadmes on lubatud andmete 0. Kui see liides pole lubatud, pead selle taas lubada ja proovige siis uuesti.
    
    - **Andmete 2 ja 3 andmete kontrollib** kinnitamaks, et andmete 2 ja 3 andmete võrgu liidesed pole lubatud. Kui liideste on lubatud, siis te peate neid keelata, ja siis proovige uuendada oma seadme. Selle kontrolli toimub ainult siis, kui värskendate GA tarkvara seadmest. Selle kontrolli seadmetes, kus töötab versioonide arv 0,1, 0,2 või 0,3 ei vaja.
    
    - **Lüüsi, märkige ruut** mis tahes seadmes versiooni enne Update 1. Selle kontrolli sooritatakse kõik seadmes eelse update 1 tarkvara, kuid seadmetes, mis on konfigureeritud võrgu liidese peale andmete 0 lüüsi nurjub.
 
    Update 1.2 rakendatakse ainult siis, kui ülaltoodud eelse update kontrollid on edukalt lõpule viidud. Kuvab teate, et eelse update kontrollid on pooleli.
  
    ![Kontrollige eelnevalt teatis](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Järgmises näites, kus versiooniuuenduse-eelne kontroll nurjus. Peate veenduge, et seadme kontrollerid on terve ja võrgus. Peate ka seisundi kontrollimiseks riistvara. Selles näites kontrolleril 0 ja 1 kontrolleril komponentide vaja tähelepanu. Kui peate Microsoft Support ühendust võtta, kui te ei saa ise nende probleemide lahendamiseks.

     ![Enne sisse nurjus.](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Kui olete rakendanud Update 1.2 StorSimple seadme andmete 2 ja 3 andmete kontrollib ja lüüsi sisse ei saa enam vaja tulevikus värskendusi. Enne kontrolli siiski vajalik.


8. Pärast versiooniuuenduse-eelne kontrollid on edukalt lõpule viidud, luuakse on töö. Kui värskenduse töö on loodud, teavitatakse teid sellest.
 
    ![Värskendage töö loomine](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Värskenduse rakendatakse siis oma seadmes.
 
9. Värskenduse töö jälgimiseks klõpsake nuppu **Kuva töö**. Klõpsake lehel **töö** näete update edenemist. 

    ![Töö edenemise värskendamine](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Värskenduse võtab mõne tunni pärast lõpuleviimiseks. Töö üksikasjad saate vaadata igal ajal.

    ![Projekti üksikasjade värskendamine](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Pärast töö on lõpule jõudnud, **hooldustööd** lehele liikumiseks ja liikuge kerides allapoole jaotiseni **Tarkvaravärskendusi**.

12. Veenduge, et teie seade töötab **StorSimple 8000 seeria Update 1.2 (6.3.9600.17584)**. **Viimati värskendatud kuupäeva** tuleks ka muuta.

    ![Lehe hooldustööd](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Nüüd näete, et hooldustööd režiimi värskendused on saadaval. Need värskendused on häiriva värskendused, mis seadme tööseisakute, saab rakendada ainult Windows PowerShelli liidese seadme kaudu. Järgige juhiseid [hoolduse režiimi värskenduste installimine](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) installida järgmised värskendused Windows PowerShelli kaudu StorSimple.

> [AZURE.NOTE] Teatud juhtudel võib teade hooldustööd režiimi värskendused on saadaval kuvada üles 24 tundi pärast hoolduse režiimi värskendusi rakendatakse edukalt seadmes.  


