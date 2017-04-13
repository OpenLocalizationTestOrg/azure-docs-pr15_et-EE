<properties
    pageTitle="Arvuta mahukat Java rakendus VM | Microsoft Azure'i"
    description="Saate teada, kuidas luua Azure virtuaalse masina, mis Arvuta mahukat Java rakendus, mida saate jälgida mõni muu Java rakendus töötab."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Kuidas käivitada Arvuta mahukat tööülesande Java virtuaalse masina

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Azure, saate virtuaalse masina käsitlema Arvuta nõudvaid ülesandeid. Näiteks saate virtuaalse masina ülesannetega ja klientarvutite või mobiilirakenduste tulemusteni. Pärast selle artikli lugemist on teil mõista, kuidas luua virtuaalse masina, mis Arvuta mahukat Java rakendus, mida saate jälgida mõni muu Java rakendus töötab.

Selle õpetuse eeldab teada, kuidas luua Java konsooli rakendusi, saate importida teekide Java rakenduse ja saate luua Java archive (JAR). Microsoft Azure'i ei tea, eeldatakse, et.

Saate:

* Kuidas luua virtuaalse masina koos mõne Java arengu Kit (JDK) juba installitud.
* Kuidas oma virtuaalse masina kaugühenduse teel sisse logida.
* Kuidas luua teenuse siini nimeruumi.
* Kuidas luua Java rakendus, mis sooritavad Arvuta mahukat tööülesande.
* Kuidas luua Java rakendus, mis jälgib Arvuta mahukat toimingu edenemine.
* Kuidas Java rakendusi käivitada.
* Kuidas peatada Java rakendusi.

Selles õpetuses kasutatakse rändkaupmehe probleemi Arvuta mahukat ülesande jaoks. Järgmine on näide Arvuta mahukat tööülesande Java rakendus.

![Rändkaupmehe probleem Solveri][solver_output]

Järgmises näites Java rakenduse jälgimise Arvuta mahukat tööülesanne.

