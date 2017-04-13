## <a name="create-an-iot-hub"></a>Soovitud asjade keskuse loomine

Soovitud asjade jaoturi jäljendatud seadme ühenduse luua. Järgmised sammud näitavad, kuidas selle ülesande Azure portaali kaudu.

1. [Azure'i portaali]sisselogimine[lnk-portal].

2. Jumpbar, klõpsake nuppu **Uus** > **Asjade Interneti** > **Azure asjade jaoturi**.

    ![Azure'i portaalis Jumpbar][1]

3. **Asjade keskuses** tera, valige oma asjade keskuses konfigureerimine.

    ![Asjade keskuses blade][2]

    * Sisestage **nimi** väljale nimi oma asjade keskuses. Kui **nimi** on kehtivate ja saadaval, kuvatakse roheline märge väljale **nimi** .
    * Valige soovitud [hinnad ja skaala taseme][lnk-pricing]. Selles õpetuses ei nõua teatud taseme. Selles õpetuses kasutada tasuta F1 taseme.
    * **Ressursirühm**luua uue ressursirühma või valige olemasoleva. Lisateavet leiate teemast [kaudu ressursside rühmade hallata oma Azure ressursse][lnk-resource-groups].
    * Valige **asukoht**, majutada oma asjade jaoturi asukoht. Selles õpetuses valida lähima asukoha.

4. Kui olete valinud oma asjade keskuses konfiguratsiooni suvandid, klõpsake nuppu **Loo**.  Võib kuluda mõni minut Azure oma asjade keskuse loomiseks. Oleku, saate jälgida edenemist soovitud Startboard või paanil teatised.

    ![Uus asjade jaoturi olek][3]

5. Kui asjade jaoturi on loodud, klõpsake nuppu Uus paani oma asjade jaoturi avada uue asjade jaoturi höövlitera Azure'i portaalis. **Hostname (hostinimi)**üles ja klõpsake **ühiskasutuses juurdepääsupoliitikaid**.

    ![Uue asjade jaoturi blade][4]

6. **Ühiskasutuses juurdepääsupoliitikaid** tera, nuppu **iothubowner** poliitika, ning kopeerige ja märkige üles ühendusstringi **iothubowner** tera. Lisateavet leiate teemast [juurdepääsu reguleerimine] [ lnk-access-control] "Azure asjade jaoturi arendaja juhend".

    ![Ühiskasutusega juurdepääsu poliitikate blade][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
