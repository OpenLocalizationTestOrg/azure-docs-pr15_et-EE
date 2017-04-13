<properties 
    pageTitle="Web app tabelimälu (Node.js) | Microsoft Azure'i" 
    description="Õppeteema, mis põhineb Web Appi abil Express õpetuse Azure Storage teenused ja Azure mooduli lisamisega." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js veebirakenduse abil salvestusruum

## <a name="overview"></a>Ülevaade

Selles õpetuses on laiendamine rakenduse [Node.js veebirakenduse abil Express] õpetuses loodud kasutades Microsoft Azure'i kliendi teegid Node.js töötamiseks andmete halduse teenused. Kas laiendamine rakenduse veebipõhine ülesandeloendi rakendus, mida saab kasutada Azure loomiseks. Tööülesannete loendi võimaldab kasutajal tuua ülesanded, lisada uusi tööülesandeid ja ülesanded lõpetatuks märkida.

Tööülesande üksused salvestatakse Azure Storage. Azure'i salvestusruumi pakub struktureerimata andmed salvestusruumi, mis on tõrketaluvusega ja tugevalt saadaval. Azure'i salvestusruumi sisaldab mitut andmestruktuurid, kus saate talletada ja Accessi andmed ja te saate kasutada salvestusruumi teenuste kaudu API-de kaasatud Azure'i SDK Node.js või REST API-de kaudu. Lisateavet leiate teemast [salvestamine ja juurdepääs andmete Azure].

Selle õpetuse eeldab, et olete täitnud [Node.js veebirakenduse] ja [Node.js koos Express][Node.js veebirakenduse abil Express] õpetused.

Saate:

-   Kuidas töötada Jade malli mootor
-   Kuidas töötada Azure'i andmed halduse teenused

Allpool on täidetud taotluse kuvatõmmis.

