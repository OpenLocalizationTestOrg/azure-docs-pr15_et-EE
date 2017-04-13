<properties
    pageTitle="Cloud Services KKK | Microsoft Azure'i"
    description="Korduma kippuvad küsimused pilveteenustega."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Pilveteenused KKK
Sellest artiklist leiate vastused korduma kippuvatele küsimustele Microsoft Azure'i pilveteenuste kohta. Leiate [Azure'i toe KKK](http://go.microsoft.com/fwlink/?LinkID=185083) üldteavet Azure hinnad ja tugi. Lugege ka [Cloud Services VM suurus lehe](cloud-services-sizes-specs.md) suurus teavet.

## <a name="certificates"></a>Serdid

### <a name="where-should-i-install-my-certificate"></a>Kus mu serdi tuleks installida?

- **Minu**  
Rakenduse serdi privaatvõti (\*pfx, \*.p12).

- **CA**  
Vahe sertide avage poes (poliitika ja Sub CAs).

- **ROOT**  
Root CA talletada, seega teie peamine root CA cert sihtkoha siin.

### <a name="i-cant-remove-expired-certificate"></a>Ma ei saa eemaldada aegunud sert

Azure'i takistab eemaldamise serdi kasutamise ajal. Peate kustutage juurutus, mis kasutab serdi või värskendage juurutamise eri või uuendatud sertifikaadiga.

### <a name="delete-an-expired-certificate"></a>Aegunud serdi kustutamine

Kui serti ei kasuta, saate [Eemalda-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShelli cmdlet-käsu eemaldamiseks sert.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Mul on aegunud serdid nimega Windows Azure'i Teenusehaldus laiendid

Need serdid luuakse alati laiendamist lisatakse pilveteenusesse, nagu kaugtöölaua laiendamine. Krüptimine ja dekrüptimine laiendamine privaatne konfiguratsiooni kasutatakse ainult need serdid. Pole oluline, kui need serdid aeguda. Aegumiskuupäeva pole sisse möllitud.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Mul on kustutatud serdid on endiselt kuvatud

Need on endiselt kuvatud tõenäoliselt kasutate, nt Visual Studio tööriista tõttu. Iga kord, kui loote ühenduse uuesti tööriista, mis kasutab sert, uuesti laaditakse see Azure.

### <a name="my-certificates-keep-disappearing"></a>Minu serdid jätta kaovad

Kui virtuaalse masina eksemplari ringlusse, kohaliku kõik muudatused lähevad kaotsi. Iga kord alustab roll virtuaalse masina installige [käivitus tööülesande](cloud-services-startup-tasks.md) abil.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ma ei leia oma halduse serdid portaalis

[Halduse serdid](..\azure-api-management-certs.md) on ainult Azure klassikaline portaalis. Praeguse Azure portaali ei kasuta halduse serdid. 

### <a name="how-can-i-disable-a-management-certificate"></a>Kuidas keelata serdi haldus?

[Halduse serdid](..\azure-api-management-certs.md) ei saa keelata. Saate kustutage need Azure'i klassikaline portaali kaudu, kui te ei soovi neid enam kasutada.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Kuidas luua SSL-serdi teatud IP-aadress?

Järgige [serdi õpetuse loomine](cloud-services-certs-create.md). Kasutage IP-aadressi DNS-i nimi.

## <a name="security"></a>Turvalisus

### <a name="disable-ssl-30"></a>SSL-i 3.0 keelamine

SSL 3.0 keelamise ja kasutada TLS Turve, mis on käivitus tööülesande loomine sellest ajaveebipostitusest: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Skaala pilveteenus

### <a name="i-cannot-scale-beyond-x-instances"></a>Lisaks X suurust ei saa eksemplari

Azure'i tellimuse on piiratud arv valdkond saate kasutada. Mastaapimine ei tööta, kui kasutasite kõigi südamike saadaval. Näiteks kui teil on limiit 100 valdkond, tähendab see teil oleks 100 A1 suurusega virtuaalse masina eksemplaris oma pilveteenuses, või 50 A2 suurusega virtuaalse masina eksemplarid.

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ma ei saa endale IP mitme-VIP pilveteenuses

Esmalt veenduge, et virtuaalse masina eksemplar, mida proovite reserveerida IP jaoks sisse lülitatud. Teiseks veenduge, et kasutate reserveeritud IP-d vaeva lavastus ja tootmise juurutuste. **Ära** muuta sätteid ajal juurutamise on üleminekut.

