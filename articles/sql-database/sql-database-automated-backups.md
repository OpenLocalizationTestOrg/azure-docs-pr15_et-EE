<properties
   pageTitle="SQL-andmebaasi varukoopiate - automaatne, geograafilise liigne | Microsoft Azure'i" 
   description="SQL-andmebaasi automaatselt loob kohaliku andmebaasi varukoopia iga viie minuti ja kasutab geo-koondamise Azure lugemisõigus – geograafilise liigne salvestusruumi (RA GRS). "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Lisateavet SQL-andmebaasi varundamine

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-andmebaasi automaatselt loob kohaliku andmebaasi varukoopia iga mõne minuti ja kasutab Azure lugemisõigus – geograafilise liigne salvestusruumi geo-koondamise. Andmebaasi varundamine on mis tahes äri järjepidevus ja avariijärgne taastamine strateegia oluline osa, sest andmete kaitsmine juhusliku rikkumise või kustutamise kaudu. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Mis on SQL-andmebaasi varukoopia?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

SQL-andmebaasi varukoopia sisaldab nii kohaliku andmebaasi varundamine varukoopiaid geograafilise liigne. Nende varukoopiate luuakse automaatselt ja lisatasu. Te ei pea neid tehakse midagi tegema.

<!----------------- 
    Explains first component of the backup feature
------------------>

Kohaliku varufailide SQL-andmebaasi kasutab SQL serveri tehnoloogia [täielik](https://msdn.microsoft.com/library/ms186289.aspx), [vahe](https://msdn.microsoft.com/library/ms175526.aspx )ja [tehingulogi](https://msdn.microsoft.com/library/ms191429.aspx) varundamiseks. Tehingu Logi varundatakse iga viie minuti järel, mis võimaldab teil teha sama andmebaasi majutava serveri punkti õigel ajal taastada. Andmebaasi taastamisel teenuse arvud mis varukoopiate vajavad taastamist täis, vahe ja tehingu Logi välja.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Kasutage kohaliku andmebaasi varukoopia.

- Andmebaasi taastamine mõne aja punkti säilitamise aja jooksul. Andmebaasi varukoopia saate andmebaasi taastamiseks mõne aja punkti, taastada kustutatud andmebaasi on kustutatud aeg või andmebaasi taastamine teise geograafilist alale. Taastamise läbi viia, lugege teemat [andmebaasi andmebaasi varukoopia põhjal taastamine](sql-database-recovery-using-backups.md).

- Kopeerige andmebaasi SQL serveri samas või mõnes muus piirkonnas. Kopeeri vastab ülekandega SQL andmebaasis. Teha koopia, lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md).

- Arhiivida Lisaks varukoopia säilitusperiood andmebaasi varukoopia. Teha ka arhiivi, [SQL-andmebaasiga, et mõne BACPAC ekspordi](sql-database-export.md) faili. Seejärel saate BACPAC pikemaks soovite arhiivida ja talletage oma säilitusperiood lisaks. Või kasutage funktsiooni BACPAC SQL serveri, kas kohapeal või Azure virtuaalse masina (VM) oma andmebaasi koopia edastamiseks.

<!----------------- 
    Explains first component of the backup feature
------------------>

SQL-andmebaasi kasutab geograafilise liigne varufailide [Azure Storage kopeerimine](../storage/storage-redundancy.md). SQL-andmebaasi talletatakse kohaliku andmebaasi varundusfaili [Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) konto. Azure'i tiražeerib varukoopia failid on [seotud data Centeri kaudu](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Kasutage geograafilise liigne varukoopia.

- Andmebaasi taastamine geograafiliste muule alale juhuks, kui te ei pääse oma esmase andmebaasi piirkonnast andmebaasi varukoopia. 

