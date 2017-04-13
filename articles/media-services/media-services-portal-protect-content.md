<properties 
    pageTitle="Azure'i portaalis sisu kaitse poliitikate konfigureerimine | Microsoft Azure'i" 
    description="Selles artiklis näitab, kuidas kasutada Azure portaali sisu kaitse poliitikate konfigureerimine. See artikkel ka kujutatakse oma varasid dünaamiline krüptimine." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"    
    ms.author="juliako"/>

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Azure'i portaalis sisu kaitse poliitikate konfigureerimine

> [AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Ülevaade

Microsoft Azure Media (AMS) võimaldab teil kaitsta oma meediumi jätab arvuti kaudu säilitamiseks, töötlemiseks ja kohaletoimetamise aeg. Media Services võimaldab teil esitada sisu krüptitud dünaamiliselt ja täiustatud Standard (AES) (128-bitise krüptimise klahvide abil), kasutades PlayReady ja/või Widevine DRM ja Apple'i FairPlay levinud krüptimise (CENC). 

AMS pakub teenuseid DRM litsentsid kohaletoimetamiseks ja AES tühjendage volitatud klientidele. Azure'i portaalis võimaldab teil luua üks **klahv/litsentsi autoriseerimine poliitika** igat tüüpi encryptions.

Selles artiklis näitab, kuidas sisu kaitse poliitikate konfigureerimine Azure'i portaalis. See artikkel ka kujutatakse dünaamiline krüptimise rakendamiseks oma varasid.

> [AZURE.NOTE]  Kui kasutasite Azure klassikaline portaali loomine poliitika, ei pruugita poliitikaid [Azure portaali](https://portal.azure.com/). Kuid kõik vana poliitikat ikka olemas. Te saate läbi vaadata Azure Media Services .NET SDK või [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tööriista abil (poliitikaid kuvamiseks paremklõpsake vara-teave (F4) -> Kuva > klõpsake vahekaardil sisu klahvid -> klõpsake võti). 
> 
> Kui soovite oma varade uue poliitika abil krüptida, konfigureerida neid Azure'i portaalis, klõpsake nuppu Salvesta ja dünaamiline krüptimise uuesti. 

## <a name="start-configuring-content-protection"></a>Käivitage konfigureerimine sisu kaitse

Kasutada portaali konfigureerimine sisu kaitse, globaalne AMS kontole alustamiseks tehke järgmist.

1. Valige [Azure portaali](https://portal.azure.com/)konto Azure Media Servicesi.
2. Valige **sätted** > **sisu kaitse**.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Klahv/litsentsi autoriseerimine poliitika

AMS toetab kinnitamise kasutajad, kes klahv või -litsentsi taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika peab teie konfigureeritud ja klientrakendus klahv/litsentsi selleks tuleb delived kliendile täita. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: **avamine** või **Turbeloa** piirang.

Azure'i portaalis võimaldab teil luua üks **klahv/litsentsi autoriseerimine poliitika** igat tüüpi encryptions.

###<a name="open"></a>Avatud 

Avatud piirang tähendab, et süsteemi toob võti kõigile, kellel võtme taotleb. See piirang võib olla kasulik katsetamiseks. 

### <a name="token"></a>Turbeloa

Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned lihtne Web sõned (SWT) vorming ja JSON Web Turbeloa (JWT) vormingus. Media Servicesi ei paku Secure Turbeloa teenused. Saate luua kohandatud STS või kasutada Microsoft Azure ACS, et probleem sõned. Määratud võti ja probleemi nõuete Turbeloa piirang konfiguratsioonis määratud allkirjastatud märgiks loomiseks tuleb konfigureerida STS. Media Servicesi võtme teenus tagasi nõutud klahvi (või litsentside) kui luba on kehtiv ja taotluste Turbeloa kattuvad need konfigureeritud võti (või litsentside).

Kui poliitika konfigureerimise luba piiratud, peate määrama esmane kinnitamine klahvi, väljaandja ja sihtrühma parameetrid. Esmane kinnitamine võti sisaldab klahvi, mis on allkirjastatud luba, väljaandja on turvaline Turbeloa teenus, mis annab luba. Sihtrühma (mõnikord nimetatakse ulatus) kirjeldatakse soovidele vastavaks luba või ressursi luba lubab juurdepääsu. Media Servicesi võtme teenus kinnitab, et need väärtused luba vastavad väärtused mall.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady õiguste malli

Üksikasjalikku teavet PlayReady õiguste malli, teemast [Media Services PlayReady litsentsi Mall ülevaade](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Mitte püsivate

Kui konfigureerite litsentsi püsiv, seda ainult toimub mälu ajal mängija kasutab litsentsi.  

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Püsivate

Kui konfigureerite litsentsi püsivate, salvestatakse see kliendi püsiv hoidla.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine õiguste malli

Üksikasjalikku teavet Widevine õiguste malli, vt [Widevine litsentsi Mall ülevaade](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Tavaline

Kui valite **tavaline**, luuakse malli kõik vaikesätted väärtustega.

### <a name="advanced"></a>Täpsemad

Üksikasjaliku selgituse advance suvand Widevine konfiguratsiooni, leiate [selle](media-services-widevine-license-template-overview.md) teema.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay konfigureerimine

FairPlay krüptimise lubamiseks peate pakkuda rakenduse serti ja rakenduse salajane võti (Küsi) kaudu FairPlay konfigureerimist. Üksikasjalikku teavet FairPlay konfigureerimine ja nõuete kohta, lugege [artiklit](media-services-protect-hls-with-fairplay.md) .

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Teie vara dünaamiline krüptimise rakendamine

Dünaamiline krüptimise ära, peate tegema järgmist:

- Kodeerida lähtefaili failikogumi kohandatava bitrate MP4.
- Vähemalt üks nõudmisel streaming ühiku saamiseks streaming lõpp-punkti, millest soovite esitada oma sisu. Lisateavet leiate teemast [mastaapimiseks tellitav streaming reserveeritud üksuste](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Mida soovite krüptida vara valimine

Kõigi oma varasid kuvamiseks valige **sätted** > **varad**.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Krüpti parooliga AES või DRM

Kui vajutate **Krüpti** vara, on esitatud kaks võimalust: **AES** või **DRM**. 

#### <a name="aes"></a>AES

Eemalda võtme krüptimise lubatakse kõik streaming protokollid AES: sujuva voogesituse, HLS ja MPEG-KRIIPSJOONE.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Kui valite menüü DRM, on esitatud erinevate valikute sisu kaitse poliitika (mis peab olema konfigureeritud nüüd) + kogumi streaming protokollid.

- **PlayReady ja Widevine MPEG - kriips** - dünaamiliselt krüptida oma MPEG-KRIIPSJOONE voo PlayReady ja Widevine digitaalõiguste.
- **PlayReady ja Widevine MPEG-kriips + FairPlay koos HLS** - dünaamiliselt krüptida saate MPEG-kriips voo PlayReady ja Widevine digitaalõiguste. Teie HLS voogu koos FairPlay ka krüptida.
- **PlayReady ainult sujuva voogesituse, ja HLS MPEG-kriips** - dünaamiliselt krüptida sujuva voogesituse HLS, MPEG-kriips voogu PlayReady DRM.
- **Widevine ainult koos MPEG-kriips** - dünaamiliselt krüptida saate MPEG-kriips Widevine DRM.
- **Ainult koos HLS FairPlay** - dünaamiliselt krüptida oma HLS voo FairPlay abil.

FairPlay krüptimise lubamiseks peate pakkuda rakenduse serti ja rakenduse salajane võti (Küsi) kaudu FairPlay konfigureerimist tiibmutri sisu kaitse sätted.

![Sisu kaitsmine](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Kui teete krüptimise valikut, klõpsake nuppu **Rakenda**.

##<a name="next-steps"></a>Järgmised sammud

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





