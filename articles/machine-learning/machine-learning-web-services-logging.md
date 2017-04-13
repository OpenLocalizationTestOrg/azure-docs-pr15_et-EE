<properties 
    pageTitle="Logimise masina õ veebiteenuste jaoks | Microsoft Azure'i" 
    description="Saate teada, kuidas logimise masina õ veebiteenuste jaoks. Logimise sisaldab täiendavat teavet tõrkeotsinguks selle API-d." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Logimise masina õ veebiteenuste jaoks  

Selle dokumendi teave logimise võimalus klassikaline veebiteenuste. Veebiteenused võimaldavad logimine sisaldab täiendavat teavet, vaid tõrke number ja sõnum, mis aitavad teil teie arvuti õ API kõned.  

Azure'i klassikaline portaalis veebiteenuste logimise lubamiseks tehke järgmist.   

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com/) sisselogimine
2.  Klõpsake veerus vasakule funktsioonide **Masina õ**.
3.  Klõpsake tööruumi, siis **VEEBITEENUSED**.
4.  Klõpsake loendis Web services veebiteenuse nime.
5.  Klõpsake loendis lõpp-punktid lõpp-punkti nimi.
6.  Klõpsake nuppu **Konfigureeri**.
7.  *Viga* või *Kõik* **Diagnostika JÄLITA taseme** määramine ja seejärel klõpsake nuppu **Salvesta**.

Lubamiseks Azure seadme õ veebiteenuste portaali sisselogimine.

1. [Azure'i masina õ veebiteenuste](https://services.azureml.net) portaali sisse logida.
2. Klõpsake käsku klassikaline Web Services.
3.  Klõpsake loendis Web services veebiteenuse nime.
4.  Klõpsake loendis lõpp-punktid lõpp-punkti nimi.
5.  Klõpsake nuppu **Konfigureeri**.
6.  **Logimise** seatud *viga* või *Kõik*ja seejärel klõpsake nuppu **Salvesta**.

## <a name="the-effects-of-enabling-logging"></a>Mõju logimise lubamine

Kui logimine on lubatud, kõik diagnostika- ja valitud lõpp-punkti vigade logitakse Azure Storage konto seotud kasutaja tööruumi. Saate vaadata selle salvestusruumi konto Azure klassikaline portaalis oma tööruumi armatuurlaua kuvamine (kiirülevaate lühidalt jaotise all).  

Logide saab vaadata mitme konto Azure salvestusruumi uurida saadaval tööriistade abil. Lihtsaim võib olla lihtsalt liikuge salvestusruumi Azure klassikaline portaali konto ja seejärel klõpsake nuppu **ÜMBRISTE**. Seejärel näete ümbris nimega **ml-diagnostika**. See ümbris hoiab diagnostika teabe kõik web teenuse lõpp-punktide kõik tööruumid, mis on seotud salvestusruumi konto jaoks. 
 
## <a name="log-blob-detail-information"></a>Logiteave bloobimälu üksikasjad

Iga bloobimälu ümbrises hoiab diagnostika teave täpselt ühe järgmistest:

-   Batch Execution meetod on täitmine  
-   Päringu vastuse meetod on täitmine  
-   Päringu vastuse container lähtestamine
  
Iga bloobimälu nimi on eesliide järgmisel kujul: 

{Tööruumi Id} – {veebiteenuse Id} – {lõpp-punkti Id} / {Logi tüüp}  

Kus Log tüüp on üks järgmistest väärtustest.  

- paketi  
- Keskmine/taotlused  
- Keskmine/käivitamise  