<properties
    pageTitle="Saate teada, kuidas kodeerida või dekodeerida tasapinnalise faile, mis on Enterprise integreerimine keelepaketi ja loogika rakenduste kasutamise | Microsoft Azure'i rakendust Service | Microsoft Azure'i"
    description="Ettevõtte integreerimine keelepaketi ja loogika rakenduste funktsioonide kasutamiseks kodeerida või dekodeerida tasapinnalise failid"
    services="app-service\logic"
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-flat-files"></a>Ettevõtte tasapinnalise failide integreerimine

## <a name="overview"></a>Ülevaade

Kui soovite kodeerida XML-sisu enne saatmist läbi business partneri stsenaariumis ettevõttelt ettevõttele (B2B). Loogika rakenduses tehtud Azure'i rakenduse teenuse funktsiooni loogika rakenduste abil saate lamefaili kodeering konnektor tehke järgmist. Loogika rakendus, mille loote pääsevad oma XML-i sisu erinevatest allikatest, sh lisamispäästiku HTTP taotluse, mõnest muust rakendusest, või isegi mõnest paljud [konnektorid](../connectors/apis-list.md). Loogika rakenduste kohta leiate lisateavet teemast [loogika rakenduste dokumentatsiooni](./app-service-logic-what-are-logic-apps.md "loogika rakenduste kohta lisateavet").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Kuidas luua lamefaili kodeering konnektor

Järgmiste juhiste abil lamefaili kodeering konnektori rakenduse loogika lisamine.

1. Saate luua loogika rakendus ja [linkida selle kontoga integreerimine](./app-service-logic-enterprise-integration-accounts.md "Õpi loogika rakenduse integreerimise kontoga linkida"). Selle konto sisaldab skeemi XML-andmete kodeerida kasutate.  
2. Loogika rakenduse **taotluse – kui an HTTP-päring** päästik lisada.  
![Kuvatõmmis päästik valimine](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Lisage lamefaili kodeering toiming järgmiselt:

    lisamine. Valige **pluss** märk.

    b. Valige link **Lisa toimingu** (kuvatakse, kui olete valinud plussmärk).

    c. Sisestage otsinguväljale *tasapinnalise* filtreerimiseks kõik toimingud, et seda, mida soovite kasutada.

    d. Valige loendist suvand **Tasapinnalise faili kodeering** .   
![Kuvatõmmis, tasapinnalise faili kodeering suvand](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Valige dialoogiboksis **Tasapinnalise faili kodeering** tekstivälja **sisu** .  
![Tekstivälja sisu kuvatõmmis](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Valige sisu, mida soovite kodeerida keha silt. Sildi sisu asustada sisu välja.     
![Kehateksti sildi kuvatõmmis](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Valige väljal **Skeemi nimi** ja valige skeemi, mida soovite kasutada kodeerida Sisestuskeel sisu.    
![Kuvatõmmis skeemi nimi loendiboks](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Salvestage oma töö.   
![Salvestamine kuvatõmmis ikoon](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Selles etapis, kui olete lõpetanud, kuidas häälestada oma lamefaili kodeering konnektor. Tõeline rakenduses, võite ärivaldkonna rakenduses, nagu näiteks Salesforce'i encoded andmete talletamiseks. Või saate saata, et kodeeritud andmed on börsipäev partneri. Toimingu saatmiseks kodeering toimingu väljund Salesforce'i või teie kaubanduspartner, teised konnektorid esitatud mõni abil saate hõlpsasti lisada.

Nüüd saate oma konnektor testimiseks paluda HTTP lõpp-punkti ja XML-sisu kaasamise taotluse keha.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Kuidas luua lamefaili dekodeerimise konnektor

>[AZURE.NOTE] Nende juhiste täitmiseks peate skeemifaili juba üleslaaditud integreerimine kontole.

1. Loogika rakenduse **taotluse – kui an HTTP-päring** päästik lisada.  
![Kuvatõmmis päästik valimine](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Lisage lamefaili dekodeerimise toiming järgmiselt:

    lisamine. Valige **pluss** märk.

    b. Valige link **Lisa toimingu** (kuvatakse, kui olete valinud plussmärk).

    c. Sisestage otsinguväljale *tasapinnalise* filtreerimiseks kõik toimingud, et seda, mida soovite kasutada.

    d. Valige loendist suvand **Tasapinnalise faili dekodeerimise** .   
![Kuvatõmmis, tasapinnalise faili dekodeerimise suvand](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Valige **sisujuhtelement** . See tekitab varasemates juhiseid, mille abil saate sisu dekodeerida sisu loendit. Teade, et sissetulevad HTTP-päringu *sisu* on saadaval dekodeerida sisu kasutada. Võite sisestada ka sisu dekodeerida otse juhtelemendi **sisu** .     
- Valige *sisu* silt. Teate kehasse silt on nüüd **sisujuhtelement** .
- Valige skeemi, mida soovite kasutada dekodeerida sisu nime. Järgmine pilt näitab, et *OrderFile* on valitud skeemi nimi. Selle skeemi nimi laaditud oli varem integreerimine kontole.

 ![Kuvatõmmis, tasapinnalise faili dekodeerimise dialoogiboks](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Salvestage oma töö.  
![Salvestamine kuvatõmmis ikoon](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Selles etapis, kui olete lõpetanud, häälestada oma lamefaili dekodeerimise konnektor. Tõeline rakenduses, võite rakenduses ärivaldkonna – nt Salesforce'i dekodeeritud andmete talletamiseks. Saate hõlpsasti lisada toimingu Salesforce'i dekodeerimise toimingu väljund saatmiseks.

Nüüd saate oma konnektor testimiseks paluda HTTP lõpp-punkti ja sh XML-sisu, mida soovite dekodeerida taotluse kehas.  

## <a name="next-steps"></a>Järgmised sammud
- [Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Lisateavet Enterprise integreerimine keelepaketi").  
