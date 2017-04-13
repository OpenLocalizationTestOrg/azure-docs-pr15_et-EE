<properties 
    pageTitle="Ülevaade integreerimine kontode ja ettevõtte integreerimine pakett | Microsoft Azure'i rakendust Service | Microsoft Azure'i" 
    description="Siit saate teada kõik integreerimine kontode kohta Enterprise integreerimine keelepaketi ja loogika rakendused" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Integreerimine kontod ülevaade

## <a name="what-is-an-integration-account"></a>Mis on integreerimine konto?
Konto integreerimine on Azure'i konto, mis võimaldab ettevõtte integreerimine rakenduste haldamiseks esemeid, sealhulgas skeemid, kaardid, serdid, partnerite ja. Mis tahes integreerimise rakendus loomist tuleb kasutada juurdepääsuks skeemi, kaardil või sert, näiteks konto integreerimine.

## <a name="create-an-integration-account"></a>Looge konto integreerimine 
1. Valige **sirvimine**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. Sisestage otsinguväljale filter **integreerimine** ning valige tulemiloendist **Integreerimine kontod**     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Valige menüüst lehe ülaservas nuppu *Lisa*      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Sisestage **nimi**, valige **tellimus** , mida soovite kasutada, kas luua uue **Ressursirühma** või valige olemasoleva ressursi rühma, valige **asukoht** , kus teie konto on majutatud, valige soovitud **taseme hinnakirjad**ja seejärel valige nuppu **Loo** .   

  Selles etapis on integreeritud konto valitud asukoha ette valmistatud. See tuleb täita 1 minuti jooksul.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Värskendage lehte. Kuvatakse loendis integreerimine kontosse. Palju õnne!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Kuidas loogika rakenduse integreerimise konto linkimine
Selleks, et teie loogika rakendustele juurdepääsu kaardid, skeemid, lepingud ja muud esemeid, mis asub teie konto, peab esmalt linkimine loogika rakenduse integreerimise konto.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Siit leiate juhised loogika rakenduse integreerimise konto linkimine 

#### <a name="prerequisites"></a>Eeltingimused
- Konto integreerimine
- Loogika rakendus

>[AZURE.NOTE]Veenduge, et teie konto ja loogika rakenduse **Azure paigal** enne alustamist

1. Valige menüüst loogika rakenduse **sätete** link  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Valige **Konto** üksus keelest sätted  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Valige konto integreerimine soovite oma loogika rakenduse kaudu, **Valige konto integreerimine** rippmenüüst loendiboksi link  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Töö salvestamine  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Teile kuvatakse teade, mis näitab, et teie konto on seotud loogika rakenduse ja et kõik esemeid integreerimine teie konto on nüüd saadaval oma loogika rakendusse.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Nüüd, kui teie konto on seotud loogika rakenduse, saate minge rakenduse loogika ja B2B konnektorid, nt XML-i valideerimine, lamefaili kodeerida/dekodeerida või teisenduse abil saate luua rakendusi B2B funktsioone.  
    
## <a name="how-to-delete-an-integration-account"></a>Kuidas kustutada integreerimine konto?
1. Valige **sirvimine**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Sisestage otsinguväljale filter **integreerimine** ning valige tulemiloendist **Integreerimine kontod**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valige **konto** , mille soovite kustutada  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Valige **Kustuta** link, mis asub menüü   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Valiku kinnituseks    

## <a name="how-to-move-an-integration-account"></a>Kuidas teisaldada integreerimine konto?
Saate hõlpsasti teisaldada konto integreerimine uus tellimus ja uue ressursirühma. Kui teil on vaja teisaldada oma konto, tehke järgmist.

>[AZURE.IMPORTANT] Peate värskendada kõik skriptid uue ressursi ID-d kasutada ka pärast seda, kui teisaldate konto integreerimine.

1. Valige **sirvimine**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Sisestage otsinguväljale filter **integreerimine** ning valige tulemiloendist **Integreerimine kontod**     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Valige **konto** , mille soovite kustutada  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Valige **teisaldamiseks** linki, mis asub menüü   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Valiku kinnituseks    

## <a name="next-steps"></a>Järgmised sammud
- [Lisateavet lepingute kohta] (./app-service-logic-enterprise-integration-agreements.md "Vaadake, kuidas lepingute enterprise integreerimine")  


 