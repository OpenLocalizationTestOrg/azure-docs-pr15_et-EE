<properties 
    pageTitle="Konfigureerimine sisu võtme autoriseerimine poliitika Media Services .NET SDK abil | Microsoft Azure'i" 
    description="Saate teada, kuidas konfigureerida autoriseerimine põhimõtted sisu klahvi Media Services .NET SDK abil." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dünaamiline krüptimise: konfigureerimine sisu võtme autoriseerimine poliitika

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Ülevaade

Microsoft Azure Media Servicesi võimaldab esitamisel täiustatud Standard (AES) (kasutades 128-bitine) või [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)MPEG-kriips, sujuva voogesituse ja HTTP Live Streaming (HLS) voogu. AMS võimaldab teil esitamisel MÕTTEKRIIPSU voogu Widevine DRM krüptitud. PlayReady nii Widevine krüptitud määratlus levinud krüptimise (ISO/IEC 23001-7 CENC) kohta.

Media Servicesi pakub **Võtit/litsentsi teenus** , kust saate hankida kliendid võtmed AES või PlayReady/Widevine litsentside krüptitud sisu.

Kui soovite Media Servicesi vara krüptimiseks, peate krüptimisvõtme (**CommonEncryption** või **EnvelopeEncryption**) seostada vara (nagu on kirjeldatud [allpool](media-services-dotnet-create-contentkey.md)) ja ka konfigureerimiseks autoriseerimine poliitikate Key (selles artiklis kirjeldatud).

Kui voo nõuab mängija, kasutab Media Servicesi määratud klahvi dünaamiliselt krüptimiseks AES või DRM krüptimise abil sisu. Dekrüptida voo, mängija taotleb võti teenusest võtmed. Otsustada, kas kasutajal on õigus saada võti, hindab teenuse autoriseerimine poliitikate võti määratud.

Media Services toetab kinnitamise kasutajad, kes võtme taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: **avamine** või **Turbeloa** piirang. Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned **Lihtne Web sõned** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) vorming ja **JSON Web Turbeloa** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) vormingus.

Media Servicesi ei paku Secure Turbeloa teenused. Saate luua kohandatud STS või kasutada Microsoft Azure ACS, et probleem sõned. STS peab olema konfigureeritud märgiks allkirjastatud määratud võti ja probleemi väidab, et teie määratud Turbeloa piirang konfiguratsioon (selles artiklis kirjeldatud) loomiseks. Media Servicesi võtme teenus tagastab krüptovõtme kliendile kui luba on kehtiv ja luba nõuded kattuvad sisu võti konfigureeritud.

Lisateavet leiate teemast

[JWT Turbeloa authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Sujuv Azure Media Services OWIN MVC vastavalt rakenduse Azure Active Directory ja sisu võtme kohaletoimetamise JWT taotluste alusel piirata](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Kasuta Azure ACS probleemi sõned abil](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Mõni järgmine kehtida.

- Saama kasutada dünaamilise pakendit ja dünaamiline krüptimist, peate kindlasti olema vähemalt üks streaming reserveeritud ühiku. Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).
- Teie vara peab sisaldama kohandatava bitrate MP4s või kohandatava bitrate sujuva voogesituse failide kogum. Lisateavet leiate teemast [Encode vara](media-services-encode-asset.md).
- Laadige ja kodeerida oma varasid **AssetCreationOptions.StorageEncrypted** suvandi abil.
- Kui plaanite on mitu sisu võtmed, mis nõuavad sama poliitika konfiguratsiooni, on tungivalt soovitatav ühe autoriseerimine poliitika loomine ja seda kasutada mitme sisu abil.
- Klahv teenus vahemälu ContentKeyAuthorizationPolicy ja sellega seotud objektid (poliitika suvandid ja piirangute) 15 minutit.  Kui loote mõnda ContentKeyAuthorizationPolicy ja määrata, et kasutada "Turbeloa" piirang, testige seda ja seejärel poliitika kasutusele "Ava" piirang, kulub umbes 15 minutit enne poliitika aktiveerib poliitika "Ava" versiooni.
- Kui lisate või värskendada oma varade kohaletoimetamise poliitika, peate kustutama mõne olemasoleva locator (vajaduse korral) ja luua uus locator.
- Praegu ei saa krüptida, HDS streaming vorming või järkjärguline allalaadimist.


##<a name="aes-128-dynamic-encryption"></a>AES 128 dünaamiline krüptimise

