<properties 
    pageTitle="Lisateavet ettevõtte integreerimine Pack dekodeerida AS2 sõnumi Connctor | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada partnerite Enterprise integreerimine keelepaketi ja loogika rakendustega" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-as2-message"></a>Dekodeerida AS2 sõnumi kasutamise alustamine

Ühenduse luua turvalisus ja usaldusväärsus ajal edastavate sõnumite dekodeerida AS2 sõnumi. Pakub digitaalset allkirjastamist, dekrüptimine ja kinnitused kaudu sõnumi likvideerimise teatised (lahti).

## <a name="create-the-connection"></a>Ühenduse loomine

### <a name="prerequisites"></a>Eeltingimused

* Azure'i konto; [tasuta konto](https://azure.microsoft.com/free) loomisel

* Konto integreerimine on vajalik dekodeerida AS2 sõnumi Connectori kasutamine. Lisateavet leiate teemast Kuidas luua [Konto](./app-service-logic-enterprise-integration-create-integration-account.md), [partnerite](./app-service-logic-enterprise-integration-partners.md) ja [AS2 lepingu](./app-service-logic-enterprise-integration-as2.md) üksikasjad

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Ühenduse loomine dekodeerida AS2 sõnumi, tehes järgmist:

1. [Loogika rakenduse loomine](./app-service-logic-create-a-logic-app.md) pakub näide.

2. See konnektor ei saa mis tahes päästikute. Käivitage rakendus loogika, nt taotluse päästik muude päästikute kasutamine.  Loogika rakenduse designer, lisage käivitamiseks ja toimingu lisada.  Valige Kuva Microsoft hallatava API-d rippmenüüst loend ja sisestage otsinguväljale "AS2".  Valige AS2 – dekodeerida AS2 sõnumi

    ![Otsingu AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Kui te pole varem loonud kõik ühendused konto, küsitakse ühenduse üksikasjad

    ![Integreerimine ühenduse loomine](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Sisestage konto üksikasjad.  Tärniga atribuudid on nõutav

  	| Atribuut   | Üksikasjad |
  	| --------   | ------- |
  	| Ühenduse nimi *    | Sisestage mis tahes ühenduse nimi |
  	| Konto * | Sisestage konto nimi. Veenduge, et teie konto ja loogika rakendus, Azure paigal |

    Kui olete lõpetanud, järgmine sarnanevad oma ühenduse üksikasjad

    ![ühenduse integreerimine](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Valige **loomine**
    
6. Pange tähele, et ühendus on loodud.  Nüüd jätkake oma loogika rakenduse juhiseid

    ![integreerimine ühenduse loonud](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Valige sisu ja päised taotluse väljundid

    ![Sisestage kohustuslikud](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>AS2 dekodeerida teeb järgmist.

* Töötleb AS2/HTTP päised
* Kinnitab allkirja (kui see on konfigureeritud)
* Sõnumite dekrüptib (kui see on konfigureeritud)
* Sõnumi decompresses (kui see on konfigureeritud)
* Vastuvõetud lahti koos algse väljamineva sõnumi lepitab
* Värskendused ja vastab loendis efektidele mitte lepingu rikkumine andmebaasi kirjetele
* Kirjutab kirjete AS2 oleku aruannete jaoks
* Väljundi last sisu on kodeeritud base64
* Määrab, kas mõni lahti on nõutav, ja kas selle lahti peaks olema sünkroonse või konfiguratsiooni asünkroonne põhjal AS2 lepingus
* Loob sünkroonse või asünkroonse lahti, (vastavalt lepingu konfiguratsioone)
* Seab selle lahti korrelatsiooni sõned ja atribuudid

##<a name="try-it-for-yourself"></a>Proovige seda ise

Miks ei anna proovida. Klõpsake [siin](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) oma loogika rakenduste AS2 funktsioonide kasutamise eeliseid täielikult loogika minirakenduse juurutamine 

## <a name="next-steps"></a>Järgmised sammud

[Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet ettevõtte integreerimine pakett") 