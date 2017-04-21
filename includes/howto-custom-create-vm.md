#<a name="how-to-create-a-custom-virtual-machine"></a>Kuidas luua kohandatud virtuaalse masina

*Kohandatud* virtuaalse masina viitab virtuaalse masina loomist, kuna see võimaldab teil konfigureerida rohkem võimalusi, kui **Kiiresti luua** meetod **Galeriist** meetodi abil. Need suvandid on järgmised.

- Rohkem võimalusi pildi abil saate luua virtuaalse masina (VM)
- VM virtuaalse võrguga ühenduse loomine
- Olemasoleva pilvepõhise teenuse VM lisamine
- VM lisamine on kättesaadavus määramine

> [AZURE.IMPORTANT] Kui soovite kasutada virtuaalse võrgu nii, et saate luua ühenduse selle hostname või häälestamine asutusesiseses ühendused arvuti virtuaalne, veenduge, et teie määratud virtuaalse võrgu virtuaalse masina loomisel. Virtuaalse masina saab konfigureerida virtuaalse võrgu liituda ainult juhul, kui loote virtuaalse masina. Virtuaalne võrkude kohta leiate lisateavet teemast [Azure virtuaalse võrgu ülevaade](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. [Azure'i portaali](http://manage.windowsazure.com)sisse logida.

2. Klõpsake käsku **Uus**.

3. Klõpsake **arvutada**, klõpsake **virtuaalse masina**ja valige **Galeriist**.

4. Valige pilt, mida soovite kasutada, ja klõpsake noolenuppu, et jätkata.

5. Kui mitme versiooni pilt on saadaval, **Väljaandmisaeg versioon**, valige versioon, mida soovite kasutada.

6. Tippige nimi, mida soovite kasutada virtuaalse masina **Virtuaalse masina nimi**.

7. Valige sobiv suurus virtual masina **taseme** - ja **suurus** abil. Valige suurus mõjutab maksimaalne konfiguratsiooni virtuaalse masina, samuti on hinnad. Konfiguratsiooni leiate teemast [virtuaalse masina ja pilvepõhise teenuse suurused Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Tippige **Uue kasutaja nimi**, haldus konto, mida soovite kasutada hallata serveri nimi.

9. Tippige **Uus parool**haldus kontole keerukas parool. Sama parool uuesti väljale **Parooli kinnitus**.

10. Klõpsake noolt jätkata.

11. **Pilveteenuses**, tehke ühte järgmistest.

    - Kui see on esimene või ainult virtuaalse masina pilveteenusesse, valige **Loo uus pilveteenuses**. **Pilveteenuse teenuse DNS-i nimi**, tippige nimi, mis kasutab 3 24 väiketähti ja numbreid vahel. See nimi muutub URI, võtke ühendust virtuaalse masina kaudu pilveteenusesse kasutatava osa.
    - Kui see virtuaalse masina lisatakse pilveteenusesse, valige see loendist.

    > [AZURE.NOTE] Pannes virtuaalmasinates sama pilveteenuses kohta lisateabe saamiseks vaadake, [Kuidas ühendada virtuaalmasinates mõnes pilveteenuses](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. Valige **Piirkond/osaleja rühma/Virtual Network**, piirkond, osaleja rühma või virtuaalse võrgu, mida soovite kasutada virtuaalse masina. Osaleja rühmade kohta leiate lisateavet teemast [kohta osaleja rühmade virtuaalse võrgu jaoks](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. **Salvestusruumi konto**, valige olemasoleva salvestusruumi konto VHD faili või kasutage automaatselt loodud salvestusruumi kontot. Ainult üks salvestusruumi konto müüginäitajaid piirkonna kohta luuakse automaatselt. Kõik muud virtuaalmasinates see säte on loodud asuvad selle konto salvestusruumi. Teil on piiratud 20 salvestusruumi kontod.

14. Kui soovite virtual seade kuulub kättesaadavus kogum, **Kättesaadavus**, valige **Loo kättesaadavus** või lisada mõne olemasoleva kättesaadavus määramine.

    **Märkus**: virtuaalmasinates mõne kättesaadavus kogumis on juurutatud erinevate viga domeenid. Mitme virtuaalmasinates pannes saadavus määramine aitab tagada, et teie taotlus on saadaval ajal võrgu tõrkeid, kohalikule kettale riistvara tõrkeid ja mis tahes kavandatud tööseisakute.

15.  **Lõpp-punktid**, klõpsake jaotises ülevaatamine uue lõpp-punktid lubama ühendusi virtuaalse masina, näiteks kaudu kaugtöölaua või mõnda muud klienti Secure Shell (SSH) loodud. Samuti saate lisada lõpp-punktid kohe või hiljem luua. Juhised nende hiljem loomise kohta leiate teemast [lõpp üles virtuaalse masina](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Jaotises **VM Agent**, otsustada, kas installida VM Agent. See agent pakub keskkonna jaoks saate installida laiendid, mis aitavad teil suhelda virtuaalse masina. Lisateavet leiate teemast [Haldamine laiendid](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Klõpsake noolt virtuaalse masina loomiseks.

    ![Kohandatud virtuaalse masina loomine õnnestus](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Järgmised sammud##
Pärast loomist virtuaalse masina see käivitatakse automaatselt. Kui portaalis kuvatakse olek käivitamine, te saate sisse logida virtuaalse masina. Lisateabe saamiseks vaadake ühte järgmistest artiklitest:

- [Kuidas töötab Linux sisse logida](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Kuidas töötab Windows Server sisse logida](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