![Kliendi rändkaupmehe probleem][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Kui soovite luua virtuaalse masina

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2. Klõpsake nuppu **Uus**, nuppu **Arvuta**, klõpsake **virtuaalse masina**ja valige **Galeriist**.
3. Valige dialoogiboksis **Valige virtuaalse masina pilt** **JDK 7 Windows Server 2012**.
Pange tähele, et **JDK 6 Windows Server 2012** on saadaval juhuks, kui teil on pärand rakendusi, mis pole veel valmis käivitamiseks JDK 7.
4. Klõpsake nuppu **edasi**.
4. **Virtuaalne arvuti konfiguratsiooni** dialoogiboksis järgmist.
    1. Määrake virtuaalse masina nimi.
    2. Määrata virtuaalse masina jaoks kasutada.
    3. Sisestage väljale **Kasutajanimi** administraatori nimi. See nimi ja siis sisestage järgmine parool meeles pidada, on neid kasutada, kui te eemalt logige virtuaalse masina.
    4. Sisestage **Uus parool** väljale parool ja sisestage see uuesti väljale **Kinnita** . See on administraatori konto parool.
    5. Klõpsake nuppu **edasi**.
5. Järgmise **virtuaalse masina konfiguratsiooni** dialoogiboksis järgmist.
    1. **Pilveteenuses**, kasutage vaikeväärtust **Loo uus pilveteenuses**.
    2. **Pilveteenuse teenuse DNS-i nimi** väärtus peab olema kordumatu cloudapp.net üle. Vajadusel muutke seda väärtust nii, et selle Azure'i näitab, et see on kordumatu.
    2. Piirkond, osaleja rühma või virtuaalse võrgu määramiseks. Määrake selle õpetuse selleks, nt **Lääne USA**piirkonnas.
    2. **Salvestusruumi konto**, valige suvand **Kasuta automaatselt loodud salvestusruumi konto**.
    3. **Kättesaadavus**, valige **(pole)**.
    4. Klõpsake nuppu **edasi**.
5. Lõplik dialoogiboksi **virtuaalse masina konfigureerimine** :
    1. Nõustuge vaikimisi lõpp-punkti kirjeid.
    2. Klõpsake nuppu **valmis**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Virtuaalne arvuti eemalt sisse logida

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logima.
2. Klõpsake **virtuaalmasinates**.
3. Klõpsake selle nime virtuaalse masina, mida soovite sisse logida.
4. Klõpsake nuppu **Loo ühendus**.
5. Vastata viipasid vastavalt vajadusele ühenduse, virtuaalse masina. Kui küsitakse administraatori nimi ja parool, kasutage registreerimisel loodud virtuaalse masina väärtused.

Pange tähele, et Azure'i teenus siini funktsioonid Baltimore CyberTrust Root serdi installimiseks oma JRE **cacerts** poe osana. Selle serdi lisatakse automaatselt sisse selle Java Runtime keskkonnas (JRE) kasutavad selles õpetuses. Kui te ei ole seda serti JRE **cacerts** poes, vt [Java CA serdi talletamiseks serdi lisamine] [ add_ca_cert] (samuti teavet oma cacerts poes serdid vaatamise kohta) lisamise kohta lisateavet.

## <a name="how-to-create-a-service-bus-namespace"></a>Kuidas luua teenuse siini nimeruum

Azure'i teenus siini järjekorrad kasutamine alustamiseks tuleb esmalt luua teenuse nimeruumi. Teenuse nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks.

Teenuse nimeruumi loomiseks tehke järgmist.

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logima.
2.  Azure'i klassikaline portaali paanil alumisel Vasakpoolsel navigeerimispaanil nuppu **teenuse siini, juurdepääsu reguleerimine ja vahemälu**.
3.  Klõpsake **Teenuse siini** sõlme Azure klassikaline portaali ülemise vasakpoolse paani ja seejärel klõpsake nuppu **Uus** .  
    ![Teenuse siini sõlm kuvatõmmis][svc_bus_node]
4.  Dialoogiboksis **Loo uus teenus Namespace** Sisestage **Namespace**ja seejärel veenduge, et see on kordumatu, klõpsake nuppu **Saadavaloleku kontrollimine** .  
    ![Looge uus Namespace kuvatõmmis][create_namespace]
5.  Pärast seda, alustades seda kindlasti nimeruumi nimi on saadaval, valige riik või regioon, kus teie nimeruum peaks olema majutatud ja seejärel nuppu **Loo Namespace** .  

    Nimeruumi loodud kuvatakse seejärel Azure klassikaline portaalis ja võtab korraks aktiveerimine. Oodake, kuni olek on **aktiivne** enne jätkamist järgmise juhise juurde.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Vaikimisi halduse identimisteabe nimeruumi hankimine

Selleks, et haldamise toiminguid, näiteks luua uue nimeruumi järjekorda, peate hankima nimeruumi identimisteabe haldamine.

1.  Klõpsake vasakpoolsel navigeerimispaanil **Teenuse siini** sõlme saadaval nimeruumid loendi kuvamiseks.
    ![Saadaval nimeruumid kuvatõmmis][avail_namespaces]
2.  Valige äsja loodud loendis kuvatud nimeruumi.
    ![Namespace loendi kuvatõmmis][namespace_list]
3.  Parempoolsel paanil **Atribuudid** on loetletud atribuutide uue nimeruumi.
    ![Atribuutide paani kuvatõmmis][properties_pane]
4.  **Vaikimisi võti** on peidetud. Klõpsake nuppu **Turvalisus identimisteabe kuvamiseks Vaade** .
    ![Vaikimisi võti kuvatõmmis][default_key]
5.  Kirjutage **Vaikimisi väljaandja** ja **Vaikimisi võti** nagu te kasutate selle alltoodud teabe abil nimeruumi toimingute tegemiseks.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Java rakendus, mis sooritavad Arvuta mahukat tööülesande loomise kohta

1. Teie arvutisse arengu (mis ei pea olema loodud virtuaalse masina), [Azure'i SDK Java](https://azure.microsoft.com/develop/java/)alla laadida.
2. Java konsooli rakendus kasutamise näide koodi lõpus selles jaotises luua. Selles õpetuses kasutame **TSPSolver.java** Java faili nimi. Muuta selle **oma\_teenuse\_siini\_nimeruum**, **oma\_teenuse\_siini\_omanik**, ja **oma\_teenuse\_siini\_klahvi** kohatäidete kasutada teenust siini **nimeruumi**, **Vaikimisi väljaandja** ja **Klahvi vaikimisi** väärtusi,.
3. Pärast koodis, eksportimine rakenduse runnable Java archive (JAR) ja paketti nõutav teekide loodud purki. Selles õpetuses kasutame **TSPSolver.jar** loodud JAR nimeks.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Kuidas luua Java rakendus, mis jälgib Arvuta mahukat ülesande edenemine

1. Teie arvutisse arengu Loo lõpus käesoleva näite koodi kasutamise Java konsool rakendus. Selles õpetuses kasutame **TSPClient.java** Java faili nimi. Nagu varem, muuta selle **oma\_teenuse\_siini\_nimeruum**, **oma\_teenuse\_siini\_omanik**, ja **oma\_teenuse\_siini\_klahvi** kohatäidete kasutada teenust siini **nimeruumi**, **Vaikimisi väljaandja** ja **Klahvi vaikimisi** väärtusi,.
2. Rakenduse runnable JAR eksportida ja paketti nõutav teekide loodud purki. Selles õpetuses kasutame **TSPClient.jar** loodud JAR nimeks.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Kuidas käivitada Java rakendused
Käivitage rakendus Arvuta mahukat, esmalt loomiseks järjekorda, siis probleemi lahendamiseks reisil Saleseman, mis lisab praeguse parima protsessi teenuse siini järjekorda. Kuigi Arvuta mahukat taotlus on töötab (või hiljem), käivitada kliendi teenuse siini kuhjuda tulemusi kuvamiseks.

### <a name="to-run-the-compute-intensive-application"></a>Arvuta mahukat rakenduse käivitamiseks

1. Logige sisse oma virtuaalse masina.
2. Kui käivitate rakenduse kausta loomine Näiteks **c:\TSP**.
3. Kopeerige **TSPSolver.jar** **c:\TSP**,
4. Looge fail nimega **c:\TSP\cities.txt** järgmised sisuga.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Käsuviip, saate muuta kataloogide c:\TSP.
6. Tagada selle JRE prügikasti kausta tee keskkonnas muutujana.
7. Peate looma teenuse siini järjekorra enne tl Solveri permutatsioonide käivitamist. Käivitage järgmine käsk teenuse siini järjekorra loomiseks.

        java -jar TSPSolver.jar createqueue

8. Nüüd, kui kuhjuda on loodud, saate käivitada tl Solveri permutatsioonide. Näiteks käivitage järgmine käsk Käivita Solveri 8 linnade.

        java -jar TSPSolver.jar 8

 Kui te ei määra arvu, kestab 10 linna. Nagu Solveri praeguse lühima marsruudib, lisab need järjekorda.

> [AZURE.NOTE]
> Suurem arv, seda enam teie määratud Solveri käivituvad. Näiteks töötab 14 linnade võib kuluda mitu minutit ja töötab 15 linnade võib kuluda mitu tundi. Suurendab 16 või mitme linnade võib põhjustada päeva Runtime (lõpuks nädalat, kuude ja aastate arv). Selle põhjuseks kiire kasv hinnata Solveri nimega arv linnade suureneb permutatsioonide arvu.

### <a name="how-to-run-the-monitoring-client-application"></a>Kuidas käivitada jälgimisega seotud klientrakendus
1. Logige sisse oma arvutisse, kui käivitate klientrakendust. See ei pea olema samasse arvutisse **TSPSolver** rakendus, kuigi see võib olla.
2. Kui käivitate rakenduse kausta loomine Näiteks **c:\TSP**.
3. Kopeerige **TSPClient.jar** **c:\TSP**,
4. Tagada selle JRE prügikasti kausta tee keskkonnas muutujana.
5. Käsuviip, saate muuta kataloogide c:\TSP.
6. Käivitage järgmine käsk.

        java -jar TSPClient.jar

    Soovi korral saate määrata magama vahepeal kontrollimine järjekorda, mille käsurea argumendis minutite arv. Vaikimisi une perioodi kuhjuda kontrollimiseks on 3 minutit, kui ilma käsurea argumenti edastatakse **TSPClient**kasutatud. Kui soovite kasutada muu väärtuse une intervalli, näiteks üks minut, käivitage järgmine käsk.

        java -jar TSPClient.jar 1

    Kliendi käivituvad seni, kuni ta näeb järjekorda sõnum "Lõpuleviimine". Pange tähele, et kui käivitate kordumist Solveri käivitamata klient, peate käivitada kliendi täielikult tühjendada kuhjuda mitu korda. Teise võimalusena saate kustutada kuhjuda ja seejärel uuesti luua. Kuhjuda kustutamiseks järgmine käsk **TSPSolver** (mitte **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Solveri kestab kuni selle lõpetab kõik marsruudib uurida.

## <a name="how-to-stop-the-java-applications"></a>Kuidas peatada Java rakendused
Solveri nii klientrakendustes, võite vajutada **Klahvikombinatsiooni Ctrl + C** väljumiseks, kui soovite enne tavalist lõpetamist lõpetada.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