###<a name="open-restriction"></a>Avatud piirang

Avatud piirang tähendab, et süsteemi toob kõigile, kellel võtme taotleb võti. See piirang võib olla kasulik katsetamiseks.

Järgmises näites on avatud autoriseerimine poliitika loob ja lisab selle sisu võti.

staatilise avaliku kehtetu AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / loomine ContentKeyAuthorizationPolicy avatud piirangutega / / ja luba poliitika IContentKeyAuthorizationPolicy poliitika loomine = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Ava autoriseerimine poliitika"). Tulemus;

Loendi<ContentKeyAuthorizationPolicyRestriction> piirangud = uue loendi<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Turbeloa piirang

Selles jaotises kirjeldatakse, kuidas luua poliitika sisu võtme autoriseerimine ja seostada sisu võti. Luba poliitika kirjeldatakse, mis autoriseerimine nõuded peavad olema täidetud määratlemiseks, kui kasutajal on õigus saate võti (nt teeb "kinnitamine võti" loend sisaldavad klahvi, mis on allkirjastatud luba).

Turbeloa piirang suvand konfigureerimiseks peate kasutama XML nõuded on luba saamiseks kirjeldamiseks. Turbeloa piirang konfiguratsiooni XML-i peab vastama järgmine XML-skeemi.

####<a id="schema"></a>Turbeloa piirang skeem
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Kui konfigureerimine on **Turbeloa** piiratud poliitika, peate määrama esmane** kinnitamine**, **väljaandja** ja **sihtrühma** parameetrid. **Esmane kinnitamine võti **sisaldab klahvi, mis on allkirjastatud luba, **väljaandja** on turvaline Turbeloa teenus, mis annab luba. **Sihtrühma** (mõnikord nimetatakse **ulatus**) kirjeldatakse soovidele vastavaks luba või ressursi luba lubab juurdepääsu. Media Servicesi võtme teenus kinnitab, et need väärtused luba vastavad väärtused mall. 

**Media Services SDK .net-i**kasutamisel saate luua piirang luba **TokenRestrictionTemplate** klassi.
Järgmises näites luuakse autoriseerimine poliitika Turbeloa piirangutega. Selles näites oleks kliendi luba, mis sisaldab esitamiseks: võti (VerificationKey), loa väljaandja ja nõutav taotluste allkirjastamine.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Luba test

Turbeloa piirang, mida kasutati võtme autoriseerimine poliitika põhjal testi märgiks saamiseks tehke järgmist.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>PlayReady dünaamiline krüptimine 

Media Services abil saate konfigureerida õigused ja piirangud, mida soovite PlayReady DRM käitusaja jõustamiseks, kui kasutaja üritab kaitstud sisu esitamiseks. 

Kui teie sisu PlayReady kaitsmisele üks asju, mida on vaja määrata oma autoriseerimine poliitika on XML-string, mis määratleb [PlayReady litsentsi malli](media-services-playready-license-template-overview.md). Media Services SDK .net-i, **PlayReadyLicenseResponseTemplate** ja **PlayReadyLicenseTemplate** tunnid aitab teil määratleda PlayReady litsentsi mall.

[Selles teemas](media-services-protect-with-drm.md) kirjeldatakse, kuidas krüptida **PlayReady** ja **Widevine**sisu.

###<a name="open-restriction"></a>Avatud piirang
    
Avatud piirang tähendab, et süsteemi toob kõigile, kellel võtme taotleb võti. See piirang võib olla kasulik katsetamiseks.

Järgmises näites on avatud autoriseerimine poliitika loob ja lisab selle sisu võti.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Turbeloa piirang

Turbeloa piirang suvand konfigureerimiseks peate kasutama XML nõuded on luba saamiseks kirjeldamiseks. Turbeloa piirang konfiguratsiooni XML-i peab vastama XML-skeemi [sellesse](#schema) lahtrisse.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Turbeloa piirang, mida kasutati võtme autoriseerimine põhjal testi märgiks poliitika kohta leiate teavet [selle](#test) jaotise. 

##<a id="types"></a>Kasutatud ContentKeyAuthorizationPolicy määratlemisel tüübid

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Järgmise juhise juurde
Nüüd, kui olete konfigureerinud sisu klahvi autoriseerimine poliitika, avage teema [konfigureerimine varade kohaletoimetamise poliitika](media-services-dotnet-configure-asset-delivery-policy.md) .
 
