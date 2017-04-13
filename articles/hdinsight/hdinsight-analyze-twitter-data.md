<properties
    pageTitle="Twitteri analüüside koos Hadoopi rakenduses Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada taru Hadoopi Hdinsightiga kasutus sagedus kindla sõna otsimiseks klõpsake Twitteri andmete analüüsimiseks."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Abil rakenduses Hdinsightiga taru Twitteri andmete analüüsiks

Sotsiaalsete veebisaitidel on üks põhi jõude suur andmete vastuvõtmiseks. Avaliku API-de poolt saitidel nagu Twitter on kasulik andmete analüüsimine ja populaarsed trendide mõistmine. Selles õpetuses saad tweets Twitter streaming API abil, ja kasutage seejärel Apache taru Windows Azure Hdinsightiga loendi Twitteri kasutajad, kes saadetud enamik tweets, mis sisalduvad teatud sõna.

> [AZURE.NOTE] Juhised selle dokumendi jaoks on vaja Windowsi-põhiste Hdinsightiga kobar. Linux-põhine arvutikobaras teatud juhised leiate teemast [analüüsida Twitteri andmete taru sisse Hdinsightiga (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **Töökoha** Azure'i PowerShelliga installinud ja konfigureerinud. 

    Windows PowerShelli skriptide käivitamiseks peate Azure PowerShelli Käivita administraatorina ja *RemoteSigned*täitmise poliitika määramine. [Käivitage Windows PowerShelli skriptide]näha[powershell-script].

    Enne operatsioonisüsteemi Windows PowerShelli skriptide, veenduge, et olete loonud ühenduse Azure tellimuse abil järgmine cmdlet-käsk:

        Login-AzureRmAccount

    Kui teil on mitu Azure tellimust, abil järgmine cmdlet-käsk praeguse tellimuse:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **An Windows Azure Hdinsightiga kobar**. Juhised kobar ettevalmistamise, vt [kasutuselevõtt Hdinsightiga] [ hdinsight-get-started] või [sätte Hdinsightiga kogumite] [hdinsight-provision]. Peate hiljem sisse õpetuse kobar nimi.

Järgmises tabelis on loetletud selles õpetuses kasutatud failid:

Failide|Kirjeldus
---|---
/Tutorials/Twitter/Data/tweets.txt|Lähteandmete taru töö.
/Tutorials/Twitter/Output|Väljund kausta taru töö. Vaikimisi taru töö väljund faili nimi on **000000_0**.
Tutorials/Twitter/Twitter.HQL|HiveQL skriptifail.
/Tutorials/Twitter/jobstatus|Hadoopi töö olek.


##<a name="get-twitter-feed"></a>Hangi Twitteri kanal

Selles õppetükis saate kasutada [Twitteri streaming API-de][twitter-streaming-api]. Konkreetset Twitteri streaming kasutate API on [olekud või filtreerimine][twitter-statuses-filter].

>[AZURE.NOTE] Faili, mis sisaldab 10 000 tweets ja taru skriptifail (järgmises jaotises kirjeldatakse) on üles laaditud avaliku bloobimälu ümbrises. Selles jaotises saate vahele jätta, kui soovite kasutada üles laaditud faile.

JavaScript Object märke (JSON) vormingus, mis sisaldab pesastatud keerukas struktuuris talletatakse [tweets andmeid](https://dev.twitter.com/docs/platform-objects/tweets) . Asemel kirjutamist palju koodiread, kasutades tavalise programmeerimiskeel, saate muuta selle pesastatud struktuuri taru tabelisse nii, et saate seda kasutataks, struktureeritud päringu keel (SQL) – näiteks keel, mida nimetatakse HiveQL.

Twitteri kasutab OAuthi volitatud juurdepääsu oma API. OAuthi on autentimise protokoll, mis võimaldab kasutajatel rakenduste tegutseda nende nimel ilma ühiskasutuse oma parooli kinnitamine. Lisateavet leiate [oauth.net](http://oauth.net/) või suurepäraseid [OAuthi Juhend algajale](http://hueniverse.com/oauth/) Hueniverse kaudu.

Kõigepealt kasutama OAuthi on Loo uus rakendus Twitteri arendaja sait.

**Twitteri rakenduse loomine**

1. Logige sisse [https://apps.twitter.com/](https://apps.twitter.com/). Klõpsake linki **Registreeruge kohe** , kui teil pole Twitteri konto.
2. Klõpsake nuppu **Loo uus rakendus**.
3. Sisestage **nimi**, **Kirjeldus**, **veebisaidi**. Saate häälestada välja **veebisaidi** URL-i. Järgmine tabel sisaldab mõningate Näidisväärtuste kasutada.

    Väli|Väärtus
    ---|---
    Nimi|MyHDInsightApp
    Kirjeldus|MyHDInsightApp
    Veebisait|http://www.myhdinsightapp.com

4. Märkige ruut **Jah, nõustun**ja klõpsake nuppu **Loo Twitteri rakenduse**.
5. Klõpsake vahekaarti **õigused** . Vaikimisi õigused on **kirjutuskaitstud**. See on selles õpetuses jaoks piisavad.
6. Klõpsake vahekaarti **Kiirklahvid ja juurdepääs sõned** .
7. Klõpsake nuppu **Loo minu juurdepääsu luba**.
8. Klõpsake nuppu **Testi OAuthi** lehe paremas ülanurgas.
9. Kirjutage **tarbija võti**, **tarbija salajane**, **juurdepääsu luba**ja **Accessi Turbeloa salajane**. Peate hiljem sisse õpetuse väärtused.

Selles õpetuses kasutate Windows PowerShelli kõne veebiteenuse teha. .NET C# valimi, leiate teemast [analüüsi reaalajas Twitteri meeleolu HBase sisse Hdinsightiga][hdinsight-hbase-twitter-sentiment]. Muud populaarsed tööriist web helistada on [*Curl*][curl]. Curl saab alla laadida [siin][curl-download].

>[AZURE.NOTE] Curl käsu kasutamisel opsüsteemis Windows kasutada jutumärkide asemel ülakomadega suvand väärtused.

**Saada tweets**

1. Avage Windows PowerShelli integreeritud skriptimine keskkonnas (ISE). (Windows 8 avakuvale, tippige **PowerShell_ISE** ja seejärel klõpsake nuppu **Windows PowerShell ISE**. Lugege teemat, [käivitage Windows PowerShelli opsüsteemis Windows 8 ja Windows][powershell-start].)

2. Kopeerige järgmine skript skripti paani.

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Seadke esimene viie 8 muutujate skripti:


    Muutuja|Kirjeldus
    ---|---
    $clusterName|See on nimi, kuhu soovite rakenduse käivitada Hdinsightiga klaster.
    $oauth_consumer_key|See on Twitteri rakenduse **tarbija klahvi** üleskirjutatud varem Twitteri rakenduse loomisel.
    $oauth_consumer_secret|See on Twitteri rakenduse **tarbija salajane** üleskirjutatud varasemas versioonis.
    $oauth_token|See on Twitteri rakenduse **juurdepääsu luba** üleskirjutatud varasemas versioonis.
    $oauth_token_secret|See on Twitteri rakenduse **access Turbeloa salajane** üleskirjutatud varasemas versioonis.
    $destBlobName|See on väljund bloobimälu nimi. Vaikeväärtus on **tutorials/twitter/data/tweets.txt**. Kui muudate vaikeväärtus, peate Windows PowerShelli skriptide vastavalt uuendada.
    $trackString|Veebiteenuse tagastab nende märksõnadega seotud tweets. Vaikeväärtus on **Azure, Cloud, Hdinsightiga**. Kui muudate vaikeväärtus, värskendatakse teie Windows PowerShelli skriptide.
    $lineMax|Väärtus määrab mitu tweets skripti lugeda. Kulub lugeda 100 tweets umbes kolm minutit. Saate määrata rohkem, kuid see võtab rohkem aega alla laadida.

5. Vajutage klahvi **F5** skripti käivitamiseks. Kui teil tekib probleeme, lahenduseks valida kõik read ja seejärel vajutage **klahvi F8**.
6. Näete "Valmis!" väljund lõpus. Tõrketeated kuvatakse punane.

Valideerimise kord, kui märgite ruudu väljundfail **/tutorials/twitter/data/tweets.txt**, Azure'i bloobimälu Azure storage explorer või Azure PowerShelli abil. Valimi Windows PowerShelli skripti failid, leiate [kasutamine bloobimälu Hdinsightiga][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>HiveQL skripti loomine

Azure'i PowerShelli abil saate korraga töötada mitme HiveQL laused ühe või paketti HiveQL lause skripti faili. Selles õpetuses loote HiveQL skripti. Azure'i bloobimälu tuleb skripti faili üles laadida. Järgmise jaotise käivitate selle skriptifail Azure PowerShelli abil.

>[AZURE.NOTE] Taru skriptifail ja faili, mis sisaldab 10 000 tweets on üles laaditud avaliku bloobimälu ümbrises. Selles jaotises saate vahele jätta, kui soovite kasutada üles laaditud faile.

HiveQL script on tehke järgmist.

1. **Tabeli tweets_raw** juhul tabel on juba olemas.
2. **Tweets_raw taru tabeli loomine**. See ajutine liigendatud tabeli hoiab andmeid edasise eraldada, muuta, ja laadimine (ETL) töötlemine. Sektsioonid kohta leiate teavet teemast [taru õpetuse][apache-hive-tutorial].  
3. **Laadi andmete** allikas kaust, /tutorials/twitter/data. Nüüd on suur tweets andmekomplekti pesastatud JSON-vormingus ümber ajutine taru tabeli struktuuri.
3. **Tabeli tweets** juhul tabel on juba olemas.
4. **Looge tabel tweets**. Enne kui saate päringu tweets andmekomplekti suhtes, kasutades taru, peate ETL muu protsess. Selle protsessi ETL määratleb üksikasjalikuma tabeli skeem "twitter_raw" tabelis talletatud andmed.  
5. **Kirjuta tabeli lisamine**. Taru keeruka kuvatakse võrgukoosolekuga pikk MapReduce töö kogumi alusel Hadoopi kobar. Sõltuvalt teie andmekomplekti ja klaster suurust, see võib võtta umbes 10 minutit.
6. **Lisa kirjutada kataloogi**. Päringu käitamine ja väljund andmekomplekti faili. See päring tagastab loendi Twitter kasutajad, kes saadetud enamik tweets, kus asunud sõna "Azure".

**Luua skripti taru ja laadige see Azure'i**

1. Avage Windows PowerShell ISE.
2. Kopeerige järgmine skript skripti paani.

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Skripti määrata kaks esimest muutujaid.

    Muutuja|Kirjeldus
    ---|---
    $clusterName|Sisestage Hdinsightiga kobar nimi, kuhu soovite rakenduse käivitada.
    $subscriptionID|Sisestage oma Azure tellimuse ID-ga.
    $sourceDataPath|Azure'i bloobimälu talletuskoht, kus taru päringud saavad lugeda andmeid. Te ei pea muutuja muutmiseks.
    $outputPath|Azure'i bloobimälu talletuskoht, kus taru päringud saavad väljund tulemused. Te ei pea muutuja muutmiseks.
    $hqlScriptFile|Asukoht ja failinimi HiveQL skriptifail. Te ei pea muutuja muutmiseks.

5. Vajutage klahvi **F5** skripti käivitamiseks. Kui teil tekib probleeme, lahenduseks valida kõik read ja seejärel vajutage **klahvi F8**.
6. Näete "Valmis!" väljund lõpus. Tõrketeated kuvatakse punane.

Valideerimise kord, kui märgite ruudu väljundfail **/tutorials/twitter/twitter.hql**, Azure'i bloobimälu Azure storage explorer või Azure PowerShelli abil. Valimi Windows PowerShelli skripti failid, leiate [kasutamine bloobimälu Hdinsightiga][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Andmete töötlemise Twitteri taru abil

Kõigi ettevalmistamine töö lõpetamist. Nüüd saate autonoomsest taru skripti ja vaadata tulemusi.

### <a name="submit-a-hive-job"></a>Esitage taru töö

Järgmine Windows PowerShelli skripti abil saate käivitada taru skripti. Peate esimese muutuja määramiseks.

>[AZURE.NOTE] Kaks viimast jaotiste tweets ja saate üles laadida HiveQL skript kasutamine määratud $hqlScriptFile "/ tutorials/twitter/twitter.hql". Need, mis on üles laaditud avaliku bloobimälu saate kasutada, seadke $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Märkige ruut tulemused

Kasutage järgmist PowerShelli skripti taru töö väljundi kontrollitavad. Peate esmalt kahte muutujat määramiseks.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Tabeli taru kasutab \001 välja eraldaja. Eraldaja pole näha väljund.

Pärast analüüsi tulemused paigutatud Azure'i bloobimälu, saate eksportida andmed SQL Azure'i andmebaasi/SQL Server, andmete eksportimine Exceli Power Query abil või ühenduse rakenduse andmed, kasutades taru ODBC-draiver. Lisateabe saamiseks lugege teemat [Kasutamine Sqoop koos Hdinsightile][hdinsight-use-sqoop], [analüüsi viivitus lennuandmetega abil Hdinsightiga][hdinsight-analyze-flight-delay-data], [Hdinsightiga Power Query Exceli ühendamine][hdinsight-power-query], ja [Ühenduse loomine Exceli Microsoft taru ODBC-draiver Hdinsightiga][hdinsight-hive-odbc].

##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses oleme näinud, kuidas muuta ka struktureerimata JSON andmekomplekti liigendatud taru tabeli päringu, andmete uurimine ja analüüsimine Twitteri Azure Hdinsightiga abil. Lisateavet leiate järgmistest teemadest.

- [Alustamine Hdinsightiga][hdinsight-get-started]
- [Reaalajas Twitteri meeleolu HBase sisse Hdinsightiga koos analüüsida][hdinsight-hbase-twitter-sentiment]
- [Analüüsi viivitus lennuandmetega Hdinsightiga abil][hdinsight-analyze-flight-delay-data]
- [Ühenduse loomine Exceli hdinsightiga Power Query][hdinsight-power-query]
- [Ühenduse loomine Exceli hdinsightiga Microsoft taru ODBC-draiver][hdinsight-hive-odbc]
- [Hdinsightiga Sqoop kasutamine][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
