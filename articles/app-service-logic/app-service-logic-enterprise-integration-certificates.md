
<properties
    pageTitle="Sertide kasutamine ettevõtte integreerimine Pack | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada serdid Enterprise integreerimine keelepaketi ja loogika rakendused"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Lisateavet serdid ja ettevõtte integreerimine pakett

## <a name="overview"></a>Ülevaade
Ettevõtte integreerimine kasutab secure B2B suhtlus serdid. Saate oma ettevõtte integreerimine rakendustes kahte tüüpi serte:

- Avaliku sertifikaatide kohta, mis tuleb osta sertimiskeskus (CA).
- Privaatne sertifikaatide kohta, millega saate ise väljastada. Need serdid on mõnikord nimetatakse iseallkirjastatud serdid.


## <a name="what-are-certificates"></a>Mis on serdid?
Serdid on digitaaldokumentide identiteedi osalejate elektroonilise side ja et tagada elektroonilise side.

## <a name="why-use-certificates"></a>Miks kasutada serdid?
Mõnikord B2B suhtlus peab konfidentsiaalseks. Ettevõtte integreerimine kasutab serdid secure teadete kahel viisil:

- Sisu Sõnumite krüptimine
- Sõnumite digitaalseks allkirjastamiseks järgi  

## <a name="how-do-you-upload-certificates"></a>Kuidas üles laadida serdid?

### <a name="public-certificates"></a>Avaliku serdid
Kasutada *avaliku serdi* loogika rakenduste B2B võimalustega, peate esmalt laadige üles sert integreerimine kontole. *Iseallkirjastatud serdi*kasutamiseks teiselt, tuleb esmalt üleslaadimist [Azure'i klahvi Vault](../key-vault/key-vault-get-started.md "klahvi Vault teave").

Pärast üleslaadimist sert, on saadaval, mis aitavad teil turvaline kui määratlete nende atribuutide [lepingute](./app-service-logic-enterprise-integration-agreements.md) loodavate sõnumite B2B.  

Siit leiate üksikasjalikud juhised üleslaadimise avaliku sertide integreerimine kontole pärast sisselogimist Azure portaali:

1. Valige **Sirvi**.  
    ![Valige Sirvi](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. Sisestage otsinguväljale filter **integreerimine** ja seejärel valige tulemite loendist **Integreerimine kontod** .     
    ![Valige kontod integreerimine](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Valige integreerimine konto, millele soovite lisada serdi.  
    ![Valige integreerimine konto, millele soovite lisada sert](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Valige paan **serdid** .  
    ![Valige paan serdid](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. **Sertide** tera, mis avab, klõpsake nuppu **Lisa** .
    ![Klõpsake nuppu Lisa](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Sisestage teie serdi **nimi** ja seejärel valige serdi tüüp. (Selles näites me kasutada avaliku serdi tüüp.) Valige **sert** tekstiväljast paremal kaustaikooni.

7. Kuupäevavalija faili avamisel otsimine ja valige serdi fail, mille soovite üles laadida oma konto.

8. Valige sert ja seejärel klõpsake **nuppu OK** faili valija. See kinnitab ja lisatud sert oma konto integreerimine.

8. Lõpuks uuesti sisse labale **serdi lisamine** nuppu **OK** .  
    ![Klõpsake nuppu OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. Umbes minuti, kuvatakse teade, mis näitab, et serdi üleslaadimine on lõpule jõudnud.

10. Valige paan **serdid** . Peaksite nägema äsja lisatud sert.  
    ![Vaadake uut serti](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Privaatne serdid
Saate üles laadida privaatne serdid integreerimine kontole järgi järgmiselt:  

1. [Laadige oma privaatvõti, et klahv Vault] (../key-vault/key-vault-get-started.md "Lisateavet võtme Vault").  

    > [AZURE.TIP] Tuleb lubada funktsiooni loogika rakendused rakendus Azure'i teenuse klahv Vault toiminguid. Te saate anda juurdepääsu loogika rakenduste teenuse põhilise järgmist PowerShelli käsu abil:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Privaatne serdi loomine.  

3. Privaatne serdi üleslaadimine oma konto.

Kui olete eelmisi juhiseid, saate privaatne serdi loomine lepingud.

Üksikasjalikud juhised üleslaadimise privaatne sertide integreerimine kontole pärast sisselogimist Azure portaali on:  

1. Valige **Sirvi**.  
    ![Laadida oma isiklike sertide integreerimine kontole](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. Sisestage otsinguväljale filter **integreerimine** ja seejärel valige tulemite loendist **Integreerimine kontod** .     
    ![Valige kontod integreerimine](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Valige integreerimine konto, millele soovite lisada serdi.  
    ![Valige integreerimine konto, millele soovite lisada sert](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Valige paan **serdid** .  
    ![Valige paan serdid](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. **Sertide** tera, mis avab, klõpsake nuppu **Lisa** .
    ![Klõpsake nuppu Lisa](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Sisestage teie serdi **nimi** ja seejärel valige serdi tüüp. (Selles näites me kasutada avaliku serdi tüüp.) Valige **sert** tekstiväljast paremal kaustaikooni.

7. Kuupäevavalija faili avamisel otsimine ja valige serdi fail, mille soovite üles laadida oma konto.

8. Kui olete valinud sert, valige faili valija **OK** . See toiming kinnitab serti ja lisatud see oma konto.

9. Lõpuks uuesti sisse labale **serdi lisamine** nuppu **OK** .  
    ![Serdi lisamine](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. Umbes minuti, kuvatakse teade, mis näitab, et serdi üleslaadimine on lõpule jõudnud.

11. Valige paan **serdid** . Peaksite nägema äsja lisatud sert.
    ![Vaadake uut serti](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Pärast üleslaadimist sert, on saadaval, mis aitavad teil turvaline sõnumite B2B kui määratlete nende atribuutide [lepingutes](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Järgmised sammud
- [Loogika rakenduse, mis kasutab B2B funktsioonide loomine](./app-service-logic-enterprise-integration-b2b.md)  
- [B2B lepingu loomine](./app-service-logic-enterprise-integration-agreements.md)  
- [Lisateavet leiate teemast klahvi Vault kohta] (../key-vault/key-vault-get-started.md "Võtme Vault teave")  
