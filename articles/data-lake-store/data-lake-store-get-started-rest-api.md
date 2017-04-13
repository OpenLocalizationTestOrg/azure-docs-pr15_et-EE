<properties 
   pageTitle="Alustamine Lake andmesalve REST API abil | Microsoft Azure'i" 
   description="Toiminguid Lake andmesalve WebHDFS REST API-de abil" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Azure'i Lake andmesalve REST API-de kasutamise alustamine

> [AZURE.SELECTOR]
- [Portaal](data-lake-store-get-started-portal.md)
- [PowerShelli](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-GA](data-lake-store-get-started-rest-api.md)
- [Azure'i CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Sellest artiklist saate teada, kuidas WebHDFS REST API-d ja andmete Lake poe REST API-de abil kontohaldus kui ka Azure andmesalve Lake failisüsteemi toiminguid teha. Azure'i andmesalve Lake seab oma REST API-de konto toimingute. Siiski, kuna Lake andmesalve ei ühildu HDFS ja Hadoopi ökosüsteemi, see toetab WebHDFS REST API-de failisüsteemi toimingute abil.

>[AZURE.NOTE] Üksikasjalikku teavet REST API Lake andmesalve kasutajatugi, leiate [Azure'i andmed Lake poe REST API viide](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- Saate **luua rakenduse Azure Active Directory**. Saate Azure AD rakendus autentida Lake andmesalve rakenduse Azure AD. On erinevate lähenemisviisi autentimiseks Azure AD, mis on **lõppkasutaja autentimist** või **teenusest autentimist**. Juhised ja autentida Lisateavet leiate teemast [andmete Lake poe autentimise abil Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Selles artiklis kasutab cURL demonstreerib REST API helistada Lake andmesalve konto suhtes.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Kuidas teha autentida, kasutades Azure Active Directory?

Saate kahel viisil autentida Azure Active Directory abil.

### <a name="end-user-authentication-interactive"></a>Lõppkasutaja autentimine (interaktiivne)

Selle stsenaariumi korral rakenduse kasutajalt sisse logida ja kõik toimingud sooritatakse kasutaja kontekstis. Järgmiste toimingute interaktiivse autentimiseks.

1. Oma rakenduse kaudu ümber suunata kasutaja järgmine URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > peab olema kodeeritud URL-is kasutamiseks. Niisiis, https://localhost, kasutada `https%3A%2F%2Flocalhost`)

    Selleks, et selles õppetükis saate kohatäite väärtusi, URL-i kohal asendada ja kleepida veebibrauseri aadressiribale. Teid suunatakse ümber autentida abil oma Azure Logi sisse. Kui logite edukalt, kuvatakse vastus brauseri aadressiribale. Vastuse saab järgmises vormingus:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Jäädvustada autoriseerimine kood vastus. Selles õpetuses mõeldud saate kopeerida luba kood veebibrauseri aadressiribale ja edastama selle postituse taotluse Turbeloa lõpp-punkti, nagu allpool näidatud:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Sel juhul on \<REDIRECT-URI > peate olema kodeeritud.

3. Vastus on JSON-objekti, mis sisaldab juurdepääsu sümboolse (nt `"access_token": "<ACCESS_TOKEN>"`) ja Värskenda luba (nt `"refresh_token": "<REFRESH_TOKEN>"`). Teie rakendus kasutab juurdepääsu luba kasutamisel Azure'i andmesalve Lake ja Värskenda luba saada teise juurdepääsu luba juurdepääsu sümboolse aegumisel.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kui juurdepääsu luba on aegunud, saate taotleda uut juurdepääsu luba abil Värskenda luba, nagu allpool näidatud:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Interaktiivne kasutaja autentimise kohta leiate lisateavet teemast [meilivoo anda kood](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Teenusest autentimine (mitte-interaktiivne)

Selle stsenaariumi korral on rakendus pakub oma mandaadi toimingute tegemiseks. Selleks väljastage postituse koosolekukutse nagu allpool näidatud. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Selle päringu väljund sisaldab mõnda autoriseerimine luba (tähistatud `access-token` allpool väljund), mida teil läheb hiljem kõnede REST API-ga. Salvestage see autentimise luba tekstifaili; See on vaja selle artikli.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Selles artiklis kasutab funktsiooni **- interaktiivne** moodust. Mitte-interaktiivne (teenusest – kõned) kohta leiate lisateavet teemast [teenuse teenuse kõnedele mandaadi abil](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Lake andmesalve konto loomine

Selle toimingu põhineb REST API-ga määratletud kõne [siin](https://msdn.microsoft.com/library/mt694078.aspx).

Kasutage cURL järgmine käsk. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ülaltoodud käsk Asenda \< `REDACTED` \> autoriseerimine luba abil saate tuua varasemas versioonis. Taotluse last selle käsu sisaldub **input.json** fail, mille jaoks on esitatud selle `-d` ülaltoodud parameeter. Input.json faili sisu näeb välja järgmine:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Lake andmesalve konto kaustade loomine

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Kasutage cURL järgmine käsk. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Ülaltoodud käsk Asenda \< `REDACTED` \> autoriseerimine luba abil saate tuua varasemas versioonis. See käsk loob kausta nimega **mytempdir** juurkausta Lake andmesalve kontole.

Kui toiming on lõpule jõudnud edukalt peaksite nägema vastuse umbes järgmine:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Loendi kaustad Lake andmesalve konto

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Kasutage cURL järgmine käsk. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Ülaltoodud käsk Asenda \< `REDACTED` \> autoriseerimine luba abil saate tuua varasemas versioonis.

Kui toiming on lõpule jõudnud edukalt peaksite nägema vastuse umbes järgmine:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Laadi andmed Lake andmesalve kontole

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

WebHDFS REST API abil andmete üleslaadimine on kaheosaline toiming, mis toimub järgmiselt.

1. HTTP sellele taotluse esitamine saatmisele andmed faili üles laadida. Järgmine käsk Asenda ** \<yourstorename >** teie Lake andmesalve nimi.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Selle käsu väljund olla sisaldab ajutine ümbersuunamine URL-i, nagu allpool esitatud menüüga.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Nüüd tuleb esitada teise HTTP sellele taotluse vastu lisatud vastuse atribuudi **asukoha** jaoks URL-i. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Väljund on järgmine:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Lake andmesalve konto andmete lugemine

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Andmete lugemine andmete Lake poest konto on kaheosaline toiming.

* Esmalt esitada toomine taotluse vastu lõpp-punkti `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Tagastab asukoht, kuhu soovite esitada järgmine GET-päringu abil.
* Seejärel esitamisel toomine taotluse vastu lõpp-punkti `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. See kuvab soovitud faili sisu.

Siiski, kuna ei erine sisendparameetrite esimese ja teise etapi, saate kasutada funktsiooni `-L` parameetri esimese taotleda. `-L`suvand põhiosas ühendab kaks kutsed ühte ja teeb cURL uuesti taotluse uude asukohta. Lõpuks väljundi taotluse kõned ei kuvata, nagu allpool näidatud. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Peaksite nägema järgmine väljund:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Lake andmesalve konto faili ümbernimetamine

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Faili ümbernimetamine cURL järgmise käsu abil. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Peaksite nägema järgmine väljund:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Faili kustutamine Lake andmesalve konto kaudu

Selle toimingu põhineb WebHDFS REST API-ga määratletud kõne [siin](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Faili kustutamine cURL järgmise käsu abil. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Peaksite nägema umbes selline väljund:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Lake andmesalve konto kustutamine

Selle toimingu põhineb REST API-ga määratletud kõne [siin](https://msdn.microsoft.com/library/mt694075.aspx).

CURL järgmise käsu abil saate kustutada Lake andmesalve konto. Asendage ** \<yourstorename >** teie Lake andmesalve nimi.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Peaksite nägema umbes selline väljund:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Vt ka

- [Avatud Source Big Data rakendused ühildu Azure'i andmesalve Lake](data-lake-store-compatible-oss-other-applications.md)
 