>[AZURE.NOTE] Azure'i salvestusruumi, viitab Termini *kopeerimine* failide kopeerimine ühest kohast teise. Hoides mitme teisene andmebaaside sünkroonitud esmane andmebaasi viitab SQL's *andmebaasi tiražeerimine* . 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Varukoopia mäluruumi on kaasatud tasuta?

SQL-andmebaasi pakub kuni 200% salvestusruumi maksimaalne ettevalmistatud andmebaasi varukoopia täiendava tasuta salvestusruumi. Näiteks, kui teil on standardne DB eksemplari ettevalmistatud DB suurusega 250 GB, peate 500 GB salvestusruumi varukoopia lisatasu. Kui teie andmebaasi ületab pakutavaid varukoopia talletamist, saate vähendada säilitusperiood Azure'i klienditoega. Teine võimalus on lisatööd varukoopia salvestusruumi, mis on lugemisõigus – geograafiliselt liigsete salvestusruumi (RA GRS) standardmäär arve eest maksta. 

## <a name="how-often-do-backups-happen"></a>Kui sageli varukoopiate juhtub?

Kohalik andmebaas varufailide, täielik andmebaasi varundamine juhtub iga nädal, erinevat andmebaasi varundamine juhtub tunni ja tehingu Logi varundatakse iga viie minuti järel. Esimene täielik varundus on kavas kohe pärast andmebaasi loomist. See tavaliselt 30 minuti jooksul lõpule jõudnud, kuid see võib võtta kauem kui andmebaas on oluline suurusega. Näiteks saate algse varundamise kauem taastatud andmebaasi või andmebaasi koopia. Pärast esimese täielik varukoopia, on kõik edasise varukoopiaid automaatselt ajastatud ja hallatavate vaikselt taustal. Täpse ajastuse täielik ja [erinevat](https://msdn.microsoft.com/library/ms175526.aspx) andmebaasi varundamine määratakse selle nõuded üldise süsteemi töökoormus. 

Geograafilise liigne varufailide täielik ja erinev varukoopia kopeeritakse Azure Storage dispersioonanalüüs skeemi järgi.

## <a name="how-long-do-you-keep-my-backups"></a>Kui kaua hoida oma varukoopiate?

Iga SQL-andmebaasi varundamine on säilitusperiood, mis põhineb [teenuse kiht](sql-database-service-tiers.md) andmebaasi. Säilitusperiood andmebaasi on:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Põhilised teenuse kiht on seitse päeva.
- Standard teenuse kiht on 35 päeva.
- Premium teenuse kiht on 35 päeva.


Kui vahetate oma andmebaasi Standard- või Premium teenus astme Basic, salvestatakse varukoopiaid seitse päeva. Kõik olemasolevad varukoopiad üle seitsme päeva pole enam saadaval. 

Kui täiendate oma andmebaasi lihtsa teenuse kiht Standard-või Premium, hoiab SQL-andmebaasi seni, kuni need 35 päevase olemasolevad varukoopiad. Uue varukoopiate hoiab esinevad 35 päeva.
 
Kui kustutate andmebaasi, hoiab SQL-andmebaasi varukoopiaid oleks Online'i andmebaasi samal viisil. Oletame näiteks, et kustutate lihtsa andmebaasi, mis on seitse päeva jooksul säilitus. Kolm päeva on salvestatud varukoopia, mis on nelja-päevase.

>[AZURE.IMPORTANT]
    Kui kustutate Azure SQL serveri majutatakse SQL andmebaase, kustutatakse ka kõigile andmebaasidele, mis kuuluvad server ja see ei saa taastada. Te ei saa taastada kustutatud server.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Järgmised sammud

Andmebaasi varundamine on mis tahes äri järjepidevus ja avariijärgne taastamine strateegia oluline osa, sest andmete kaitsmine juhusliku rikkumise või kustutamise kaudu. Kuvamiseks kuidas andmebaasi varukoopiate laiema strateegia, vt [äri järjepidevuse ülevaade](sql-database-business-continuity.md).


