<properties
    pageTitle="Installida RStudio R Server (eelvaade) Hdinsightiga | Microsoft Azure'i"
    description="Kuidas installida RStudio R serveris Hdinsightiga (eelvaade)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Installimine RStudio R serveris Hdinsightiga (eelvaade)

Mitme integreeritud keskkonnas (IDE) jaoks on saadaval R täna, sh Microsofti viimati teada [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), pere [RStudio](https://www.rstudio.com/products/rstudio-server/)või Walware's Eclipse-põhise [StatET](http://www.walware.de/goto/statet)server ja tööriistu. Kõige populaarsemate Linux seas on kasutada [RStudio Server](https://www.rstudio.com/products/rstudio-server/) , mis pakub Brauseripõhine IDE remote klientidele kasutamiseks.  Serva sõlme mõne Hdinsightiga Premium kobar RStudio serveri installimise annab täielik IDE kogemus täitmise R skripte ja R Server klaster ja võib olla märkimisväärselt veel tõhusamalt töötada kui R konsooli vaikimisi kasutada.

Sellest artiklist saate teada, kuidas ühenduse RStudio serveri (tasuta) versiooni installimiseks serva sõlme klaster kohandatud skripti abil. Kui eelistate äriliselt litsentsitud Pro RStudio serveri versiooni, peate järgima installimise juhiseid [RStudio](https://www.rstudio.com/products/rstudio/download-server/)serverist.

> [AZURE.NOTE] Juhised selle dokumendi nõua R serveris Hdinsightiga kobar ja ei tööta kui kasutate mõnda Hdinsightiga kobar õigesti kui R installiti [Installida R skripti toiming](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Eeltingimused

* Windows Azure Hdinsightiga kobar R serveris installitud. Juhised leiate teemast [Alustamine Hdinsightiga kogumite R serveris](hdinsight-hadoop-r-server-get-started.md).
* SSH kliendi. Unix ja Linux jaotuse või Mac OS X, on `ssh` käsk on esitatud operatsioonisüsteemi. Windowsi jaoks, soovitame [Cygwin](http://www.redhat.com/services/custom/cygwin/) [OpenSSH suvand](https://www.youtube.com/watch?v=CwYSvvGaiWU)või [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Kohandatud skript kasutamine klaster RStudio installimine

1. Leidke serva sõlme klaster. Hdinsightiga kobar R serveriga, on järgmine nimereeglistik pea sõlme ja serva sõlm.

    * Sõlm juht-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Serva sõlm-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH serva sõlme klaster ülaltoodud nimede muster abil sisse. 
 
    * Kui loote Linuxi klient, vt [ühenduse loomine Linux-põhine Hdinsightiga kobar](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Kui loote Windows kliendi, vt [ühenduse loomine Linux-põhine Hdinsightiga kobar, kasutades PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Kui olete loonud, muutuvad root kasutaja klaster. Kasutage SSH seansiga järgmine käsk.

        sudo su -

4. Kohandatud skript RStudio installimiseks laadige. Käsu.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Kohandatud skriptifail õigusi muuta, ja käivitage skript. Saate kasutada järgmisi käske.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Kui kasutasite loomisel kuvatakse Hdinsightiga kobar R serveriga on SSH parooli, saate selle sammu vahele jätta ja minna edasi. Kui kasutasite mõne SSH võti hoopis klaster loomiseks, peate määrama parooli oma SSH kasutaja jaoks. RStudio ühendamisel, tuleb see parool. Käivitage järgmised käsud. **Praeguse Kerberos**parooli küsimise korral vajutage lihtsalt **sisestusklahvi**.  Pange tähele, et peate asendama `USERNAME` SSH kasutajaga klaster Hdinsightiga jaoks.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Kui teie parool on edukalt, peaksite nägema järgmine teade.

        passwd: password updated successfully


    Väljuge SSH seanss.

7. Luua mõne SSH tunneliga klaster, vastendades `localhost:8787` Hdinsightiga klaster kliendi arvutisse. Peate looma ka SSH tunneliga enne avamist uus brauseriseanss.

    * Linux kliendi või Windowsi kliendi [Cygwin](http://www.redhat.com/services/custom/cygwin/) ja seejärel avage terminal seanss ja kasutage järgmist käsku.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Asendage **kasutajanimi** SSH kasutajaga klaster Hdinsightiga jaoks, ja **CLUSTERNAME** Hdinsightiga klaster nime saate SSH klahvi asemel parooli, lisades`-i id_rsa_key`     

    * Kui kasutate Windowsi kliendi Putty seejärel

        1.  Avage PuTTY ja sisestage oma ühenduse teave. Kui olete tuttav kitt, leiate teavet [Kasutada SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md) kohta, kuidas seda kasutada koos Hdinsightiga.
        2.  Dialoogiboksi vasakul jaotises **kategooria** laiendamiseks **ühendust**, laiendage **SSH**ja valige **tunnelid**.
        3.  Vormi **Valikud juhtimine SSH pordi suunamise** järgmist teavet:

            * **Lähteport** - porti klient, mida soovite edastada. Näiteks **8787**.
            * **Sihtkoha** - sihtkoht, mis tuleb vastendada kohaliku klientarvutis. Näiteks **localhost:8787**.

            ![Mõne SSH tunneliga loomine] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Mõne SSH tunneliga loomine")

        4. Lisamiseks klõpsake nuppu **Lisa** sätted ja valige SSH ühendus avamiseks **avage** .
        5. Kui palutakse, logige sisse server. See luua mõne SSH seanssi ja luba selle tunneliga.

8. Avage veebibrauser ja sisestage järgmine põhjal pordi jaoks soovitud tunneliga sisestatud URL.

        http://localhost:8787/ 

9. Teil palutakse sisestada SSH kasutajanimi ja parool ühenduse klaster. Kui kasutasite mõne SSH võti klaster loomisel, peate sisestama kohal juhises 5 loodud parool.

    ![Ühenduse loomine R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Mõne SSH tunneliga loomine")

10. Testimiseks RStudio installimine õnnestus, võite käivitada testi skripti, mis käivitab R klaster MapReduce ja säde töökohtade põhjal. Minge tagasi SSH konsooli ja sisestage järgmised käsud testi skripti käivitamiseks RStudio alla laadida.

    * Kui olete loonud Hadoopi kobar r, kasutage selle käsu.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Kui olete loonud säde kobar r, kasutage selle käsu.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. RStudio, kuvatakse testi skripti alla laadida. Topeltklõpsake faili avada, valige soovitud faili sisu ja seejärel klõpsake nuppu **Käivita**. Peaksite nägema väljundi **konsooli** paanil.
 
    ![Installi testimine] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Installi testimine")

Teine võimalus on tippige `source(testhdi.r)` või `source(testhdi_spark.r)` skripti käivitada.

## <a name="see-also"></a>Vt ka

- [Arvutage kontekstis Hdinsightiga kogumite R serveri suvandid](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure'i talletamise võimalused R Server Hdinsightiga premium](hdinsight-hadoop-r-server-storage.md)


 
