<properties
    pageTitle="Twitteri analüüside koos Apache taru kohta Hdinsightiga | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Python Tweets, mis sisaldavad märksõnu ja seejärel kasutage taru ja Hadoopi Hdinsightile Twitteri toorandmetega muutuda otsitavate taru tabeli talletamiseks."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Abil rakenduses Hdinsightiga taru Twitteri andmete analüüsiks

Selles dokumendis tweets Twitter streaming API abil saate ja seejärel kasutage Apache taru Linux-põhine Hdinsightiga kobar-protsess on JSON vormindatud andmed. Tulemus Twitter kasutajad, kes saadetud enamik tweets, mis sisalduvad teatud sõna loendit.

> [AZURE.NOTE] Kuigi üksikute osade seda dokumenti saab kasutada koos Windowsi-põhiste Hdinsightiga kogumite (nt Python), põhinevad palju juhiseid kasutades Linux-põhine Hdinsightiga kobar. Windowsi-põhiste arvutikobaras teatud juhised leiate teemast [analüüsida Twitteri andmete taru Hdinsightiga sisse](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- __Windows Azure Hdinsightiga Linux-põhine kobar__. Klaster loomise kohta leiate artiklist [Alustamine Linux-põhine Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md) luua klaster juhised.

- Kui __SSH kliendi__. Lisateavet kasutades SSH Linux-põhine Hdinsightiga leiate järgmistest artiklitest:

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ ja [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Kanali Twitter hankimine

Twitteri võimaldab teil läbi REST API dokumendina JavaScript Object märke (JSON) [iga säutsu andmete](https://dev.twitter.com/docs/platform-objects/tweets) toomiseks. [OAuthi](http://oauth.net) on vaja API autentimist. Peate looma ka on _Twitteri rakendus_ , mis sisaldab API juurdepääsu sätted.

###<a name="create-a-twitter-application"></a>Twitteri rakenduse loomine

1. Veebibrauseris, logige sisse [https://apps.twitter.com/](https://apps.twitter.com/). Klõpsake linki **Registreeruge kohe** , kui teil pole Twitteri konto.
2. Klõpsake nuppu **Loo uus rakendus**.
3. Sisestage **nimi**, **Kirjeldus**, **veebisaidi**. Saate häälestada välja **veebisaidi** URL-i. Järgmine tabel sisaldab mõningate Näidisväärtuste kasutada.

  	| Väli | Väärtus |
  	|:----- |:----- |
  	| Nimi  | MyHDInsightApp |
  	| Kirjeldus | MyHDInsightApp |
  	| Veebisait | http://www.myhdinsightapp.com |
    
4. Märkige ruut **Jah, nõustun**ja klõpsake nuppu **Loo Twitteri rakenduse**.
5. Klõpsake vahekaarti **õigused** . Vaikimisi õigused on **kirjutuskaitstud**. See on selles õpetuses jaoks piisavad.
6. Klõpsake vahekaarti **Kiirklahvid ja juurdepääs sõned** .
7. Klõpsake nuppu **Loo minu juurdepääsu luba**.
8. Klõpsake nuppu **Testi OAuthi** lehe paremas ülanurgas.
9. Kirjutage **tarbija võti**, **tarbija salajane**, **juurdepääsu luba**ja **Accessi Turbeloa salajane**. Väärtused on hiljem vaja.

>[AZURE.NOTE] Curl käsu kasutamisel opsüsteemis Windows kasutada jutumärkide asemel ülakomadega suvand väärtused.

###<a name="download-tweets"></a>Tweets allalaadimine

Järgmine kood Python on 10 000 tweets alla laadida Twitteri ja salvestada fail nimega __tweets.txt__.

> [AZURE.NOTE] Järgmised toimingud tehakse Hdinsightiga kobar, kuna Python on juba installitud.

1. Ühendada Hdinsightiga klaster SSH abil.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Kui kasutasite konto SSH kasutaja parooli, palutakse teil sisestada. Kui kasutasite avalik võti, peate kasutama funktsiooni `-i` parameetri määrama kattuvad privaatvõti. Näiteks `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Lisateavet kasutades SSH Linux-põhine Hdinsightiga leiate järgmistest artiklitest:
    
    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Vaikimisi on installitud __pip__ kasuliku Hdinsightiga pea sõlme. Installimiseks kasutada järgmist ja seejärel värskendage seda kasuliku.

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Koodi alla laadida tweets tugineb [Tweepy](http://www.tweepy.org/) ja [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Need installimiseks kasutage järgmine käsk:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Vältimiseks on InsecurePlatform hoiatus ühendamisel Twitter SSL-i kaudu Python on bittide python openssl, python-arendaja, libffi-arendaja, libssl-arendaja, pyOpenSSL ja taotlusi [turvalisuse] eemaldamise kohta.
    >
    > Tweepy v3.2.0 kasutatakse [tõrge](https://github.com/tweepy/tweepy/issues/576) , mis võivad tekkida töötlemine tweets vältimiseks.

4. Järgmise käsu abil saate luua uus fail nimega __gettweets.py__:

        nano gettweets.py

5. Kasutage järgmisi __gettweets.py__ faili sisu. Asendage kohatäide teave __tarbija\_salajane__, __tarbija\_klahvi__, __juurdepääsu /\_Turbeloa__, ja __juurdepääsu\_Turbeloa\_salajane__ Twitteri rakenduse teabega.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Faili salvestamiseks kasutage __klahvikombinatsiooni Ctrl + X__ja seejärel __Y__ .

7. Käivitage fail ja laadige tweets järgmise käsu abil:

        python gettweets.py

    Edenemise näidik tuleks kuvada ja loendada kuni 100%, nagu tweets alla ja salvestada faili.

    > [AZURE.NOTE] Kui see võtab väga kaua aega tasemetele riba edenemine, muudate filtri jälgida trendid teemasid; Kui palju tweets filtreerimine on teema kohta, saate avada väga kiiresti vaja 10000 tweets.

###<a name="upload-the-data"></a>Andmete üleslaadimine

Andmete üleslaadimiseks WASB (hajutatud failisüsteemi kasutavad Hdinsightiga) saate kasutada järgmisi käske:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

See salvestab andmed kohas, kus kõik sõlmed klaster pääsete juurde.

##<a name="run-the-hiveql-job"></a>Töö HiveQL


1. Järgmise käsu abil saate luua HiveQL laused sisaldava faili:

        nano twitter.hql
    
    Kasutage järgmist faili sisu:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Vajutage __klahvikombinatsiooni Ctrl + X__ja seejärel vajutage klahvi __Y__ , kuhu soovite faili salvestada.

4. Järgmise käsu abil saate käivitada HiveQL, fail sisaldab:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    See laadib taru shell, käivitage selle HiveQL __twitter.hql__ faili ja lõpuks tagastada on `jdbc:hive2//localhost:10001/>` küsimus.
    
5. Kaudu Linnulennutee kuvatakse vastav viip, kasutage järgmist kinnitamaks, et saate valida andmed tabelist __tweets__ loodud HiveQL __twitter.hql__ faili:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Tagastab kuni 10 tweets, mis sisaldavad sõna __Azure'i__ sõnumi tekst.

##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses oleme näinud, kuidas muuta ka struktureerimata JSON andmekomplekti liigendatud taru tabeli päringu, andmete uurimine ja analüüsimine Twitteri Azure Hdinsightiga abil. Lisateavet leiate järgmistest teemadest.

- [Alustamine Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analüüsi viivitus lennuandmetega Hdinsightiga abil](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
