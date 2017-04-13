<properties
   pageTitle="Data Service turuplatsil loomise juhend | Microsoft Azure'i"
   description="Üksikasjalikud juhised, kuidas luua, kinnitamine ja juurutada andmeid teenuse osta Azure'i turuplatsilt."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Olemasoleva web teenuse vastendus OData alla CSDL kaudu

>[AZURE.IMPORTANT] **Sel ajal me ei ole enam kasutuselevõtt mis tahes uute andmete teenuse tootjad. Uue dataservices ei saada heaks kirjet.** Kui teil on SaaS ärirakendusi soovite avaldada AppSource leiate lisateavet [siin](https://appsource.microsoft.com/partners). Kui teil on mõni IaaS rakendusi või arendaja teenuse soovite avaldatava Azure'i turuplatsi võite leida rohkem teavet [siin](https://azure.microsoft.com/marketplace/programs/certified/).

Selles artiklis antakse ülevaade mõne alla CSDL vastendamiseks olemasoleva teenuse OData ühilduvad teenuse kasutamise kohta. Seda selgitatakse, kuidas luua vastenduse dokument (alla CSDL), et muudab Sisestuskeel taotluse kliendi teenuse kõne kaudu ja klienti ühilduvad OData kanali abil tagasi väljundi (andmed). Microsoft Azure'i turuplatsi seab teenuste lõppkasutajate OData-protokolli abil. Teenuseid, mis on esitatud sisu pakkujate (andmete omanikud) on esitatud vorme, nagu ülejäänud, SOAP, jne.

## <a name="what-is-a-csdl-and-its-structure"></a>Mis on saanud alla CSDL ja selle struktuuri?
On alla CSDL (kontseptuaalne skeem määratlus Language) on täpsustus viisi veebiteenuse või andmebaasi teenuse levinud XML-i paljusõnalisus Azure'i turuplatsi kirjeldamiseks.

Ülevaade lihtsate soovitud **voo Taotle:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Teistpidi on **andmevoo** .

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Joonis 1** diagrammide, kuidas mõnda muud klienti soovite saada andmete sisu pakkuja (teenus) Azure'i turuplatsi läbi vaadata.  Selle alla CSDL kasutab vastenduse/teisendus komponent päringu töötlemiseks ja andmete edasi sisu pakkuja API-taotlemise kliendi vahel.

*Joonis 1: Üksikasjalikud meilivoo taotlemise kliendi sisu pakkuja kaudu Azure'i turuplatsilt*

  ![Joonis](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Atom taust, Atomi Pub ja OData-protokolli, mille koostamine Azure'i turuplatsi laiendid, võtke läbivaatus: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Väljavõte ülevalt link:     *"Open Data protokolli (edaspidi OData) eesmärk on ülejäänud põhine protokoll ette CRUD-laadi toimingud (loomine, lugemine, värskendamine ja kustutamine) esitada ressursse, mis on esitatud data services. "data service" on lõpp avatud üks või mitu "saidikogumid" iga ajakirjetega null või rohkem"", mis hõlmavad andmete tipitud nimega ja väärtuse paarideks. OData avaldab Microsoft jaotises standardid OASIS (ettevõtte liigendatud teavet edutamine standardid) nii, et keegi, kes soovib saate koostada serverid, kliendid või ilma kasutus- või piiranguteta."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Kolm kriitilised osad, millel on määratletud selle alla CSDL on:

- . Selle teenuse pakkuja The Web aadress (URL) teenuse **lõpp-punkti**
- **Andmete parameetrite** edastatavad on sisendina teenusepakkuja parameetrid määratlused saadetakse sisu pakkuja teenuse allapoole andmetüüpi.
- **Skeemi** andmed tagastatakse paluda teenuse skeemi andmed on esitatud sisu pakkuja teenus, sh Container, saidikogumite ja tabelid, muutujate/veerud ja andmetüübid.

Järgmisel joonisel on esitatud ülevaade veevool, kus klient sisestab OData lause (kõne sisu pakkuja veebiteenuse) tulemuste/andmete toomine tagasi.

  ![Joonis](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Juhised:

1. Klient saadab kutse kaudu taotluse täieliku sisestatud parameetriga määratletud XML-i Azure'i turuplatsi
2. Alla CSDL saab kinnitada teenuse kõne.
    - Vormindatud teenuse kõne siis saadetakse sisu teenuse pakkujad Azure turuplatsiga
3. Funktsiooni Webservice aktiveeritakse ja eelvormid Http-Verbi toimingu (st saada) andmed tagastatakse Azure'i turuplatsi, kus on nõutud andmed (vajadusel) seab XML-vormingus kliendile abil soovitud alla CSDL määratletud vastendust.
4. Kliendi saadetakse andmed (vajadusel) XML või JSON-vormingus

## <a name="definitions"></a>Määratlused

### <a name="odata-atom-pub"></a>OData ATOMI pub

Määrake ATOMI pub, kus iga kirje tähistab tulemuseks ühe rea laiendamine. Kirje sisu osa on täiustatud sisaldama väärtusi rea – võtme ja väärtuse paarideks. Lisateavet leiate siit: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>Alla CSDL – kontseptuaalne skeem määratlus keel

Võimaldab määratleda funktsioonid (SPROCs) ja üksused, mis on avatud andmebaasi kaudu. Lisateavet leiate siit: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Klõpsake **muude versioonide** ripploendit ja valige versioon, kui te ei näe soovitud artikkel.

### <a name="edm---entry-data-model"></a>EDM - kirje andmemudel
- Ülevaade: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Eelvaade: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Andmetüübid: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Tagasi järgmisel näha üksikasjalikku vasakult paremale voog, kus kliendi siseneb OData lause (kõne sisu pakkuja veebiteenuse) saada tulemused/andmed:

  ![Joonis](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Alla CSDL põhialused

On alla CSDL (kontseptuaalne skeem määratlus Language) on täpsustus viisi veebiteenuse või andmebaasi teenuse levinud XML-i paljusõnalisus Azure'i turuplatsi kirjeldamiseks. Alla CSDL kirjeldatakse kriitilise ühikut mis **andmeallikast pärinevate andmete vastuvõtmise teeb võimalike Azure'i turuplatsi.** Siin kirjeldatakse peamisi andmeühikuga:

- Liidese teave, mis kirjeldab avalikult kättesaadavaks kõik funktsioonid (FunctionImport sõlme)
- Andmetüüpide kohta kõigi sõnumi requests(input) ja sõnumi responses(outputs) (EntityContainer/Olemikomplekt/EntityType sõlmed)
- Köitmine transport protokolli teavet kasutatud (päise sõlme)
- Aadressiteabe asukoha määratud teenuse (BaseURI atribuut)

Lühidalt, on alla CSDL tähistab platvormi ja keelest sõltumatu lepingu teenuse planeerimisüksused ja teenusepakkuja vahel. Selle alla CSDL kasutamisel saate kliendi otsige teenuse/andmebaasi veebiteenuse ja autonoomsest oma avalikult kättesaadavaks ülesannete.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Andmebaasi või kogumi on alla CSDL seotud
**Alla CSDL määratlus**

Alla CSDL on XML-i grammatika veebiteenuse kirjeldamiseks. Määratlus, ise on jagatud 4 põhi elemendid: Olemikomplekt, FunctionImport; Nimeruumi ja EntityType.

See võtmiseks hõlbustamiseks võimaldab mõista on alla CSDL seotud tabeli.

Pidage meeles;

  Alla CSDL tähistab platvormi ja keelest sõltumatu lepingu **teenuse planeerimisüksused** ja **teenusepakkuja**vahel. Alla CSDL kasutamisel saate **Kliendi** **teenuse/andmebaasi, veebiteenuse** leida ja ühtegi avalikult kättesaadavaks **funktsioonide.**

Andmete teenuse neli osad on alla CSDL saab vaadelda andmebaasi, tabeli, veeru ja poe toimingut.

Seotud need andmed teenuse eest järgmiselt:

- EntityContainer ~ = andmebaas
- Olemikomplekt ~ = tabel
- EntityType ~ = veerud
- FunctionImport ~ = salvestatud protseduur

**HTTP verbide lubatud**
- SAADA – tagastab väärtuste dB (tagastab kogumi)
- POSTITUSE – kasutatud andmeid ja valikuline tagastusväärtuste edasi db (Loo uus kirje kogumise, saatja id/URI)
- Kustuta – kustutab andmeid dB (kustutab kogumi)
- SELLELE – üheks on DB andmete värskendamine (asendamine kogumi või luua)

## <a name="metadatamapping-document"></a>Metaandmete/vastenduse dokumendi

Metaandmete/vastenduse dokumenti kasutatakse sisu pakkuja olemasoleva veebiteenuste nii, et see saab avatud OData web teenust Azure'i turuplatsi süsteem. See on alla CSDL ja rakendab mõne laiendamist alla CSDL vajadustele REST majutada veebiteenuste Azure'i turuplatsi kaudu. Laiendid on leitud [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) nimeruumi.

Näide selle alla CSDL järgmiselt: (käske Kopeeri ja kleebi on näide alla CSDL XML-i redaktoris ja Muuda teenust vastavaks.  Kleepige alla CSDL vastendamise DataService vahekaardil [Azure'i turuplatsi Avaldamisportaal](https://publish.windowsazure.com)teenust loomisel).

**Tingimused:** [Avaldamisportaal](https://publish.windowsazure.com) kasutajaliides (PPUI) tingimuste alla CSDL terminite kohta.
- "Pealkiri" on PPUI, mis on seotud MyWebOffer pakkumine
- Klõpsake soovitud PPUI MyCompany seotud **Publisheri kuvatav nimi** [Microsoft Arenduskeskus](http://dev.windows.com/registration?accountprogram=azure) UI
- Oma API, mis on seotud Web või Data Service (lepingu soovitud PPUI)

**Hierarhia:** 
 ettevõtte (pakkuja) kuulub Offer(s), mis on kava(d), milleks teenust, mis rida üles rakendusliidest.

### <a name="webservice-csdl-example"></a>WebService alla CSDL näide

Loob ühenduse teenus, mis on asetades lõpp web rakendus (nt C# rakendus)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Vaadata rohkem alla CSDL veebiteenuse näiteid artikli [näited vastendamise olemasoleva web teenuse OData CSDLs kaudu](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>DataService alla CSDL näide

Loob ühenduse teenus, mis on asetades andmebaasi tabelit või vaadet, nagu lõpp näide all kuvatakse kaks API-de jaoks andmebaasi vastavalt API alla CSDL (saate kasutada vaateid, mitte tabelid).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Vt ka
- Kui olete huvitatud Õppekeskuse ja teatud sõlmed ja nende parameetrid lugege seda artiklit [Andmete teenuse OData vastendamise sõlmed](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) määratlused ja selgitused, näited ja kasutada juhul kontekstis.
- Kui olete huvitatud läbivaatamise näited, lugege seda artiklit [Andmete teenuse OData vastendamise näited](marketplace-publishing-data-service-creation-odata-mapping-examples.md) näha proovi kood ja koodi süntaksit ja konteksti mõistmine.
- Naasmiseks ettenähtud tee avaldamine Azure'i turuplatsi Data Service, lugege seda artiklit [Andmete teenuse avaldamise juhend](marketplace-publishing-data-service-creation.md).
