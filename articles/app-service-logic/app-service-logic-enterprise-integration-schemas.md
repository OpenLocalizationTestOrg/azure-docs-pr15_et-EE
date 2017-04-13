<properties
    pageTitle="Skeemide ja ettevõtte integreerimine pakett ülevaade | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada skeemid Enterprise integreerimine keelepaketi ja loogika rakendustega"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Lisateavet skeemide ja ettevõtte integreerimine pakett  

## <a name="why-use-a-schema"></a>Miks kasutada skeemi?
Veenduge, et saate XML-dokumendid on lubatud, valmisvormingu oodatud andmeid skeemide abil. Skeemide kasutatakse valideerimiseks sõnumid, mis on vahetatud B2B stsenaariumi.

## <a name="add-a-schema"></a>Skeemi lisamine
Azure'i portaalis:  

1. Valige **rohkem teenuseid**.  
![Kuvatõmmis, Azure'i portaalis teenustega"rohkem" esile tõstetud](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Filtri otsinguväljale, sisestage **integreerimine**ja valige **Integreerimine kontod** tulemite loendis.     
![Filtri otsinguvälja kuvatõmmis](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Valige **konto** , kuhu lisada skeem.    
![Kuvatõmmis integreerimine kontode loend](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Valige paan **skeemid** .  
![Kuvatõmmis, IEP integreerimine konto, mille "Skeemid" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Lisage skeemifaili väiksem kui 2 MB  

1. **Skeemide** tera avanev (eelmist toimingut), valige käsk **Lisa**.  
![Kuvatõmmis, skeemid labale "Lisa" nupp on esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Sisestage oma skeemi nimi. Seejärel valige üleslaaditavad skeemifaili **skeemi** tekstivälja kõrval asuvat kaustaikooni. Kui üleslaadimine on lõpule jõudnud, klõpsake nuppu **OK**.    
![Kuvatõmmis "Lisada skeem", "Väike faili" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Lisage skeemifaili, mis on suuremad kui 2 MB (kuni 8 MB)  

Juurdepääsutase bloobimälu container sõltub selle: **avaliku** või **pole Anonüümne juurdepääs**. Valige see juurdepääsutase **Azure'i salvestusruumi Exploreri** **Bloobimälu ümbriste**, klõpsake jaotises määratlemiseks bloobimälu s.o ümbris, mille soovite. Valige **Turve**ja valige vahekaart **Juurdepääsutase** .

1. Kui access bloobimälu turbetaseme on **avalik**, tehke järgmist.  
  ![Exploreri kuvatõmmis, Azure'i salvestusruumi, "Bloobimälu ümbriste", "Turvalisus" ja "Avalik" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    lisamine. Skeemi salvestusruumi üles laadida, ja kopeerige URI.  
    ![Kuvatõmmis salvestusruumi konto, URI esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. **Lisa skeem**, valige **suurt faili**ja Sisestage tekstiväljale **Sisu URI** URI.  
    ![Kuvatõmmis skeemid nuppu "Lisa" ja "Suure faili" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Kui access bloobimälu turbetaseme on **Anonüümne juurdepääs puudub**, tehke järgmist.  
  ![Exploreri kuvatõmmis, Azure'i salvestusruumi, "Bloobimälu ümbriste", "Turvalisus" ja "Pole Anonüümne juurdepääs" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    lisamine. Laadige skeemiga salvestusruumi.  
    ![Salvestusruumi konto kuvatõmmis](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Skeemi ühiskasutusega juurdepääsu signatuuri luua.  
    ![Kuvatõmmis salvestusruumi sccount, ühiskasutusega juurdepääsu allkirjade vahekaart esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. **Lisa skeem**, valige **suurt faili**ja anda ühiskasutusse antud juurdepääs allkirja URI tekstivälja **Sisu URI** .  
    ![Kuvatõmmis skeemid nuppu "Lisa" ja "Suure faili" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. **Skeemide** tera EIP integreerimine konto, peaksite nägema nüüd äsja lisatud skeemi.  
![Kuvatõmmis, EIP integreerimine konto, mille "Skeemide" ja uus skeem, mis on esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Skeemide redigeerimine
1. Valige paan **skeemid** .  
2. Valige redigeeritava keelest **skeemid** , mis avab skeemiga.
3. Enne **skeemid** , valige **Redigeeri**.  
![Kuvatõmmis, skeemid blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Valige skeemifaili, mida soovite redigeerida, kasutades funktsiooni faili valija avanevas dialoogiboksis.
5. Valige käsk **Ava** faili valija.  
![Kuvatõmmis faili valija](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Saate teate, mis näitab üles õnnestus.  

## <a name="delete-schemas"></a>Skeemide kustutamine
1. Valige paan **skeemid** .  
2. Valige kustutatava keelest **skeemid** , mis avab skeemiga.  
3. Enne **skeemid** , valige **Kustuta**.
![Kuvatõmmis, skeemid blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Valiku kinnitamiseks valige **Jah**.  
!["Kustuta skeem" kinnitusteate kuvatõmmis](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Lõpuks märkate, et skeemid **skeemid** tera loendit värskendab ja kustutamist skeemiga enam on loetletud.  
![Kuvatõmmis, EIP integreerimine konto, mille "Skeemid" esile tõstetud](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Järgmised sammud

- [Lisateavet leiate teemast hoolduspaketi Enterprise integreerimise kohta] (./app-service-logic-enterprise-integration-overview.md "Vaadake, kuidas ettevõtte integreerimine pakett").  
