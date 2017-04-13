<properties
    pageTitle="Kohandatud vahemällu Azure'i API haldus"
    description="Saate teada, kuidas üksusi vahemälu halduse Azure'i API võti"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Kohandatud vahemällu Azure'i API haldus
Azure'i API halduse teenuse on tugi [HTTP vastuse vahemällu](api-management-howto-cache.md) abil ressursi URL-i võti. Võti saab muuta taotluse päised, kasutades funktsiooni `vary-by` atribuudid. See on kasulik vahemällu kogu HTTP vastuste (ehk esindused), kuid mõnikord on kasulik lihtsalt vahemälu kujutis osa. Uue [vahemälu-otsing väärtus](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) ja [vahemälu-poe väärtus](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) poliitikate pakuvad võimalust talletada ja suvalise tekstilõigule poliitika määratlusi andmete toomiseks. Selle võimaluse ka lisab väärtust varem kasutusele [Saada päring](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) poliitika Kuna te saate nüüd vahemälu välisteenused vastuseid.

## <a name="architecture"></a>Arhitektuur  
API halduse teenuse kasutab ühiskasutusega kohta rentniku andmete vahemälu nii, et kui muudate kuni saad juurdepääsu sama mitu üksuste vahemälus talletatud andmed. Siiski mitme piirkonna juurutamise töötamisel on sõltumatu vahemälu iga piirkonna sees. Tänu sellele, on oluline, mitte käsitleda andmesalve, kus mõne infokillu ainuke vahemälu. Kui ei, ja hiljem otsustanud mitme piirkonna juurutamise ära, siis reisi kasutajate klientidele kaotada juurdepääsu vahemällu salvestatud andmeid.

## <a name="fragment-caching"></a>Fragmendid vahemällu talletamine
On teatud juhul, kui tagastatakse vastuste mingi osa andmeid, mis on kallis määramiseks ja veel jääb värske mõistlik aeg. Näiteks, kaaluge teenus, mis on loodud lennuliin, mis sisaldab teavet lendude broneerimine, flight olek jne. Kui kasutaja on liige lennuliinide punktide programmi, nad oleks nende praegune olek ja läbitud vahemaa kogunenud seotud teavet. Selle kasutaja seotud teave võib olla salvestatud muu süsteemi, kuid soovitatav kaasamine vastuste tagastatud flight olek ja broneerimine kohta. Seda saab teha ehk fragment vahemällu. Esmane esituse tagastatavad origin serverist luba mingi abil saate näidata, kui kasutaja seotud teavet lisada. 

Võtke arvesse järgmist JSON vastuse lisamine taustväärtus API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

Ja teisene ressursside veebisaidil `/userprofile/{userid}` , et välja näeb,

     { "username" : "Bob Smith", "Status" : "Gold" }

Määramiseks sobivat kasutajateabe kaasamiseks läheb vaja tuvastada, kes on lõppkasutaja. See on sõltuvad rakendamist. Näiteks kasutan selle `Subject` väide on `JWT` Turbeloa. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Me salvestada see `enduserid` väärtuse kontekstis muutuja hilisemaks kasutamiseks. Järgmiseks on kindlaks teha, kui eelmine taotlus on juba kasutajateabe tuua ja see vahemälus talletatud. Selle jaoks kasutame funktsiooni `cache-lookup-value` poliitika.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Kui vahemälu, mis vastab võtmeväärtuse, siis ei ole sisenemise `userprofile` kontekstis muutuja luuakse. Me edu otsingu abil kontrollida selle `choose` meilivoo poliitika määrata.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Kui soovitud `userprofile` kontekstis muutuja pole olemas, siis me olema, veendumaks, et hankida see HTTP-päring.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Kasutame funktsiooni `enduserid` ehitada kasutaja profiili ressursi URL-i. Kui vastus on me kehateksti vastuse välja tõmmata ja salvestada selle uuesti sisse kontekstis muutujana.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Meile teha HTTP-päring uuesti, kui sama kasutaja teeb teise taotluse vältimiseks saame talletada kasutaja profiili vahemälu.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Me hoida väärtus täpse sama Key, mida me algselt proovitakse tuua koos vahemälu. Kuidas peaks põhinema kestus, mida me valida väärtuse talletamiseks soovitud muutused ja kuidas salliv kasutajad on sageli juurde aegunud teavet. 

See on oluline mõistan, et toomine vahemälust on endiselt out protsess, võrgus taotluse ja potentsiaalselt saate ikkagi lisada kümneid millisekundit kutse. Eeliste tulevad määramisel kasutajaprofiili teabe võtab oluliselt pikem kui tõttu vajavad andmebaasi päringute või mitme tagasi otstega liitväärtuse teavet.

