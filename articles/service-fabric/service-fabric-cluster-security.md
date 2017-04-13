<properties
   pageTitle="Turvaline teenuse struktuuri kobar | Microsoft Azure'i"
   description="Kirjeldatakse turvalisuse võtted teenuse struktuuri kobar ja need stsenaariumid rakendada eri tehnoloogiaid."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Teenuse struktuuri kobar väärtpaberi stsenaariumid

Teenuse struktuuri kobar on ressurss, mille omanik te olete. Kogumite tuleks alati olema kinnitatud takistada volitamata kasutajate ühenduse klaster, eriti siis, kui ta on tootmise töökoormus see töötab. Kuigi see on võimalik luua ka turvamata kobar, tehes nii võimaldab mis tahes Anonüümne kasutaja loomiseks, kui see seab halduse lõpp-punktid avaliku Interneti-ühendus. 

Selles artiklis antakse ülevaade turvalisus stsenaariumid kogumite töötavate Azure'i või eraldi ja rakendada stsenaariumid erinevate tehnoloogiate jaoks. Kobar turvalisus stsenaariumid on:

- Sõlm sõlme turvalisus
- Kliendi sõlme turvalisus
- Rollipõhine juurdepääsu reguleerimine (RBAC)

## <a name="node-to-node-security"></a>Sõlm sõlme turvalisus
Tagab VMs või masinad klaster. See tagab, et ainult arvutid, millel on lubatud liituda klaster saavad osaleda majutusteenuse rakenduste ja teenuste klaster.

![Sõlm sõlme side skeem][Node-to-Node]

Kogumite töötavate töötab Windows Azure'i või autonoomse kogumite saate kasutada [Serdi turvalisus](https://msdn.microsoft.com/library/ff649801.aspx) või [Windowsi turvalisus](https://msdn.microsoft.com/library/ff649396.aspx) Windows Server masinad.
### <a name="node-to-node-certificate-security"></a>Sõlm sõlme serdi turvalisus
Teenuse struktuuri kasutab X.509 klaster loomisel kuvatakse sõlm-tüüpi osana määrake serveri serdid. Kiire ülevaade sellest, millised need serdid on ja kuidas saate hankida või luua need on esitatud käesoleva artikli lõpus.

Serdi turvalisus on konfigureeritud loomisel kobar Azure portaali, Azure'i ressursihaldur mallid või autonoomse JSON malli abil. Saate määrata esmane sert ja valikuline teisene sert, mida kasutatakse serdi rollovers. Saate määrata esmaseid ja teiseseid serdid peaks olema erinevas administraator klient ja kirjutuskaitstud kliendi sertide määrate [Kliendi sõlme turvalisus](#client-to-node-security).

Azure'i lugeda [häälestamine klaster on Azure ressursihaldur malli abil](service-fabric-cluster-creation-via-arm.md) saate teada, kuidas konfigureerida klaster serdi turvalisus.

