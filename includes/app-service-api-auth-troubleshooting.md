Enamik aega autentimise vigade põhjuseks olla valed või vastuolulised kogemused konfiguratsioonisätted. Siin on mõned asjad, mida kontrollida teatud soovitused.

* Veenduge, et ei jääks märkamata **salvestamine** nuppu suvalist kohta. See on sageli lihtne teha ja tulemus on, siis saate vaadata õigete väärtuste portaali lehel, kuid need pole salvestatud tegelikult Azure keskkonnas või Azure AD rakenduses.
* Sätete konfigureeritud **Rakenduse sätted** tera Azure portaali, veenduge, et õige API rakenduse või web app on valitud sätete sisestamisel.  Samuti veenduge, et **rakenduse sätete** ja pole **ühendusstringi**, sisestamise sätteid nagu kaks jaotist vorming on sarnane.
* JavaScripti vastendamisel autentimine, laadige alla manifest uuesti veendumaks, et `oauth2AllowImplicitFlow` on muudetud `true`.
* Veenduge, et HTTPS kasutada kõikjal, kus olete konfigureerinud URL-id.

    * Projekti kood
    * Klõpsake CORS
    * Azure'i keskkonnas rakenduse sätteid iga API app ja web app
    * Azure AD Rakenduse sätted.
    
    Pange tähele, et kui kopeerite portaalist API rakenduse URL-i, see on sageli `http://` ja teil on käsitsi muuta seda, et `https://`.

* Veenduge, et kood on edukalt juurutatud. Näiteks mitme projektiga lahendus on võimalik muuta projekti kood ja valige üks teisi kogemata kui kavatsete juurutada muutmine.
* Veenduge, et te ei kavatse HTTPS URL brauseri HTTP URL-id. Visual Studio loob vaikimisi avaldamine profiile HTTP URL-id, ja see on, mis avab brauseris, kui juurutate projekti.
* JavaScripti vastendamisel autentimine, veenduge, et CORS õigesti konfigureeritud API rakendust, mis nõuab JavaScripti koodi. Kahtluse kohta, kas tekkinud probleem on seotud CORS, proovige "*" lubatud origin URL. 
* JavaScripti vastendamisel, avage oma brauseri Arendaja tööriistad konsooli menüü tõrge lisateabe saamiseks ja uurida HTTP päringuid võrgus. Siiski võib konsooli tõrketeated eksitavat. Kui kuvatakse teade, mis näitab, CORS tõrke, võib tegelik küsimus autentimist. Saate kontrollida, kui see on nii rakendus töötab autentimise ajutiselt ajutiselt keelatud.
* .Net-i API rakenduse, veenduge, et saite nii palju teavet tõrketeadetes võimalikult seadmisega [customErrors režiimi olekusse väljas](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* .Net-i API rakenduse, [silumine Kaugseanss](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)käivitamine ja uurida muutujate edastatud koodi, mis kasutab ADAL omandada esitaja luba või kood, mis kontrollib vastu oodatud teenuse põhisumma ID väärtused Pange tähele, et teie koodi saate valida konfiguratsiooni väärtuste paljudest erinevatest allikatest, nii, et see on võimalik leida üllatusi sellisel viisil. Näiteks, kui te Näpu `ida:ClientId` nimega `ida:ClientID` Azure'i rakendust Service keskkonna sätteid konfigureerimisel koodi võite saada soovitud `ida:ClientId` väärtus, mis see otsib fail, ignoreerides Azure'i rakenduse teenuse säte. 
* Kui asjad ei tööta tavaline Internet Exploreri aknas, mõne olemasoleva sisse logida võivad takistada; Proovige InPrivate ja Chrome'i või Firefoxi.
