<properties 
    pageTitle="Azure'i rakendust Service PHP MySQL web appi loomine ja juurutamine FTP abil" 
    description="Õppeteema, mis näitab, kuidas luua PHP veebirakenduse, mis salvestab andmed MySQL-i ja kasutage FTP juurutamise Azure." 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>


#<a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-ftp"></a>Azure'i rakendust Service PHP MySQL web appi loomine ja juurutamine FTP abil

Selle õpetuse näete, kuidas luua PHP MySQL web appi ja juurutamise FTP abil. Selle õpetuse eeldab, et teil on [PHP][install-php], [MySQL-i][install-mysql], veebiserverisse ja FTP klient teie arvutisse installitud. Selles õpetuses juhiseid saate järgida operatsioonisüsteem, sh Windowsi, Maci ja Linux. Selle juhendi vormistamisel on teil PHP/MySQL web appi Azure töötab.
 
Saate:

* Kuidas luua web appi ja MySQL-andmebaasiga Azure'i portaalis. Kuna PHP on vaikimisi veebirakendustes, on vaja midagi erilist oma PHP koodi käivitada.
* Kuidas avaldada rakenduse Azure FTP abil.
 
Selle õpetuse järgides koostate lihtsa registreerimise veebirakenduse php. Rakendus on majutatud Web Appis. Allpool on täidetud taotluse kuvatõmmis.

