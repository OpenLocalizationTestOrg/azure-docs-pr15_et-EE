<properties
    pageTitle="Azure'i AD Java alustamine | Microsoft Azure'i"
    description="Kuidas koostada Java käsurea rakendus, milles on kasutajate juurdepääs API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Azure AD API Java käsurea rakenduse kaudu

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD muudab lihtne ja arusaadav tellida oma veebirakenduse identiteetide haldus, andes ühe sisselogimise ja väljunud vaid paar rida koodi.  Java veebirakendustes, saate selle saavutamiseks Microsofti kogukonna juhitud ADAL4J rakendamise abil.

  Siin kasutame ADAL4J, et:
- Kasutaja kasutamine Azure AD identiteedipakkuja rakendusse sisse logida.
- Kasutaja kohta leiate teavet kuvada.
- Logige kasutaja rakenduse välja.

Selleks, peate:

1. Azure AD Rakenduse registreerimine
2. Häälestada rakenduse kasutada ADAL4J teeki.
3. Sisselogimine ja väljunud taotlusi probleemi Azure AD ADAL4J teegi abil.
4. Kasutaja andmeid välja printida.

Alustamiseks, [laadige rakendus luukere](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) või [allalaadimine on lõpule viidud valimi](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Peate mõne Azure AD rentniku, kuhu rakenduse registreerida.  Kui teil pole seda juba teinud, [saate teada, kuidas saada üks](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. rakenduse registreerimine Azure AD
Rakenduse kasutajate autentimiseks lubamiseks esmalt peate registreerida oma rentniku uus rakendus.

- Logige sisse teenusekomplekti Azure haldusportaali.
- Klõpsake vasakpoolsel navigeerimisribal, **Active Directory**.
- Valige rentniku, kuhu soovite rakenduse registreerida.
- Klõpsake vahekaarti **rakendused** ja klõpsake nuppu Lisa alla sahtlis.
- Järgige viipasid ja luua mõne uue **veebirakenduse ja/või WebAPI**.
    - Rakenduse **nimi** kirjeldada oma rakenduse lõppkasutajad
    - **Sisselogimise URL** on rakenduse base URL.  Funktsiooni skelett vaikimisi `http://localhost:8080/adal4jsample/`.
    - **Rakenduse ID URI** on rakenduse jaoks kordumatut tunnust.  Soovite on kasutada `https://<tenant-domain>/<app-name>`, nt`http://localhost:8080/adal4jsample/`
- Kui olete registreerimine, AAD määrata rakenduse kordumatu kliendi identifikaator.  Peate seda väärtust järgmistest jaotistest nii kopeerige see menüü konfigureerimine.

Kui portaalis oma rakenduse **Rakenduse salajane** rakenduse loomine ja selle kopeerimine alla.  Peate varsti.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2 häälestada rakenduse kasutama ADAL4J teek ja Maven kasutamise eeltingimused
Siin kuvatakse configure ADAL4J kasutama protokolli OpenID Connect autentimist.  ADAL4J kasutatakse probleemi sisselogimine ja väljunud taotlusi, hallata kasutaja seansi ja kasutajale, muu hulgas kohta teabe saamine.

-   Projekti juurkaustas, / avada või luua `pom.xml` ja otsige üles soovitud `// TODO: provide dependencies for Maven` ja asendage järgmist:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. java PublicClient faili loomine

Nagu eespool näidatud, me kasutame Graph API saada sisselogitud kasutaja andmeid. Oleks lihtne meile peaks loome üksiku faili tähistada **kasutaja** , et OO mustri Java saab kasutada nii faili tähistada **Directory objekti** lisamine.

1. Looge fail nimega `DirectoryObject.java` mida me kasutame peamine andmete kohta, mis tahes DirectoryObject (võite vabalt kasutada seda hiljem muude Graphi päringud, võite teha) talletamiseks. Te saate Lõika ja kleebi see alt:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Kompileerida ja valimi käivitamine

Muuta oma juurkaust tagasi välja ja käivitage järgmine käsk valimi koostamiseks saate lihtsalt panna koos kasutades `maven`. Seda kasutatakse funktsiooni `pom.xml` sa ei kirjutanud sõltuvused fail.

`$ mvn package`

Nüüd peaks teil on `adal4jsample.war` salvestage oma `/targets` directory. Võite juurutada, mis teie Tomcat ümbrises ja külastage URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
See on väga lihtne juurutada vähe uusima Tomcat serveritega. Liikuge lihtsalt `http://localhost:8080/manager/` ja järgige juhiseid üleslaadimise kohta oma "adal4jsample.war" fail. See on autodeploy jaoks õige lõpp-punkti.

##<a name="next-steps"></a>Järgmised sammud

Palju õnne! Nüüd on teil töötamise Java rakendus, mis on võimalus autentida turvaliselt kasutajad, kasutades OAuth 2.0 Web API-d ja saada põhiteavet kasutaja.  Kui te pole seda veel teinud, siis nüüd on aeg asustamiseks oma rentniku mõned kasutajatega.

Viide, lõpule viidud näidis (ilma väärtuste määramine) [soovitud ZIP siin on saadaval](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)või saate klooni seda GitHub kaudu:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

