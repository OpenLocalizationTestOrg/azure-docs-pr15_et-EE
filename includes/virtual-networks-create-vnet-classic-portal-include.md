## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Kuidas luua mõne VNet Azure'i portaalis

Mõne VNet ülaltoodud stsenaariumi loomiseks järgige alltoodud juhiseid.

1. Brauserist, avage http://manage.windowsazure.com ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake nuppu **Uus** > **Võrguteenuste** > **VIRTUAALSE võrgu** > **Kohandatud loomine** , nagu on näidatud alloleval joonisel.

    ![VNet portaali loomine](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. **Virtuaalse võrgu üksikasjade** lehel, tippige soovitud VNet **nimi** , valige selle **asukoht**ja seejärel klõpsake järku 2 lehe parempoolses allnurgas noolt. Alloleval joonisel meie stsenaarium sätteid.

    ![Lehe virtuaalse võrgu üksikasjad](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Määrake lehel **DNS-serverid ja virtuaalse Privaatvõrgu ühendus** nime ja IP-aadress kuni 9 nimeservereid kasutada. Kui te ei määra DNS-i server, kasutatakse teie VNet sisemise nime eraldusvõime eraldusvõime esitatud Azure. Meie stsenaariumi me ei konfigureerida DNS-serverid.
5. Kui soovite pakkuda oma VNet punkti – saidile VPN juurdepääsu, lubage ruut **Konfigureeri punkti – saidile VPN** . Kui konfigureerite punkti saidi VPN, te saate lisada selle oma VNet igal ajal pärast loomist. Meie stsenaariumi me ei konfigureerida punkti saidi VPN.
6. Kui soovite esitada oma VNet ja muu VNet või kohapealse võrgu-saidilt VPN Ühenduvus, **VPN saitide konfigureerimine** ruut Luba ja määrake, kas soovite **ExpressRoute** või märkuse ja võrgu nimi ühenduse loomiseks kasutada. Kui konfigureerite VPN saidilt, te saate lisada selle oma VNet igal ajal pärast loomist. Meie stsenaariumi me ei konfigureerida VPN saidilt.
7. Klõpsake liikumiseks samm 3. alloleval joonisel meie stsenaarium sätete lehe parempoolses allnurgas noolt.

    ![Lehe DNS-serverid ja VPN Ühenduvus](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Lehel **Virtuaalse võrgu aadress tühikute** **Alates IP**, klõpsake jaotises *10.0.0.0* VNet aadressiruumi muutmiseks klõpsake nuppu ja tippige käivitamine aadressiruumi, mida soovite kasutada. Sisestage väljale *192.168.0.0*meie stsenaariumi. 
9. Valige jaotises **CIDR (aadressi arv)** määratud arvu bittide jaoks alamvõrgu mask. Meie stsenaariumi puhul valige *16 (65536)*.
10. Klõpsake jaotises **ALAMVÕRKU**, klõpsake *alamvõrgu-1* ja ümber alamvõrgu vajadusel. Meie stsenaariumi nimetage see ümber *FrontEnd*.

    >[AZURE.NOTE] Kui klõpsate väljaspool nimi rühmitusaluse jaoks soovitud alamvõrgu te ei saa nime muuta, kui alamvõrgu uuesti. Määrata, et peate eemaldama alamvõrgu, klõpsates nuppu X, paremale, lisage uus alamvõrgu etapis 13 allpool kirjeldatud.

11. Määrake jaotises **Alates IP** alamvõrgu esimese, alustades alamvõrgu IP-aadress. Sisestage väljale *192.168.1.0*meie stsenaariumi.
12. Valige jaotises **CIDR (aadressi arv)** määratud arvu bittide esimese alamvõrgu alamvõrgu maski jaoks. Meie stsenaariumi puhul valige *24 (256)*.
13. Klõpsake vajadusel **lisada alamvõrgu** lisada uue alamvõrku. Meie stsenaariumi lisamine on alamvõrgu ja korrake juhiseid 10 – 12 konfigureerida VNet, nagu on näidatud alloleval joonisel.

    ![Lehel virtuaalse võrgu aadressi tühikud](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klõpsake lehe loomine on VNet parempoolses allnurgas nuppu märge. Mõne sekundi pärast oma VNet kuvatakse loendis Saadaolevad VNets, nagu on näidatud alloleval joonisel.

    ![Uue virtuaalse võrgu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)