![Azure'i PHP veebisait][running-app]

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Krediitkaardid nõutav kohustusi. 


##<a name="create-a-web-app-and-set-up-ftp-publishing"></a>Veebirakenduse loomine ja häälestamine FTP avaldamine

Web appi ja MySQL-i andmebaasi loomiseks tehke järgmist

1. [Azure'i portaali]sisse logida[management-portal].
2. Klõpsake vasakus ülanurgas **+ Uus** ikoonile Azure'i portaal.

    ![Azure'i uue veebisaidi loomine][new-website]

3. Tippige otsinguväljale **veebirakenduse + MySQL-i** ja klõpsake **veebirakenduse + MySQL-i**.

    ![Kohandatud uue veebisaidi loomiseks][custom-create]

4. Klõpsake nuppu **Loo**. Sisestage kordumatu rakenduse teenuse nime, ressursirühma ja uue teenusleping jaoks sobiv nimi.

    ![Rühma ressursinimi määramine][resource-group]


6. Sisestage uue andmebaasi, sh juriidiline terminite väärtused.

    ![Looge uus MySQL-andmebaasiga][new-mysql-db]
    
7. Veebirakenduse loomisel kuvatakse uus rakendus teenuse laba.


6. Klõpsake **sätete** > **juurutamise mandaat**. 

    ![Mandaadi seadmine juurutamine][set-deployment-credentials]

7. FTP avaldamise lubamiseks peate sisestama kasutajanime ja parooli. Salvestage mandaat ja kirjutage kasutajanime ja parooli luua.

    ![Avaldamise identimisteabe loomine][portal-ftp-username-password]

##<a name="build-and-test-your-app-locally"></a>Koostamine ja testige oma rakenduse kohalik

Registreerimise taotlus on lihtne PHP rakendus, mis võimaldab teil registreeruda sündmuse esitada oma nimi ja meiliaadress. Teavet eelmise registreerijad kuvatakse tabeli. Andmed on salvestatud MySQL-i andmebaasist. Rakendus koosneb kahest failid:

* **index.php**: kuvatakse vormi registreerimise ja tabel, mis sisaldab registreerija teavet.
* **CreateTable.php**: loob MySQL-i tabeli rakenduse. Selle faili kasutatakse ainult üks kord.

Koostamine ja käivitage rakendus kohalikult, järgige alltoodud juhiseid. Pange tähele, et need juhistes eeldatakse, PHP, MySQL-i, peate ja veebiserverisse häälestamine oma kohalikus arvutis ja millele teil on lubatud [KPN laiend MySQL-i][pdo-mysql].

1. Loomine MySQL-i andmebaasi, mida nimetatakse `registration`. Saate seda teha see käsk Käsuviip MySQL-i.

        mysql> create database registration;

2. Teie veebiserver juurkaust, looge kaust nimega `registration` ja kahe failide loomine see - ühte nimetatakse `createtable.php` ja üks nimetatakse `index.php`.

3. Avage soovitud `createtable.php` faili tekstiredaktoris või IDE ja lisada alljärgnev kood. Järgmine kood kasutatakse loomiseks on `registration_tbl` tabeli soovitud `registration` andmebaasi.

        <?php
        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
                        id INT NOT NULL AUTO_INCREMENT, 
                        PRIMARY KEY(id),
                        name VARCHAR(30),
                        email VARCHAR(30),
                        date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    > [AZURE.NOTE] 
    > Värskendage väärtused on vaja <code>$user</code> ja <code>$pwd</code> teie kohaliku MySQL-i kasutajanime ja parooliga.

4. Avage veebibrauser ja liikuge sirvides [http://localhost/registration/createtable.php][localhost-createtable]. See loob soovitud `registration_tbl` andmebaasi tabelisse.

5. Avage tekstiredaktoris või IDE **index.php** fail ja lisage lihtsa HTML-i ja CSS-i kood lehe (PHP kood lisatakse hiljem juhiseid).

        <html>
        <head>
        <Title>Registration Form</Title>
        <style type="text/css">
            body { background-color: #fff; border-top: solid 10px #000;
                color: #333; font-size: .85em; margin: 20; padding: 20;
                font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
            }
            h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
            h1 { font-size: 2em; }
            h2 { font-size: 1.75em; }
            h3 { font-size: 1.2em; }
            table { margin-top: 0.75em; }
            th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
            td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
        </style>
        </head>
        <body>
        <h1>Register here!</h1>
        <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
        <form method="post" action="index.php" enctype="multipart/form-data" >
              Name  <input type="text" name="name" id="name"/></br>
              Email <input type="text" name="email" id="email"/></br>
              <input type="submit" name="submit" value="Submit" />
        </form>
        <?php

        ?>
        </body>
        </html>

6. Sees PHP sildid, lisada PHP koodi andmebaasiga ühenduse loomisel.

        // DB connection info
        $host = "localhost";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    > [AZURE.NOTE]
    > Uuesti, peate värskendama väärtused <code>$user</code> ja <code>$pwd</code> teie kohaliku MySQL-i kasutajanime ja parooliga.

7. Pärast andmebaasi ühenduse koodi, lisada koodi lisamine andmebaasi andmed.

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. Lõpuks pärast ülaltoodud koodi lisada koodi andmete toomine andmebaasist.

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }

Nüüd saate sirvida [http://localhost/registration/index.php] [ localhost-index] testimiseks rakendus.

##<a name="get-mysql-and-ftp-connection-information"></a>MySQL-i ja FTP ühenduse kohta teabe saamine

Ühenduse MySQL-andmebaasiga, kus töötab veebirakendustes oma on vaja ühenduse teavet. MySQL-i ühenduse teabe saamiseks tehke järgmist.

1. Teenuse rakenduse web appi blade klõpsake ressursi rühma linki:

    ![Valige ressursirühm][select-resourcegroup]

1. Klõpsake oma ressursirühm andmebaasi:

    ![Valige andmebaas][select-database]

2. **Kokkuvõte andmebaasist nuppusätted** > **Atribuudid**.

    ![Valige Atribuudid][select-properties]
    
2. Märkige üles selle väärtused `Database`, `Host`, `User Id`, ja `Password`.

    ![Märkme atribuudid][note-properties]

3. Web appi, klõpsake alumises paremas nurgas lehe **allalaadimine avaldada profiili** linki:

    ![Laadi alla avaldamine profiil][download-publish-profile]

4. Avage soovitud `.publishsettings` faili XML-i redaktorit. 

3. Otsige üles soovitud `<publishProfile >` element `publishMethod="FTP"` , mis näeb välja umbes järgmine:

        <publishProfile publishMethod="FTP" publishUrl="ftp://[mysite].azurewebsites.net/site/wwwroot" ftpPassiveMode="True" userName="[username]" userPWD="[password]" destinationAppUrl="http://[name].antdf0.antares-test.windows-int.net" 
            ...
        </publishProfile>
    
Märkige üles soovitud `publishUrl`, `userName`, ja `userPWD` atribuute.

##<a name="publish-your-app"></a>Rakenduse avaldamine

Kui olete rakenduse kohalik testinud, saate selle avaldada oma veebirakenduse FTP abil. Siiski, peate esmalt andmebaasi ühenduse teavet rakenduse värskendamine. Andmebaasi ühenduse teavet hankisite varasemas versioonis ( **saada MySQL-i ja FTP ühenduseteavet** jaotise abil), **nii** järgmise teabe värskendamine on `createdatabase.php` ja `index.php` failide väärtused:

    // DB connection info
    $host = "value of Data Source";
    $user = "value of User Id";
    $pwd = "value of Password";
    $db = "value of Database";

Nüüd olete valmis oma rakenduse FTP avaldada.

1. Avage oma FTP klient valik.

2. *Hosti nimi osa* : Sisestage soovitud `publishUrl` atribuut märkida kohal sisse oma FTP klient.

3. Sisestage soovitud `userName` ja `userPWD` atribuute eespool märgitud muutmata sisse oma FTP klient.

4. Ühendust luua.

Pärast saab üles- ja allalaadimine failide vastavalt vajadusele. Veenduge, et teil on failide üleslaadimine juurkaust, mis on `/site/wwwroot`.

Pärast üleslaadimist nii `index.php` ja `createtable.php`, **http://[site name].azurewebsites.net/createtable.php** MySQL-i tabeli rakenduse loomine ja seejärel **http://[site name].azurewebsites.net/index.php** rakenduse kasutamise alustamiseks liikuge sirvides.
 
## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [PHP Arenduskeskus](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[install-mysql]: http://dev.mysql.com/doc/refman/5.6/en/installing.html
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[localhost-createtable]: http://localhost/tasklist/createtable.php
[localhost-index]: http://localhost/tasklist/index.php
[running-app]: ./media/web-sites-php-mysql-deploy-use-ftp/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-ftp/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-ftp/create_web_mysql.png
[website-details]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/website_details.jpg
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-ftp/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-ftp/select_webapp.png
[set-deployment-credentials]: ./media/web-sites-php-mysql-deploy-use-ftp/set_credentials.png
[portal-ftp-username-password]: ./media/web-sites-php-mysql-deploy-use-ftp/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-ftp/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-ftp/create_wa.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-ftp/select_database.png
[select-resourcegroup]: ./media/web-sites-php-mysql-deploy-use-ftp/select_resourcegroup.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-ftp/note-properties.png

[connection-string-info]: ./media/web-sites-php-web-site-mysql-deploy-use-ftp/connection_string_info.png
[management-portal]: https://portal.azure.com
[download-publish-profile]: ./media/web-sites-php-mysql-deploy-use-ftp/download_publish_profile_3.png
 