Autonoomse lugege Windows Server [Secure autonoomse kobar X.509 sertide kasutamine Windowsis](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Sõlm sõlme windows turvalisus
Autonoomse lugege Windows Server [Secure autonoomse kobar Windows Windows turvalisuse abil](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Kliendi sõlme turvalisus
Autendib kliendid ja tagab suhtlemine klient ja üksikud sõlmed klaster. Seda tüüpi turvalisus autendib ja tagab kliendi suhtlus, mis tagab, et ainult autoriseeritud kasutajad pääsevad klaster ja klaster juurutatud rakendused. Kliendid on kordumatult tuvastada volituste Windowsi turbe-või serdi volituste.

![Kliendi sõlme side skeem][Client-to-Node]

Kogumite töötavate töötab Windows Azure'i või autonoomse kogumite saate kasutada [Serdi väärtpaberi](https://msdn.microsoft.com/library/ff649801.aspx) või [Windowsi turvalisus](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Kliendi sõlme serdi turvalisus
 Kliendi sõlme sert on konfigureeritud nii loomisel klaster ühte ressursihaldur malle või autonoomse JSON malli, määrates on administraator kliendi serti ja/või kasutaja kliendi serti Azure portaali kaudu.  Administraator klient ja kasutajale kliendi serdid teie määratud peaks olema erinevas määrate [sõlm sõlme turvalisus](#node-to-node-security)esmaseid ja teiseseid serdid.

Klientide ühenduse klaster serdiga administraator on täielik juurdepääs haldusvõimalusi.  Kirjutuskaitstud kasutaja kliendi serdiga klaster kliendid on ainult lugemisõigus haldusvõimalusi. Teisisõnu kasutavad selle rolli alused juurdepääsu reguleerimine (RBAC) selles artiklis allpool kirjeldatud need serdid.

Azure'i lugeda [häälestamine klaster on Azure ressursihaldur malli abil](service-fabric-cluster-creation-via-arm.md) saate teada, kuidas konfigureerida klaster serdi turvalisus.

Autonoomse lugege Windows Server [Secure autonoomse kobar X.509 sertide kasutamine Windowsis](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Kliendi sõlme Azure Active Directory (AAD) turvalisus Azure
Kogumite töötavate Azure saate tagada juurdepääs kasutades Azure Active Directory (AAD) halduse lõpp-punktid. Vajalikud AAD esemeid loomise kohta, kuidas neid asustamiseks kobar loomise ajal ja nende kogumite hiljem ühenduse loomise kohta lisateabe saamiseks vt [häälestamine klaster, mis Azure'i ressursihaldur malli abil](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Turvalisus soovitused
Azure'i kogumite, on soovitatav kasutada AAD turvalisus autentida kliendid ja serdid sõlm sõlme turvalisus.

Autonoomse Windows Server kogumite, on soovitatav Windows väärtpaberi kasutamine rühma hallatavate kontode (GMA) kui teil on Windows Server 2012 R2 ja Active Directory. Siiski teisiti kasutada Windows turvalisus Windowsi kontoga.

## <a name="role-based-access-control-rbac"></a>Rollipõhise juurdepääsu reguleerimine (RBAC)
Juurdepääsu reguleerimine võimaldab piirata juurdepääsu teatud kobar toimingute jaoks erinevate kasutajarühmade, klaster turvalisemaks kobar administraator. Kahe eri juurdepääsu juhtimine tüübid on toetatud arvutikobaras kliendid: administraatorirolli ja kasutajale rolli.

Administraatoritel on täielik juurdepääs halduse võimaluste (sh lugemis-ja kirjutamisõigusega võimaluste). Kasutajad, vaikimisi on ainult lugemisõigus halduse võimaluste (nt päringu võimalused) ja võimalus lahendada rakendused ja teenused.

Määrake administraatori ja kasutajale kliendi rollid kobar loomise ajal esitada eraldi identiteedid (sert AAD jne) iga. Vaikesätete Accessi juhtelementi ja kuidas muuta vaikesätete kohta leiate lisateavet teemast [rollipõhise juurdepääsu reguleerimine teenuse struktuuri klientidele](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X.509 serdid ja teenuse struktuuri
X.509 digitaalsertide on tavaliselt kasutatakse klientide ja serverite autentimiseks ja sõnumite krüptimine ja digitaalne allkirjastamine. Lisateavet need serdid, minge [töötamine serdid](http://msdn.microsoft.com/library/ms731899.aspx).

Oluline silmas pidada:

- Serdid kasutatud rühmades töötab tootmise töökoormus peaks õigesti konfigureeritud Windows Server serdi teenuse abil loodud või saadud on kinnitatud [Serdi sertimiskeskuse (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Ärge kasutage mõnda ajutised või test serdid valmistamisel tööriistadega nagu MakeCert.exe loodud.
- Te saate kasutada iseallkirjastatud serdi, kuid tuleks teha ainult jaoks testi kogumite ja pole.

### <a name="server-x509-certificates"></a>Serveri X.509 serdid

Serveri serdid on esmane ülesanne autentimisel server (sõlme) klientidele või autentimine (sõlme) server server (sõlme). Üks esialgse kontrolli kui kliendi või sõlm autendib sõlm on Kontrollige levinud nimi väljale teema. See levinud nime või üks soovitud serdid teema Alternatiivsed nimed peab olema lubatud levinud nimede loendi kohal.

Järgmises artiklis kirjeldatakse, kuidas luua serdid teema Alternatiivsed nimed (SAN): [Kuidas lisada turvaline LDAP serdi teema alternatiivne nimi](http://support.microsoft.com/kb/931351).

Väli Subject võib sisaldada mitut väärtust, iga eesliide on lähtestamine väärtuse tüübi määramiseks. Kõige sagedamini lähtestamine on "CN" levinud nimi; näiteks "CN = www.contoso.com". Samuti on võimalik teema välja tühjaks. Kui väli valikuline teema alternatiivne nimi on täidetud, peab sisaldama nii levinud nimi serti ja ühe kande teema alternatiivne nimi. Need on sisestatud DNS-i nimi väärtustena.

Serdi soovitud eesmärgid välja väärtus peaks sisaldama sobivat väärtust, näiteks "Serveri autentimine" või "Kliendi autentimine".

### <a name="client-x509-certificates"></a>Kliendi X.509 serdid

Kliendi serdid ei ole tavaliselt välja andnud kolmanda osapoole sertimiskeskus. Selle asemel isikliku salve kasutaja asukoha sisaldab tavaliselt kliendi sertide sinna juurkausta asutuse mõne otstarbe "Kliendi autentimine". Kui vastastikune autentimine on vaja, saate kliendi sertifikaati kasutada.

>[AZURE.NOTE] Teenuse struktuuri klaster kõigi toimingute jaoks on vaja serveri serdid. Kliendi serdid ei saa kasutada haldamiseks.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Järgmised sammud

Selles artiklis põhiteave kobar turvalisuse kohta. Järgmise, [klaster Azure ressursihaldur malli abil saate luua](service-fabric-cluster-creation-via-arm.md) või [Azure portaali](service-fabric-cluster-creation-via-portal.md)kaudu.

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
