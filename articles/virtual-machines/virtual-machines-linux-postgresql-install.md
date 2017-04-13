<properties
    pageTitle="PostgreSQL-i Linux VM häälestamine | Microsoft Azure'i"
    description="Siit saate teada, kuidas installida ja konfigureerida PostgreSQL-i Linux virtuaalse masina Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Installimine ja Azure PostgreSQL-i konfigureerimine

PostgreSQL-i on sarnane Oracle'i ja DB2 täpsemalt avatud allika andmebaasi. See sisaldab ettevõttele kasutusvalmis funktsioone nagu täielik HAPPE vastavuse, usaldusväärne selgituseks töötlemise ja mitme versiooni kokkulangevus kontrolli. Samuti toetab standardeid nagu ANSI SQL-i ja SQL-i/MED (sh võõrkeelsed andmete ümbriste Oracle, MySQL-i, MongoDB ja paljud teised). See on väga laiendatav koos üle 12 keelt, GIN ja Kokkuvõte registrite, ruumiline andmete tugi-ja mitme NoSQL nagu funktsioonide tugi JSON- või võti väärtus-põhiseid rakendusi.

Sellest artiklist saate teada, kuidas installida ja konfigureerida PostgreSQL-i Azure virtuaalne arvutisse, mis töötab Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>PostgreSQL-i installimine

> [AZURE.NOTE] Juba peab teil olema Azure virtuaalse masina, mis töötab Linux selles õpetuses lõpuleviimiseks. Loomine ja häälestamine Linux VM enne jätkamist, leiate [Azure'i Linux VM õpetuse](virtual-machines-linux-quick-create-cli.md).

Sel juhul kasutage pordi 1999 PostgreSQL-i pordi.  

Ühenduse Linux loodud kaudu PuTTY VM. Kui kasutate mõnda Azure Linux VM esimest korda, vaadake, [Kuidas kasutada SSH Linux Azure](virtual-machines-linux-mac-create-ssh-keys.md) saate teada, kuidas kasutada PuTTY ühenduse loomiseks Linux VM.

1. Käivitage järgmine käsk aktiveerimiseks root (haldus).

        # sudo su -

2. Mõned jaotuse on sõltuvused, mis tuleb teil installida enne installimist PostgreSQL-i. Otsi sellest loendist oma distributsiooni ja asjakohane käsk Käivita.

    - Punane rolli base Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian base Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Laadige alla PostgreSQL-i juurkausta ja siis lahti pakkimine.

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Ülal on näide. Leiate üksikasjalikumat allalaadimine aadressi [/pub/allikas/register](https://ftp.postgresql.org/pub/source/).

4. Koostamine alustamiseks käivitage järgmised käsud:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Kui soovite luua kõike, mida saab ehitada, sh dokumendid (HTML-i ja mees lehte) ja täiendavate moodulid (contrib), käivitage järgmine käsk:

        # gmake install-world

    Peaksite nägema järgmine kinnitusteade:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL-i konfigureerimine

1. (Valikuline) Lühendage PostgreSQL-i viide ei sisalda versiooninumber sümboolse lingi loomiseks tehke järgmist.

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Kataloogi andmebaasi loomiseks tehke järgmist.

        # mkdir -p /opt/pgsql_data

3. Kasutaja-root luua ja muuta selle kasutajaprofiili. Aktiveerige uus kasutaja (nn *postgres* siinses näites):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Turvalisuse põhjustel kasutab PostgreSQL-root kasutaja lähtestada, käivitage või andmebaas sulgeda.


4. Saate muuta *bash_profile* faili, sisestades alltoodud käsud. Faili *bash_profile* lõppu lisatakse need read:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Käivita *bash_profile* fail:

        $ source .bash_profile

6. Kinnitage oma installi abil järgmine käsk:

        $ which psql

    Kui teie Install õnnestub, kuvatakse järgmise vastuse.

        /opt/pgsql/bin/psql

7. Saate vaadata ka PostgreSQL-i versioon:

        $ psql -V

8. Lähtestab andmebaasi:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Peaksite nägema järgmine väljund:

![pilt](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL-i häälestamine

<!--    [postgres@ test ~]$ exit -->

Käivitage järgmine käsk:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Muutke kahte muutujat /etc/init.d/postgresql faili. Eesliide on seatud PostgreSQL-i Installitee: **/opt/pgsql**. PostgreSQL-i andmete salvestusruumi tee on seatud PGDATA: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![pilt](./media/virtual-machines-linux-postgresql-install/no2.png)

Veenduge, et see käivitatava faili muutmiseks tehke järgmist.

    # chmod +x /etc/init.d/postgresql

PostgreSQL-i käivitamiseks tehke järgmist.

    # /etc/init.d/postgresql start

Märkige ruut, kui PostgreSQL-i lõpp-punktile on:

    # netstat -tunlp|grep 1999

Peaksite nägema järgmine väljund:

![pilt](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Postgres andmebaasiga ühenduse loomiseks

Aktiveerige uuesti postgres kasutaja.

    # su - postgres

Postgres andmebaasi loomiseks tehke järgmist.

    $ createdb events

Äsja loodud sündmused andmebaasiga on ühenduse:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Loomine ja Postgres tabeli kustutamine

Nüüd, kui andmebaas on ühendatud, saate luua tabeleid ei.

Näiteks näide Postgres uue tabeli loomine abil järgmine käsk:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Nüüd olete seadistanud nelja veeruga tabel järgmiste veerunimede ja piirangutega:

1. Piiratud VARCHAR 20 märki käsk "nimi" veerg.
2. Veeru "toit" näitab toiduga üksust, mis toob iga inimese. VARCHAR piirab selle teksti alla 30 märki.
3. "Kinnitatud" veeru kirjete, kas isik on RSVP'd abil soovitud ühislõuna. Sobivad väärtused on "Y" ja "N".
4. "Kuupäev" veerus kuvatakse kui sündmus registreerunud. Postgres nõuab, et kuupäevad kirjutatakse yyyy-mm-dd.

Peaksite nägema järgmine kui tabel on edukalt loodud:

![pilt](./media/virtual-machines-linux-postgresql-install/no4.png)

Samuti saate tabeli struktuuri abil järgmine käsk:

![pilt](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Andmete lisamine tabelisse

Teabe esmalt rea lisamine:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Peaksite nägema selle väljundi:

![pilt](./media/virtual-machines-linux-postgresql-install/no6.png)

Saate lisada ka tabelis paari rohkem inimesi. Siin on mõned või saate luua oma.

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Kuva tabelid

Tabeli kuvamiseks kasutage järgmine käsk:

    select * from potluck;

Väljund on:

![pilt](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Tabeli andmete kustutamine

Järgmise käsu abil saate kustutada tabeli andmete:

    delete from potluck where name=’John’;

See kustutab kogu teabe "John" rida. Väljund on:

![pilt](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Tabeli andmete värskendamine

Järgmise käsu abil saate tabeli andmete värskendamine. Selle ühe Sandy kinnitanud, et ta on neil osaleda, nii, et saaksime muudab oma RSVP "N", "Y":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>PostgreSQL-i kohta lisateabe saamiseks
Nüüd, kui te rakenduses on Azure Linux VM PostgreSQL-i installimine lõpule viinud, saate nautida kasutamine Azure. PostgreSQL-i kohta lisateabe saamiseks külastage [PostgreSQL-i veebisaidil](http://www.postgresql.org/).
