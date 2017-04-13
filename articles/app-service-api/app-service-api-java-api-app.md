<properties
    pageTitle="Luua ja juurutada Java API rakenduse teenuses Azure rakendus"
    description="Saate teada, kuidas luua Java API rakenduse paketi ja selle juurutama Azure'i rakendust Service."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Luua ja juurutada Java API rakenduse teenuses Azure rakendus

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Selle õpetuse näitab, kuidas Java rakenduse loomine ja selle juurutama Azure'i rakenduse API rakendused [Git]abil. Mis tahes operatsioonisüsteem, mida suudab käivitada Java saate järgida selles õpetuses juhiseid. Selles õpetuses kood on ehitatud, kasutades [Maven]. [Jax-RS] saab luua rahulik teenuse ja on loodud põhjal [ärplema] metaandmete spetsifikatsioon [Ärplema redaktori]abil.

## <a name="prerequisites"></a>Eeltingimused

1. [Java arendaja Kit 8] \(või uuem versioon)
1. [Maven] arengu teie arvutisse installitud
1. [Git] arengu teie arvutisse installitud
1. [Microsoft] Azure makstud või [tasuta prooviversiooni] tellimuse
1. HTTP testi rakendust [postiljon]

## <a name="scaffold-the-api-using-swaggerio"></a>Tellingud API Swagger.IO abil

Swagger.io online toimetaja saate sisestada oma API struktuuri tähistav koodi ärplema JSON- või YAML. Kui olete API pindala, mis on loodud, saate eksportida erinevaid platvormid ja raamistiku koodi. Järgmise jaotise scaffolded koodi muudetakse sisaldavad fiktiivne funktsioone. 

See näide algab ärplema JSON asutusest kleepimist swagger.io redaktori, mida kasutatakse siis koodi kasutades JAX-RS juurdepääsuks on REST API lõpp-punkti loomiseks. Seejärel saate redigeerida scaffolded koodi fiktiivne andmeid, simuleerida REST API-ga ehitatud atop andmete püsimine süsteem.  

1. Teie lõikelauale kopeerida ärplema JSON järgmine kood:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Liikuge [võrgus ärplema redaktor]. Üks kord seal, klõpsake menüü **Fail -> Kleebi JSON** .

    ![Kleebi JSON menüükäsk][paste-json]

1. Kleepige soovitud kontaktide loendi API ärplema JSON varem kopeeritud. 

    ![Kleepimise ärplema JSON kood][pasted-swagger]

1. Saate vaadata dokumentatsiooni lehed ja API Kokkuvõte renderdamise redaktoris. 

    ![Vaate ärplema loodud dokumendid][view-swagger-generated-docs]

1. Valige **Loo Server-> JAX-r** menüü suvand tellingud serveripoolse koodi, saate hiljem redigeerida lisada fiktiivne rakendamine. 

    ![Luua koodi menüükäsk][generate-code-menu-item]

    Kui kood on loodud, peate olema sätestatud ZIP faili alla laadida. See fail sisaldab koodi, ärplema koodi generaator scaffolded ja kõik seotud koostada skriptide. Kogu teegi oma töökoha arengu kataloogiga pakkige see lahti. 

## <a name="edit-the-code-to-add-api-implementation"></a>Redigeeri koodi lisada API rakendamine

Selles jaotises saate asendada oma kohandatud koodi ärplema genereeritud koodi serveripoolne rakendamist. Uue koodi tulemuseks on üksuste loendit ArrayList, kontakti helistaja kliendile. 

1. Avage *Contact.java* mudeli faili, mis asub kaustas *src/gen/java/io/ärplema/mudel* [Visual Studio kood] või oma lemmik tekstiredaktoris abil. 

    ![Avatud kontakt mudeli faili][open-contact-model-file]

1. Lisage järgmised ehitaja **ärikontakti** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Avage *ContactsApiServiceImpl.java* teenuse rakendamine faili, mis asub kaustas *src/main/java/io/ärplema/api/impl* [Visual Studio kood] või oma lemmik tekstiredaktorit abil.

    ![Avatud kontakt teenuse kood faili][open-contact-service-code-file]

1. Kirjutada koodi faili selle uue koodi lisada fiktiivne rakendamist teenuse kood. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Avage käsuviip ja muuta kataloogi rakenduse juurkausta.

1. Käsu järgmised Maven luua koodi ja käivitage see Jetty rakenduse serveri kohalikult kasutamine. 

        mvn package jetty:run

1. Peaksite nägema kajastamiseks Jetty käivitas oma koodi Port 8080 käsk akna. 

    ![Avatud kontakt teenuse kood faili][run-jetty-war]

1. [Postiljon] abil saate teha taotluse "saada kõik kontaktid" API meetodi http://localhost:8080/api/kontaktid.

    ![Kõne kontaktid API][calling-contacts-api]

1. [Postiljon] abil saate esitada "get kindla kontakti" API meetodit asub http://localhost:8080, api, kontaktide ja 2.

    ![Kõne kontaktid API][calling-specific-contact-api]