![Internet Exploreris lõplikus veebileht](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Web.Config salvestusruumi mandaadi seadmine

Azure'i salvestusruumi, peate talletusruumi mandaadi edasi. Selleks saate kasutada web.config rakenduse sätted.
Nende sätete edastatakse nimega keskkonna muutujate sõlm, mis on seejärel lugege Azure'i SDK.

> [AZURE.NOTE] Salvestusruumi identimisteabe kasutatakse ainult rakendus on juurutatud Azure. Emulaator käivitamisel rakendusele salvestusruumi emulaator.

Järgmiste toimingute salvestusruumi konto identimisteave alla laadida ja lisada need web.config sätted:

1.  Kui see pole juba avatud, Alustuseks Azure PowerShelli menüü **Start** kaudu laiendamine **Kõik programmid, Azure**, paremklõpsake **Azure PowerShelli**ja valige **Käivita administraatorina**.

2.  Saate muuta kataloogide kaust, kus teie rakendus. Näiteks C:\\sõlm\\tasklist\\WebRole1.

3.  Azure'i PowerShelli aknas Sisestage salvestusruumi konto teabe toomiseks järgmine cmdlet-käsk:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    See toob salvestusruumi kontod ja konto teie majutatud teenusega seotud klahvid loendit.

    > [AZURE.NOTE] Kuna Azure'i SDK loob salvestusruumi konto teenuse juurutamisel, peaks juurutamise eelmise juhendid rakenduse juba olemas salvestusruumi konto.

4.  Avage **ServiceDefinition.csdef** fail, mis sisaldab keskkonna sätteid, mida kasutatakse rakenduse Azure juurutamisel.

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Lisada järgmised blokeering jaotises **keskkonna** element, asendades {salvestusruumi konto} ja {salvestusruumi KIIRKLAHV} konto nimi ja salvestusruumi konto, mida soovite kasutada juurutamiseks primaarvõti.

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Web.cloud.config faili sisu](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Salvestage fail ja sulgege notepad.

### <a name="install-additional-modules"></a>Täiendavad moodulid installimine

2. Järgmise käsu abil saate installida [azure], [sõlm uuid], [nconf] ja [asünkroonse] moodulid ka kohalik esmakordseks salvestamiseks kirje neid **package.json** faili:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Selle käsu väljund peaks kuvatama umbes järgmine:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Tabeli teenuse sõlm rakenduse kasutamine

Selles jaotises saate laiendab loodud **kiire** käsk **task.js** faili, mis sisaldab mudeli ülesannete lisamisega rakendused. Saate ka muuta olemasolevaid **app.js** ja luua uus **tasklist.js** faili, mis kasutab mudelit.

### <a name="create-the-model"></a>Mudeli loomine

1. **WebRole1** kataloogi, looge uus kaust nimega **mudelid**.

2. Kataloogis **mudelite** **task.js**nimega uue faili loomine. See fail sisaldab mudeli jaoks loodud rakenduse tööülesanded.

3. Lisada **task.js** faili alguses viitamiseks nõutav teekide järgmine kood:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Järgmiseks lisamist määratlemine ja tööülesande objekti kood. See objekt on tabel ühenduse.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Järgmiseks lisada järgmine kood määratleda täiendavad meetodite tööülesande objekti, mis võimaldavad kasutusviisid tabeli andmetega:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Salvestage ja sulgege fail **task.js** .

### <a name="create-the-controller"></a>Selle domeenikontrolleri loomine

1. **WebRole1/marsruudib** kataloogi, looge uus fail nimega **tasklist.js** ja avage see tekstiredaktoris.

2. Lisage järgmine kood **tasklist.js**. See laadib azure ja asünkroonse moodulid, mida kasutatakse **tasklist.js**. See määratleb ka **TaskList** funktsioon, mis on näiteks **tööülesande** objekti, mida me varem määratletud:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Jätkake, lisades **showTasks**, **addTask**ja **completeTasks**meetodite **tasklist.js** faili lisamine:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Salvestage fail **tasklist.js** .

### <a name="modify-appjs"></a>App.js muutmine

1. Kataloogis **WebRole1** tekstiredaktoris **app.js** faili avada. 

2. Lisage faili alguses azure mooduli laadimise ja tabeli nimi ja sektsiooni võtme seadmiseks järgmine tekst:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. App.js faili, liikuge kerides allapoole, kus on kuvatud järgmine rida:

        app.use('/', routes);
        app.use('/users', users);

    Asendage kood allpool ülaltoodud read. Eksemplari <strong>tööülesande</strong> salvestusruumi kontoga ühenduse lähtestada. See on <strong>TaskList</strong>, mis kasutab seda suhelda tabeli teenuse edastatakse:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Salvestage fail **app.js** .

### <a name="modify-the-index-view"></a>Registri vaate muutmine

1. Muuta kataloogide kataloogi **vaadete** ja avage **index.jade** fail tekstiredaktoris.

2. Asendage kood allpool **index.jade** faili sisu. See määratleb vaate kuvamiseks olemasoleva tööülesannete ning lisada uusi tööülesandeid ja olemasolevate märkimine lõpuleviiduks vorm.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Salvestage ja sulgege **index.jade** fail.

### <a name="modify-the-global-layout"></a>Globaalne paigutuse muutmine

**Vaadete** kataloogis **layout.jade** faili kasutatakse globaalmall **.jade** muid faile. Selles etapis tuleb saab seda kasutada [Alglaaduri Twitteri](https://github.com/twbs/bootstrap), mis on tööriistakomplekt, mis hõlbustab kena ilmega veebisaidi kujundamiseks muuta.

1. Laadige alla ja ekstraktimiseks failide jaoks [Twitteri alglaaduri](http://getbootstrap.com/). Kopeerige **bootstrap.min.css** fail soovitud **bootstrap\\dist\\CSS-i** kaust, kuhu on **avaliku\\laadilehte** tasklist rakenduse kataloogi.

2. **Vaadete** kaustast, avage **layout.jade** oma tekstiredaktoris ja asendage sisu järgmist:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Salvestage fail **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Emulaator rakendus töötab?

Käivitage rakendus emulaator järgmise käsu abil.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

Brauseris avatakse ja kuvatakse järgmine leht.

![Web leheküljed nimega Minu ülesandeloendi tabeliga, mis sisaldab tööülesandeid ja väljad uue ülesande lisamiseks.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Vormi abil üksuste lisamine või eemaldamine olemasolevate üksuste märkides need lõpetatuks.

## <a name="publishing-the-application-to-azure"></a>Azure'i rakenduse avaldamine


Windows PowerShelli aknas kõne järgmine cmdlet-majutatud teenust Azure Juurutage uuesti.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Asendage **myuniquename** selle rakenduse jaoks kordumatu nimi. Asendage **datacentername** Azure andmekeskuse, nt **Lääne USA**nime.

Kui juurutamise on lõpule jõudnud, peaksite nägema umbes järgmist vastuse:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Nagu varem, kuna määratud on **-käivitada** suvand brauseris avatakse ja kuvatakse teie rakendus Azure kui avaldamine on lõpule viidud.

![Lehe minu ülesandeloendi kuvamine brauseriaknas. URL-i näitab leht on nüüd majutatud Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Peatamine ja rakenduse kustutamine

Pärast rakenduse juurutamine, võite välja lülitada, nii et saate kulude vältimiseks või luua ja juurutada muude rakenduste tasuta prooviversiooni aja jooksul.

Azure'i arved veebis rolli aknad serveri aja tarbitud tunnis.
Kui teie taotlus on juurutatud, isegi kui linnanimede ei tööta ja on peatatud olekus tarbib serveri aeg.

Järgmised sammud näitavad, kuidas peatada ja kustutada teie rakendus.

1.  Klõpsake aknas Windows PowerShelli abil järgmine cmdlet-käsk eelmises jaotises loodud teenuse juurutamise lõpetamiseks tehke järgmist.

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Teenuse peatamine võib kuluda mitu minutit. Kui teenus on peatatud, kuvatakse teade, et see on lõpetanud.

3.  Teenuse kustutamiseks helistada järgmine cmdlet-käsk:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Küsimise korral sisestage **Y** teenuse kustutamiseks.

    Teenuse kustutamine võib kuluda mitu minutit. Kui teenus on kustutatud kuvatakse teade, mis näitab, et teenus on kustutatud.

  [Node.js veebirakenduse abil Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Salvestamise ja Azure'i andmetele juurde pääseda]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js veebirakenduse]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
