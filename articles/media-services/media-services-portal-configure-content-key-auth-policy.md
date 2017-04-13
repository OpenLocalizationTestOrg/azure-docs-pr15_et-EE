<properties 
    pageTitle="Konfigureerimine sisu klahvi autoriseerimine poliitika Azure'i portaalis | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerida autoriseerimine põhimõtted sisu klahvi." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Sisu võtme autoriseerimine poliitika konfigureerimine
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Ülevaade

Microsoft Azure Media Servicesi võimaldab esitamisel täiustatud Standard (AES) (kasutades 128-bitine) või [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)MPEG-kriips, sujuva voogesituse ja HTTP Live Streaming (HLS) voogu. AMS võimaldab teil esitamisel MÕTTEKRIIPSU voogu Widevine DRM krüptitud. PlayReady nii Widevine krüptitud määratlus levinud krüptimise (ISO/IEC 23001-7 CENC) kohta.

Media Servicesi pakub **Võtit/litsentsi teenus** , kust saate hankida kliendid võtmed AES või PlayReady/Widevine litsentside krüptitud sisu.

Selles teemas kirjeldatakse, kuidas kasutada Azure portaali sisu võtme autoriseerimine poliitika konfigureerimiseks. Võti saab hiljem krüptimiseks dünaamiliselt sisu. Praegu saate krüptida järgmised streaming Märkus vormingud: HLS, MPEG MÕTTEKRIIPSU ja sujuva voogesituse. Te ei saa krüptida HDS streaming vorming või järkjärguline allalaadimine.

Kui mängija taotleb voo, mis on määratud dünaamiliselt krüptimist, kasutab Media Servicesi konfigureeritud võti dünaamiliselt krüptimiseks AES või DRM krüptimise abil sisu. Dekrüptida voo, mängija taotleb võti teenusest võtmed. Otsustada, kas kasutajal on õigus saada võti, hindab teenuse autoriseerimine poliitikate võti määratud.


Kui kavatsete on mitu sisu võtmed või peale Media Servicesi peamised teenuse **Klahv/litsentsi teenus** URL-i määramiseks kasutage Media Services .NET SDK või REST API-d.

[Sisu klahvi autoriseerimine poliitika abil Media Services .NET SDK konfigureerimine](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigureerimine sisu klahvi autoriseerimine poliitika Media Services REST API kasutamine](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Mõni järgmine kehtida.

- Saama kasutada dünaamilise pakendit ja dünaamiline krüptimist, peate kindlasti olema vähemalt üks streaming reserveeritud ühiku. Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).
- Teie vara peab sisaldama kohandatava bitrate MP4s või kohandatava bitrate sujuva voogesituse failide kogum. Lisateavet leiate teemast [Encode vara](media-services-encode-asset.md).
- Klahv teenus vahemälu ContentKeyAuthorizationPolicy ja sellega seotud objektid (poliitika suvandid ja piirangute) 15 minutit.  Kui loote mõnda ContentKeyAuthorizationPolicy ja määrata, et kasutada "Turbeloa" piirang, testige seda ja seejärel poliitika kasutusele "Ava" piirang, kulub umbes 15 minutit enne poliitika aktiveerib poliitika "Ava" versiooni.


##<a name="how-to-configure-the-key-authorization-policy"></a>Kuidas: võtme autoriseerimine poliitika konfigureerimine

Võtme autoriseerimine poliitika konfigureerimiseks **Sisu kaitse** lehe valimine

Media Services toetab kinnitamise kasutajad, kes võtme taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika võib olla **avamine**, **Turbeloa**või **IP** autoriseerimine piiranguteta (**IP** saab konfigureerida ülejäänud või .NET SDK).

###<a name="open-restriction"></a>Avatud piirang

**Avage** piirang tähendab, et süsteemi toob võti kõigile, kellel võtme taotleb. See piirang võib olla kasulik katsetamiseks.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Turbeloa piirang

Turbeloa piiratud poliitika valimiseks vajutage **TURBELOA** nupule.

**Turbeloa** piiratud poliitika peab kaasas märgiks välja **Secure Turbeloa Service** (STS). Media Services toetab sõned **Lihtne Web sõned** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) vorming ja **JSON Web Turbeloa** (JWT) vormingus. Lisateavet leiate teemast [JWT Turbeloa autentimist](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Servicesi ei paku **Secure Turbeloa teenused**. Saate luua kohandatud STS või kasutada Microsoft Azure ACS, et probleem sõned. Määratud võti ja probleemi nõuete Turbeloa piirang konfiguratsioonis määratud allkirjastatud märgiks loomiseks tuleb konfigureerida STS. Media Servicesi võtme teenus tagastab krüptovõtme kliendile kui luba on kehtiv ja luba nõuded kattuvad sisu võti konfigureeritud. Lisateavet leiate teemast [Kasutamine Azure ACS, et probleem sõned](http://mingfeiy.com/acs-with-key-services).

Kui konfigureerimine on **TURBELOA** piiratud poliitika, peate määrama väärtused **kontrollimise klahvi**, **väljaandja** ja **publiku**jaoks. Esmane kinnitamine võti sisaldab klahvi, mis on allkirjastatud luba, väljaandja on turvaline Turbeloa teenus, mis annab luba. Sihtrühma (mõnikord nimetatakse ulatus) kirjeldatakse soovidele vastavaks luba või ressursi luba lubab juurdepääsu. Media Servicesi võtme teenus kinnitab, et need väärtused luba vastavad väärtused mall.

###<a name="playready"></a>PlayReady

Kui teie sisu **PlayReady**kaitsmisele üks asju, mida on vaja määrata oma autoriseerimine poliitika on XML-string, mis määratleb PlayReady litsentsi mall. Poliitika on vaikimisi:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Saate **importida poliitika xml** nuppu ja pakkuda erinevate XML-i, mis vastab määratud XML-skeemi [siin](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

