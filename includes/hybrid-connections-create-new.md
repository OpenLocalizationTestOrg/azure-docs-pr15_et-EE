
1. [Azure'i haldusportaal](http://manager.windowsazure.com) (see on vana portaali) logige arvutisse kohapealse.

2. Navigeerimispaani allosas valige **+ Uus** > **Rakenduse teenuste** > **BizTalki teenuse** > **Kohandatud loomine**.

3. Valige **väljaande** **BizTalki teenuse nimi** . 

    Selle õpetuse kasutab **mobile1**. Peate esitama uue BizTalki teenust kordumatu nimi.

4. Kui BizTalki teenus on loodud, valige menüü **Hübriid ühendused** ja seejärel käsku **Lisa**.

    ![Lisage ühenduse hübriid](./media/hybrid-connections-create-new/3.png)

    See loob uue ühenduse hübriid.

5. Sisestage **nimi** ja **Hostinimi** hübriid-ühenduse ja **pordi** määramine `1433`. 
  
    ![Hübriidjuurutuse ühenduse konfigureerimine](./media/hybrid-connections-create-new/4.png)

    Hosti nimi on kohapealse serveri nimi. See konfigureerib juurdepääsu SQL Server töötab port 1433 ühenduse hübriid. Kui kasutate nimega SQL serveri eksemplar, selle asemel kasutada staatilised pordid varem määratletud.

6. Uus ühendus on loodud, olek on uus ühendus, kuvatakse **kohapealse häälestamise lõpetamata**.

7. Tagasi liikuda oma mobiilsideteenuse, nuppu **Konfigureeri**, liikuge kerides allapoole jaotiseni **hübriid ühendused** ja klõpsake käsku **Lisa hübriid ühendust**, siis valige äsja loodud ühenduse hübriid ja nuppu **OK**.

    See võimaldab kasutada uut hübriid ühendust oma mobiilsideteenuse.

Järgmiseks peate ühenduse hübriid ülemus kohapealse arvutisse installida.