1. Lõpuks koostada Java sõda (Veebiarhiiv) faili kasutades järgmised Maven käsku oma konsooli. 

        mvn package war:war

1. Kui sõda fail on ehitatud, on see paigutatud **sihtkaust** . Liikuge kausta **target** ja **ROOT.war**sõda faili ümber nimetada. (Veenduge, et suurtähestuse vastab selles vormingus).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Lõpuks täita järgmised käsud juurkaustast rakenduse **juurutamine** kasutada juurutamiseks sõda faili Azure kausta loomiseks. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Azure'i rakendust Service väljund avaldamine

Selles jaotises saate saate teada, kuidas luua uue API rakenduse Azure'i portaalis, majutusteenuse Java rakendusi API rakenduse ettevalmistamine ja juurutada vastloodud sõda faili Azure'i rakendust Service käivitamiseks API uus rakendus. 

1. Luua uue API rakenduse [Azure portaali], klõpsates **-> uus Web + mobiil -> API rakenduse** menüükäsk, rakenduse andmete sisestamise ja klõpsates siis käsku **Loo**.

    ![Uue API rakenduse loomine][create-api-app]

1. Kui rakenduse API on loodud, avage oma rakenduse **sätted** blade ja klõpsake menüükäsk **rakenduse sätted** . Valige saadaolevate suvandite seast Java uusimad valige uusima Tomcat **Web container** menüüst, ja klõpsake siis nuppu **Salvesta**.

    ![API rakenduse tera Java häälestamine][set-up-java]

1. Klõpsake menüükäsku **juurutamise identimisteabe** sätted ja sisestada kasutajanimi ja parool, mida soovite kasutada rakenduse API failide avaldamine. 

    ![Mandaadi seadmine juurutamine][deployment-credentials]

1. Klõpsake menüükäsku **juurutamise andmeallika** sätted. Üks kord, klõpsake nuppu **Vali allikas** , valige suvand **Kohaliku Git hoidla** ja seejärel klõpsake nuppu **OK**. See loob Azure, töötab koos rakenduse API-ga, millel on Git hoidla. Iga kord, kui koodi järgida *juhtslaidi* haru oma Git hoidla, avaldatakse reaalajas töötava API rakenduse eksemplari sisse oma kood. 

    ![Uue kohaliku Git hoidla häälestamine][select-git-repo]

1. Kopeerige uue Git hoidla URL lõikelauale. Salvesta see on hetkel. 

    ![Uue Git hoidla oma rakenduse häälestamine][copy-git-repo-url]

1. Git push sõda faili online hoidla. Selleks, avage varem loodud nii, et saate hõlpsasti endale koodi kuni töötavad rakenduse teenust hoidla **juurutamine** kausta. Üks kord olete konsooli aken ja seal, kus asub veebirakenduste kausta, kausta probleemi Git järgmised käsud käivitada protsess ja tule välja juurutamine. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Kui teil on väljastanud **tõuketeatised** kutse, küsitakse teilt, kas soovite juurutamise mandaadi varem loodud parool. Kui olete sisestanud mandaat, peaksite nägema oma portaali kuvamine, et värskendus on juurutatud.

1. Kui kasutate taas postiljon tabas äsja juurutatud API rakendus töötab Azure'i rakendust Service, kuvatakse käitumise vastab ja nüüd see on tagasi kontaktandmed oodatud viisil, ja abil lihtsa koodi muudatused on Swagger.io scaffolded Java kood. 

    ![Teie Java kontaktide REST API Azure Live'i kasutamine][postman-calling-azure-contacts]

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis teil oli võimalik hakata ärplema JSON-faili ja mõned scaffolded Java kood Swagger.io redaktor. Lihtsate muudatuste ja Git juurutamine sealt protsess tulemuseks on otstarbekas API rakenduse kirjutatud Java. Järgmise õpetus näitab kuidas [tarbimine API rakenduste JavaScripti klientidega, kasutades CORS][App Service API CORS]. Kuidas rakendada autentimise ja luba hiljem õppeteemade sarjas kuvamine

Selle valimi koostamiseks, leiate lisateavet [Mäluruumi SDK Java] püsima JSON plekid. Või saate kasutada [Dokumendi DB Java SDK] Azure'i dokumendi DB kontakti andmete salvestamiseks. 

Java Azure kasutamise kohta leiate lisateavet teemast [Java Arenduskeskus].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure'i portaal]: https://portal.azure.com/
[Dokumendi DB Java SDK]: ../documentdb/documentdb-java-application.md
[Tasuta prooviversioon]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java Arenduskeskus]: /develop/java/
[Java arendaja Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[JAX-r]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure'i]: https://azure.microsoft.com/
[Veebis ärplema redaktor]: http://editor.swagger.io/
[Postiljon]: https://www.getpostman.com/
[Salvestusruumi SDK Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Ärplema]: http://swagger.io/
[Ärplema redaktor]: http://editor.swagger.io/
[Visual Studio kood]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
