<properties 
    pageTitle="Ühenduse DocumentDB konto koos protokolli tugi MongoDB | Microsoft Azure'i" 
    description="Saate teada, kuidas protokolli tugi MongoDB eelvaade nüüd saadaval koos mõne DocumentDB kontoga ühenduse loomiseks. Ühendage oma MongoDB ühendusstringi abil." 
    keywords="mongodb ühendusstring"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Kuidas luua ühendus DocumentDB konto koos MongoDB protokolli tugi

Saate teada, kuidas ühenduse Azure'i DocumentDB protokolli tugi MongoDB abil MongoDB ühenduse tekstistringi URI vormi konto.  

## <a name="get-the-accounts-connection-string-information"></a>Konto ühenduse stringi teavet

1. [Azure portaali](https://portal.azure.com)sisselogimine uues aknas.
2. Klõpsake konto tera **Vasakul** navigeerimisribal **Ühendusstring**. **Konto Blade**liikumiseks klõpsake soovitud Jumpbar nuppu **Rohkem teenuseid**, klõpsake **DocumentDB (NoSQL)** , ja valige protokolli tugi MongoDB DocumentDB kontot.

    ![Kõik sätted tera kuvatõmmis](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. **Ühendusestringi teave** tera avaneb ja on selle draiveri MongoDB, sh ette valmistatud ühendusstringi abil kontoga ühenduse loomiseks vajalikku teavet.

    ![Ühenduse stringi tera kuvatõmmis](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Ühenduse stringi nõuded

On oluline märkida, et DocumentDB toetab MongoDB ühenduse tekstistringi URI vormi, erinõuetega paar: DocumentDB kontod nõuab autentimist ja turvaline side SSL-i kaudu.  Seega ühendusstringi vorming on:

    mongodb://username:password@host:port/[database]?ssl=true

Kui string väärtused on saadaval ühendusstringi tera eespool näidatud.

- Kasutajanimi (nõutav)
    - DocumentDB konto nimi
- Parool (nõutav)
    - DocumentDB konto parooli
- Host (nõutav)
    - FQDN, DocumentDB konto
- Port (nõutav)
    - 10250
- Andmebaasi (valikuline)
    - Vaikimisi andmebaasi ühendust
- SSL-i = true (nõutav)

Näiteks kaaluge stringi ühendusteabe eespool näidatud konto.  Sobiva ühendusstringi on:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Ühendab MongoDB C# draiver
Nagu juba mainitud, vaja kõik DocumentDB kontod autentimis- ja turvaline side SSL-i kaudu. MongoDB ühenduse tekstistringi URI vorming toetab ssl = true päringustringi parameetri, töötavad koos MongoDB C# draiver nõutavad MongoClientSettings objekti lisamine MongoClient koostamisel.  Kontoteabe ülal antud, järgmised koodilõigu näitab, kuidas ühendust võtta ja nendega töötada "Tööülesanded" andmebaas.

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Järgmised sammud


- Siit saate teada, kuidas [kasutada MongoChef](documentdb-mongodb-mongochef.md) DocumentDB kontoga protokoll MongoDB kasutajatugi.
- Avasta DocumentDB protokolli tugi MongoDB [näidised](documentdb-mongodb-samples.md).

 
