<properties
    pageTitle="Azure'i rakendust Service PHP MySQL web appi loomine ja juurutamine Git abil"
    description="Õppeteema, mis näitab, kuidas luua PHP veebirakenduse, mis salvestab andmed MySQL-i ja kasutada Git juurutamise Azure."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="mysql"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-php-mysql-web-app-in-azure-app-service-and-deploy-using-git"></a>Azure'i rakendust Service PHP MySQL web appi loomine ja juurutamine Git abil

Selles õpetuses näete, kuidas luua PHP MySQL web appi ja juurutamise selle [Rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=529714) abil Git. Kasutage [PHP][install-php], MySQL-i käsurea tööriist (osa [MySQL-i][install-mysql]), ja [Git] [ install-git] teie arvutisse installitud. Selles õpetuses juhiseid saate järgida operatsioonisüsteem, sh Windowsi, Maci ja Linux. Selle juhendi vormistamisel on teil PHP/MySQL web appi Azure töötab.

Saate:

* Kuidas luua web appi ja [Azure portaali]loomine MySQL-andmebaasiga[management-portal]. Kuna PHP on vaikimisi [rakenduse teenuse](http://go.microsoft.com/fwlink/?LinkId=529714) veebirakendustes, on vaja midagi erilist oma PHP koodi käivitada.
* Kuidas avaldada ja uuesti avaldamiseks rakenduse abil Git Azure.
* Kuidas helilooja laiendamiseks automatiseerimiseks helilooja toimingud veebisaidil iga `git push`.

Selle õpetuse järgides koostate lihtsa registreerimise veebirakenduse php. Rakendus on majutatud veebirakendustes. Allpool on täidetud taotluse kuvatõmmis.

![Azure'i PHP veebisait][running-app]

## <a name="set-up-the-development-environment"></a>Arengu keskkonna häälestamine

Selle õpetuse eeldab, et teil on [PHP][install-php], MySQL-i käsurea tööriist (osa [MySQL-i][install-mysql]), ja [Git] [ install-git] teie arvutisse installitud.

<a id="create-web-site-and-set-up-git"></a>
## <a name="create-a-web-app-and-set-up-git-publishing"></a>Veebirakenduse loomine ja häälestamine Git avaldamine

Web appi ja MySQL-i andmebaasi loomiseks tehke järgmist

1. [Azure'i portaali]sisse logida[management-portal].
2. Klõpsake ikooni **Uus** .

3. Klõpsake nuppu **Vaata kõik** **turuplatsi**kõrval. 

4. Klõpsake **Web + Mobile**ja siis **Web Appi + MySQL-i**. Klõpsake nuppu **Loo**.

4. Sisestage kehtiv nimi ressursirühma.

    ![Rühma ressursinimi määramine][resource-group]

5. Sisestage uue veebirakenduse jaoks väärtused.

    ![Veebirakenduse loomine][new-web-app]

6. Sisestage uue andmebaasi, sh juriidiline terminite väärtused.

    ![Looge uus MySQL-andmebaasiga][new-mysql-db]

7. Veebirakenduse loomisel kuvatakse uus web appi laba.

7. **Sätted** **Pidev**juurutuse, klõpsake käsku _konfigureerimine vajalik sätted_.

    ![Häälestage Git avaldamine][setup-publishing]

8. Valige **Kohalik Git hoidla** allikas.

    ![Git hoidla häälestamine][setup-repository]


9. Git avaldamise lubamiseks peate sisestama kasutajanime ja parooli. Kirjutage kasutajanime ja parooli luua. (Kui olete loonud Git hoidla enne selle toimingu jäetakse vahele.)

    ![Avaldamise identimisteabe loomine][credentials]


## <a name="get-remote-mysql-connection-information"></a>Remote MySQL-i ühenduse teavet

Ühenduse MySQL-andmebaasiga, kus töötab veebirakendustes oma on vaja ühenduse teavet. MySQL-i ühenduse teabe saamiseks tehke järgmist.

1. Klõpsake oma ressursirühm andmebaasi:

    ![Valige andmebaas][select-database]

2. Andmebaasist **sätted**, valige **Atribuudid**.

    ![Valige Atribuudid][select-properties]

2. Märkige üles selle väärtused `Database`, `Host`, `User Id`, ja `Password`.

    ![Märkme atribuudid][note-properties]

## <a name="build-and-test-your-app-locally"></a>Koostamine ja testige oma rakenduse kohalik

Nüüd, kui olete loonud web appi, saate töötada rakenduse kohalikult ja seejärel juurutada pärast testimine.

Registreerimise taotlus on lihtne PHP rakendus, mis võimaldab teil registreeruda sündmuse esitada oma nimi ja meiliaadress. Teavet eelmise registreerijad kuvatakse tabeli. Andmed on salvestatud MySQL-i andmebaasist. Rakendus koosneb ühe faili (kopeerige/kleepige kood saadaval allpool).

* **index.php**: kuvatakse vormi registreerimise ja tabel, mis sisaldab registreerija teavet.

Koostamine ja käivitage rakendus kohalikult, järgige alltoodud juhiseid. Pange tähele, et need juhistes on PHP ja MySQL-i käsurea tööriista (MySQL-i osa) oma kohalikus arvutis häälestamine ja et teil on lubatud [KPN laiend MySQL-i][pdo-mysql].

1. Ühenduse loomine MySQL-i kaugserverisse, kasutades väärtus `Data Source`, `User Id`, `Password`, ja `Database` , mille proovisite peidust välja tuua varasemas versioonis:

        mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. MySQL-i Käsuviip kuvatakse:

        mysql>

3. Kleebi järgmises `CREATE TABLE` käsku Loo selle `registration_tbl` oma andmebaasi tabelisse:

        CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. Looge oma kohaliku rakenduse juurkausta **index.php** fail.

5. Avage tekstiredaktoris või IDE **index.php** fail ja lisada järgmine kood ja tehke vajalikud muudatused, mis on tähistatud `//TODO:` kommentaarid.


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
            // DB connection info
            //TODO: Update the values for $host, $user, $pwd, and $db
            //using the values you retrieved earlier from the Azure Portal.
            $host = "value of Data Source";
            $user = "value of User Id";
            $pwd = "value of Password";
            $db = "value of Database";
            // Connect to database.
            try {
                $conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
                $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            }
            catch(Exception $e){
                die(var_dump($e));
            }
            // Insert registration info
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
            // Retrieve data
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
        ?>
        </body>
        </html>

4.  Minge kausta rakenduse terminal ja tippige järgmine käsk:

        php -S localhost:8000

Saate nüüd sirvides **http://localhost:8000 /** test taotluse.


## <a name="publish-your-app"></a>Rakenduse avaldamine

Kui olete rakenduse kohalik testinud, saate selle avaldada Web Appsi kasutamise Git. Saate lähtestada oma kohaliku Git hoidla ning rakenduse avaldamine.

> [AZURE.NOTE]
> Need on näidatud Azure portaali loomine web appi ja Sea lõpus üles Git avaldamise ülaltoodud samu juhiseid.

1. (Valikuline)  Kui olete unustanud või valesti paigutatud Git remote repostitory URL-i, liikuge web appi atribuutide Azure'i portaalis.

1. Avage GitBash (või terminal, kui Git on teie `PATH`), muuta kataloogide juurkaust rakenduse ja käivitage järgmine käsk:

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    Teil palutakse varem loodud parool.

    ![Algne tõuketeatised Azure Git kaudu][git-initial-push]

2. Liikuge sirvides **http://[site name].azurewebsites.net/index.php** (see teave talletatakse teie konto armatuurlaud) rakenduse kasutamise alustamiseks:

    ![Azure'i PHP veebisait][running-app]

Pärast teie rakendus on avaldatud, saate alustada muudatuste tegemist ja avaldada need Git abil.

## <a name="publish-changes-to-your-app"></a>Rakenduse muudatuste avaldamine

Rakenduse muudatuste avaldamine, toimige järgmiselt.

1. Rakenduse kohalik muudatusi teha.
2. Avage GitBash (või terminal, it Git on teie `PATH`), muuta kataloogide juurkaust rakenduse ja käivitage järgmine käsk:

        git add .
        git commit -m "comment describing changes"
        git push azure master

    Teil palutakse varem loodud parool.

    ![Saidi muudatuste lükkamine Azure Git kaudu][git-change-push]

3. Liikuge sirvides **http://[site name].azurewebsites.net/index.php** rakenduse ja teie tehtud muudatused kuvamiseks:

    ![Azure'i PHP veebisait][running-app]

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

<a name="composer"></a>
## <a name="enable-composer-automation-with-the-composer-extension"></a>Luba laiendiga helilooja helilooja automatiseerimine

Vaikimisi git juurutamise protsessi rakenduse teenus ei tee midagi koos composer.json, kui teil on üks PHP projektis. Saate lubada composer.json töötlemise ajal `git push` , võimaldades helilooja laiendamine.

1. Oma php web rakenduse blade [Azure portaali][management-portal], klõpsake **menüü Tööriistad** > **laiendid**.

    ![Helilooja laiend sätted][composer-extension-settings]

2. Klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **helilooja**.

    ![Helilooja laiend lisamine][composer-extension-add]
    
3. Klõpsake nuppu **OK** , et juriidiline nõustumine. Klõpsake nuppu **OK** uuesti lisamiseks laiendamine.

    Nüüd kuvatakse **installitud laiendid** tera helilooja laiendamine.  
    ![Helilooja laiendi kuvamine][composer-extension-view]
    
4. Nüüd teha `git add`, `git commit`, ja `git push` nagu eelmises jaotises. Nüüd näete helilooja installib sõltuvused määratletud composer.json.

    ![Edu helilooja laiend][composer-extension-success]

## <a name="next-steps"></a>Järgmised sammud

Lisateavet leiate teemast [PHP Arenduskeskus](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png
