<properties 
    pageTitle="Serdi lisamine Java CA pood | Microsoft Azure'i" 
    description="Saate teada, kuidas lisada Java CA sert (cacerts) poe Twilio teenuse või Azure'i teenus siini serdi sertimiskeskuse (CA) sert." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Java CA sertide salves serdi lisamine
Järgmised toimingud näitavad, kuidas lisada Java CA sert (cacerts) poe serdi sertimiskeskuse (CA) sert. Näiteks kasutada on nõutud Twilio teenuse CA serti. Teave hiljem teemas kirjeldatakse, kuidas CA serdi installimiseks Azure'i teenus siini jaoks. 

Keytool abil saate lisada CA serdi enne tihendamise oma JDK ja lisage see Azure'i projekti **approot** kausta või saab käivitada Azure käivitamise tööülesande, mis kasutab keytool serdi lisamiseks. Selles näites eeldab, et lisate CA serdi enne JDK, on zip. Ka näites kasutatakse CA konkreetse serdi, kuid mõne muu CA serdi importimine cacerts pood ja juhiseid oleks sarnast.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Serdi lisamiseks cacerts pood

1. Käivitage käsku Käsuviip, mis on seatud teie JDK **jdk\jre\lib\security** kausta, mis serdid on installitud järgmine:

    `keytool -list -keystore cacerts`

    Palutakse poe parool. Vaikimisi parool on **changeit**. (Kui soovite parooli muuta, vt <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>keytool dokumentatsioonis.) Selles näites eeldab, et MD5 serdi sõrmejälje 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 pole loendis ja soovite importida (selle konkreetse serdi vajalik Twilio API-teenus).
2. Hankige sert loendist [GeoTrust juursertide](http://www.geotrust.com/resources/root-certificates/)loetletud serdid. Paremklõpsake linki serdi järjenumbri 35:DE:F4:CF ja salvestage see kausta **jdk\jre\lib\security** . Selles näites huvides see salvestatud fail nimega **Equifax\_Secure\_serdi\_Authority.cer**.
3. Importida serdi kaudu järgmine käsk:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Kui teil palutakse usalda seda serti kui sert on MD5 sõrmejälje 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 vasta, tippides **y**.
4. Käivitage järgmine käsk CA serdi tagamiseks on edukalt imporditud.

    `keytool -list -keystore cacerts`

5. Funktsiooni JDK zip ja Azure projekti **approot** kausta lisada.

Keytool kohta leiate teavet teemast <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure'i juursertide

Azure'i teenuste (nt Azure'i teenus siini) kasutavate rakenduste vaja Baltimore CyberTrust Root serti usaldada. (Alates 15 aprillis 2013 Azure'i algas migreerimine versioonist topeltklõpsakeNr on globaalne Baltimore CyberTrust juurkausta. See migreerimise võttis lõpuleviimiseks mitu kuud).

Serdi võib juba olla installitud poes cacerts Baltimore nii meeles käivitada soovitud **keytool-loendi** käsk esmalt, et näha, kui see on juba olemas.

Kui teil on vaja lisada Baltimore CyberTrust juurkausta on järjenumber 02:00:00:b9 ja SHA1 sõrmejälje d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. See saab alla laadida <https://cacert.omniroot.com/bc2025.crt>, salvestatud kohaliku faili laiend **CER**ning seejärel imporditud **keytool** abil, nagu eespool näidatud.

## <a name="next-steps"></a>Järgmised sammud

Juursertide, mis kasutavad Azure'i kohta lisateabe saamiseks lugege teemat [Azure Root sert](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Java kohta leiate lisateavet teemast [Java Arenduskeskus](/develop/java/).