Protsessi viimane toiming on tagastatud vastus koos meie kasutajaprofiili teabe värskendamine.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Valisin lisada nii, et ka siis, kui väli asendaja ei esine, vastus oli veel kehtib JSON luba osana jutumärgid. See oli peamiselt silumine lihtsam teha.

Kui ühendate koos kõigi järgmiste toimingute tegemiseks, seetõttu on poliitika, mis näeb välja umbes järgmine ühe.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Seda vahemällu moodust kasutatakse peamiselt veebisaitide, kus HTML on koostatud serveripoolse nii, et see saab ühele lehele nimega renderdada. Kuid see võib olla ka mugav API-d, kus kliendid ei saa teha kliendi pool HTTP vahemällu või on soovitatav kliendi vastutuse sellele.

Fragmendid vahemällu sama liiki saab teha ka kirjutamata web serverites Redis vahemällu talletamise serveri kasutamine, aga API halduse teenuse kasutamine selle töö tegemiseks on kasulik, kui vahemällu talletatud fragmendid on pärit kui esmane vastused eri tagasi lõpetatakse.

## <a name="transparent-versioning"></a>Läbipaistva Versioonimine
See on tavaks mitme seotud erinevate versioonide API toetatavate korraga. See on võib-olla toetama viibite, nagu arendaja, test, tootmise jne, või see võib olla toetamiseks vanemate versioonide API aega API tarbijatele siirdamiseks uuemad versioonid. 

Ühe lähenemisviisi töötlemise selle asemel kliendi arendajate URL-ide kaudu muuta `/v1/customers` et `/v2/customers` on versiooni nad soovivad praegu kasutada ja kõnede korral taustväärtus URL-i API tarbija profiili andmete talletamiseks. Konkreetse kliendi kõne õige taustväärtus URL-i määramiseks on vaja päringu konfigureerimise andmeid. Selle konfiguratsiooni andmete vahemällu, saab me vähendada soovitud jõudluse kahju tehes selle otsingu.

Esimese asjana identifikaator, saab konfigureerida soovitud versiooni määratlemiseks. Selles näites valisin versiooni tootenumbri tellimuse seostada. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Seejärel teeme vahemälu otsing kuvamiseks, kui me juba on tuua soovitud klientrakenduse versiooni.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Siis saame vaadata kui me ei leia seda vahemälu.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Kui me ei, siis avage ja neid.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Kehateksti vastuse ekstraktida vastus.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Salvestage need tagasi vahemälu edaspidiseks kasutamiseks.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

Ja lõpuks värskendada, valige soovitud kliendi teenuse versioon tagaandmebaas URL-i.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Täielikult poliitika on järgmine.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


API tarbijatele läbipaistev kontrolli versiooni kirjutamata kaardi poole kliendid värskendage ja Juurutage uuesti kliendid ilma on elegantne lahenduse, mis käsitleb palju API versioonimise probleemid.

## <a name="tenant-isolation"></a>Rentniku eraldamise

Suurem, mitme rentniku kasutuselevõttu mõnes ettevõttes luua eraldi rühmad rentnikud kirjutamata riistvara erinevate juurutuste. See vähendab arvu klientidele, kes mõjutab riistvara probleem, klõpsake soovitud taustväärtus. See lubab ka järk-järgult kasutama uut tarkvara versioonid. Ideaalvariandis mitte see kirjutamata arhitektuur olema läbipaistev API tarbijatele. Seda saavutada sarnaselt läbipaistvaid versioone, kuna see põhineb sama võtet, käsitsemiseks kirjutamata URL-i kasutamise kohta API võti konfigureerimine olekus.  

Tagastamise eelistatud versiooni iga märkimise API, asemel naasete kood, mis on seotud rentniku jaoks määratud riistvara rühma. Identifikaatori saab koostada vastav taustväärtus URL-i.

## <a name="summary"></a>Kokkuvõte
Azure'i API halduse vahemälu kasutada mis tahes tüüpi andmete talletamiseks vabadusastmega võimaldab tõhusaks juurdepääsuks konfiguratsiooni andmed, mis võivad mõjutada sissetuleva taotluse töötlemise. Seda saab kasutada andmete fragmendid, mida saate tõsta vastuseid, on taustväärtus API tagastatud talletamiseks.

## <a name="next-steps"></a>Järgmised sammud
Palun andke tagasisidet Disqus teemas selle teema kui stsenaariumit muu poliitika on lubatud, või kui on stsenaariumid, mida soovite saavutada, kuid ei tunne on praegu võimalik teha.
