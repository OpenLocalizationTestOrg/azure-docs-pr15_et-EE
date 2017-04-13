<properties
    pageTitle="Interneti-ühendusega koormusetasakaalustusega lahenduse IPv6-ga malli abil juurutada | Microsoft Azure'i"
    description="Kuidas juurutada IPv6 tugi Azure koormusetasakaalustusteenuse laadimine ja koormusetasakaalustusega VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6 azure laadimine koormusetasakaalustusteenuse, kahekordne virnas, avaliku ip, kohalikke ipv6, mobile, asjade"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Interneti-ühendusega laadi-koormusetasakaalustusteenuse lahenduse IPv6-ga malli abil juurutamine

> [AZURE.SELECTOR]
- [PowerShelli](./load-balancer-ipv6-internet-ps.md)
- [Azure'i CLI](./load-balancer-ipv6-internet-cli.md)
- [Mall](./load-balancer-ipv6-internet-template.md)

Mõne Azure laadi koormusetasakaalustusteenuse on kiht-4 (TCP, UDP) laadi koormusetasakaalustusteenuse. Laadi koormusetasakaalustusteenuse pakub kõrge-saadavus jagades liiklust terve teenuse pilveteenuste aknad või virtuaalmasinates laadi koormusetasakaalustusteenuse komplekti vahel. Azure'i laadi koormusetasakaalustusteenuse saate esitada ka nende teenuste mitme pordid, mitu IP-aadressi või mõlemad.

## <a name="example-deployment-scenario"></a>Näide juurutamise stsenaarium

Järgmine diagramm näitab tasakaalustamiseks lahenduse laadi kasutusele selles artiklis kirjeldatud näide malli abil.

![Laadi koormusetasakaalustusteenuse stsenaarium](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

Selle stsenaariumi loote Azure järgmistest allikatest:

- virtuaalse võrgu kasutajaliidese jaoks iga VM määratud nii IPv4 ja IPv6 aadressid
- mõne IPv4 ja IPv6 avaliku IP-aadress on Interneti-ühendusega laadi koormusetasakaalustusteenuse
- kaks laadimine tasakaalustavad reeglid avaliku VIP vastendamiseks privaatne lõpp-punktid
- mõne kättesaadavus seadmine kahe VMs sisaldava
- kahe virtuaalmasinates (VM)

## <a name="deploying-the-template-using-the-azure-portal"></a>Azure'i portaalis malli juurutamine

Selles artiklis viited Mall, mis on avaldatud [Azure'i Kiirjuhend](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) kuukalendreid. Saate alla laadida malli galeriist või käivitage juurutamise Azure otse galeriist. Selles artiklis eeldatakse, et teie kohalikku arvutisse allalaaditud mall.

1. Avage Azure portaali ja logige sisse kontoga, mis on õigused VMs ja Azure tellimuse võrgu ressursse. Ka juhul, kui te kasutate olemasolevate ressursid, peab konto ressursirühma ja salvestusruumi konto loomise õigus.

2. Valige "+ uus" menüü siis tippige otsinguväljale "Mall". Valige otsingu tulemuste seast "Malli juurutamine".

    ![LB ipv6-portaali step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Kuvatakse kõik tera, klõpsake "Malli juurutamine."

    ![LB ipv6-portaali step3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klõpsake "Loo".

    ![LB ipv6-portaali step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klõpsake nuppu "Redigeeri malli." Kustutage olemasolev sisu ja kopeerige/kleepige kogu sisu mallifail (peab sisaldama käivitamine ja lõpetamiseks {}) ja seejärel nuppu "Salvesta".

    > [AZURE.NOTE] Kui kasutate Microsoft Internet Exploreri kleepimisel saate kuvada dialoogiboks küsitakse luba juurdepääs Windowsi lõikelauale. Valige "Luba juurdepääs."

    ![LB ipv6-portaali step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klõpsake "Parameetreid redigeerida." Parameetrite labale määrata jaotises malli parameetrite väärtused on juhised selle kohta, siis parameetrite tera sulgemiseks klõpsake nuppu "Salvesta". Kohandatud juurutus labale tellimuse ressursi olemasolevasse rühma valige või luua. Kui loote ressursirühma, seejärel valige asukoht ressursirühma. Järgmiseks **õiguslikult**, klõpsake käsku **ostu** juriidiline argumentidega. Azure'i hakkab ressursse. Kulub minuti juurutamiseks ressursse.

    ![LB ipv6-portaali step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Järgmiste parameetrite kohta lisateabe saamiseks vt selle artikli jaotises [malli parameetrite ja muutujate](#template-parameters-and-variables) .

7. Selle malli abil loodud ressursside vaatamiseks klõpsake nuppu Sirvi, kerige loendis allapoole, kuni leiate "Ressursi rühmad", siis klõpsake seda.

    ![LB-ipv6-portaali-Step7 pakkumine](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Enne ressursi rühmad, klõpsake nime juhises 6 määratud ressursirühma. Näete ressursse, mis olid juurutatud loendit. Kui kõik läks hästi, peaks olema kirjas "Õnnestus" jaotises "Viimase juurutamine." Kui pole, veenduge, et kasutate konto on õigused vajalikud ressursid.

    ![LB ipv6-portaali step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Kui kohe pärast lõpetamist juhist 6 sirvida oma ressursside rühma, kuvatakse "Viimase juurutamine" "Juurutamine" oleku ajal ressursside kasutusele võtta.

9. Klõpsake loendis ressursid "myIPv6PublicIP". Näete, et see on IPv6-aadress IP-aadress ja, et oma DNS-i nimi on määratud väärtuse dnsNameforIPv6LbIP parameetri juhises 6. Selle ressursi on avaliku IPv6 aadress ja hosti nimi, mis on kättesaadavad Interneti-kliendid.

    ![LB ipv6-portaali step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Kinnitage Ühenduvus

Kui mall on edukalt juurutatud, saate kinnitada ühenduvust, tehes järgmist:

1. Azure portaali sisse logida ja ühenduse iga loodud malli juurutamise VMs. Kui olete juurutanud Windows Server VM käivitada ipconfig/kõik Käsuviip. Näete, et VMs on nii IPv4 ja IPv6 aadressid. Kui olete juurutanud Linux VMs, peate konfigureerima Linux OS saada dünaamiline IPv6 aadressid abil oma Linux jaotuse kuvatavad juhised.
2. IPv6 Interneti-ühendusega klientrakenduses kontaktiga ühenduse avaliku IPv6-aadress, laadi koormusetasakaalustusteenuse. Kontrollimaks, et laadi koormusetasakaalustusteenuse on kaks VMs vahel tasakaalustamiseks, võib installida veebiserverisse nagu Microsoft Internet Information Services (IIS) iga VMs. Vaikimisi veebilehe iga server võib sisaldada tekst "Server0" või "Server1" tuvastamiseks. Seejärel avage Interneti-brauseri IPv6 Interneti-ühendusega klientarvutis ja liikuge hostname laadi koormusetasakaalustusteenuse kinnitamiseks lõpuni IPv6 ühenduvuse iga VM dnsNameforIPv6LbIP parameetri jaoks määratud. Kui näete ainult ainult üks serverist veebileht, peate võib-olla brauseri vahemälu tühjendamine. Avage mitut privaatse sirvimise seanssi. Peaksite iga serverist vastust.
3. IPv4 Interneti-ühendusega klientrakenduses kontaktiga ühenduse avaliku IPv4 aadressi, laadi koormusetasakaalustusteenuse. Veenduge, et koormusetasakaalustusteenuse laadimine on kaks VMs tasakaalustamiseks laadi, võib testida, IIS-i, kasutades toiminguga 2 üksikasjalikku.
4. Iga VM algatada jaoks väljamineva ühenduse IPv6 või Internet IPv4 ühendatud seadmes. Mõlemal juhul allika IP näha sihtkoha seade on laadi koormusetasakaalustusteenuse avaliku IPv4 või IPv6 aadress.

>[AZURE.NOTE]
Azure'i võrgu ICMP nii IPv4 ja IPv6 on blokeeritud. Selle tulemusena ICMP tööriistad nagu ping alati nurjub. Ühenduvuse testimiseks kasutada TCP asemel, nt TCPing või PowerShelli Test-NetConnection cmdlet-käsk. Pange tähele, et joonisel IP-aadresside näiteid väärtused, mis võidakse kuvada. Kuna IPv6 aadressid on määratud dünaamiliselt, kuvatakse aadressid erinevad ja saate piirkonniti. Samuti on tavaline klõpsake Laadi koormusetasakaalustusteenuse alustada muu eesliite kui tagaandmebaas kogumi privaatne IPv6 aadressid avaliku IPv6-aadress.

## <a name="template-parameters-and-variables"></a>Malli parameetrid ja muutujad

Mõni Azure ressursihaldur Mall sisaldab mitut muutujat ja parameetrid, mida saate kohandada oma vajadustele. Muutujaid kasutatakse fikseeritud väärtuste, mida soovite muuta kasutaja jaoks. Parameetreid kasutatakse väärtuste, mida soovite esitada juurutamisel malli kasutaja. Selles artiklis kirjeldatud näide mall on konfigureeritud. Saate kohandada see keskkond, mis on teie vajadustele.

Näide kasutab sellest artiklist Mall sisaldab järgmisi muutujate ja parameetrid:

| Parameetri / muutuv | Märkmete |
|-----------|-------|
| adminUsername | Määrake administraatori konto virtuaalmasinates koos sisselogimiseks kasutatav nimi. |
| adminPassword | Määrake virtuaalmasinates koos sisselogimiseks kasutatava administraatori konto parool. |
| dnsNameforIPv4LbIP | Saate määrata DNS-i hosti nimi, mida soovite määrata avalik nimi, laadi koormusetasakaalustusteenuse. Sellise nimega lahendab laadi koormusetasakaalustusteenuse avaliku IPv4 aadress. Nimi peab olema väiketähed ja vastavad regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Saate määrata DNS-i hosti nimi, mida soovite määrata avalik nimi, laadi koormusetasakaalustusteenuse. See adressaadiväljal laadi koormusetasakaalustusteenuse avaliku IPv6-aadress. Nimi peab olema väiketähed ja vastavad regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. See võib olla sama nimi IPv4 aadress. Kui klient saadab DNS-i päringu selle nimi Azure tulemuseks A ja AAAA kirjete kui nimi on ühiskasutuses. |
| vmNamePrefix | Määrake VM nime eesliide. Malli lisab arvu (0, 1, jne) nime VMs loomisel. |
| nicNamePrefix | Määrake võrgu kasutajaliidese nime eesliide. Malli lisab arvu (0, 1, jne) nime võrgu liidesed loomisel. |
| storageAccountName | Sisestage olemasoleva salvestusruumi konto nimi või mall luuakse uus nimi. |
| availabilitySetName | Seejärel sisestage nimi koos VMs seadmine kättesaadavus |
| addressPrefix | Kasutatav meiliaadress vahemiku virtuaalse võrgu aadress eesliide |
| subnetName | Funktsiooni VNet jaoks loonud rakenduses alamvõrgu nimi |
| subnetPrefix | Kasutatav meiliaadress vahemiku alamvõrgu aadress eesliide |
| vnetName | Määrake VNet, mis kasutavad VMs nimi. |
| ipv4PrivateIPAddressType | Privaatne IP-aadress (staatiline või dünaamiline) eraldatud meetod |
| ipv6PrivateIPAddressType | Privaatne IP-aadress (dünaamiline) eraldatud meetod. IPv6 toetab ainult dünaamiline eraldatud. |
| numberOfInstances | Juurutatud malli laadimine tasakaalustatud eksemplaride arv |
| ipv4PublicIPAddressName | Määrake soovitud laadi koormusetasakaalustusteenuse avaliku IPv4-aadressi suhelda DNS-i nimi. |
| ipv4PublicIPAddressType | Avaliku IP-aadress (staatiline või dünaamiline) eraldatud meetod |
| Ipv6PublicIPAddressName | Saate määrata DNS-i nimi, mida soovite kasutada suhelda laadi koormusetasakaalustusteenuse avaliku IPv6-aadress. |
| ipv6PublicIPAddressType | Avaliku IP-aadress (dünaamiline) eraldatud meetod. IPv6 toetab ainult dünaamiline eraldatud. |
| lbName | Määrake nimi, laadi koormusetasakaalustusteenuse. See nimi kuvatakse portaalis või kasutatakse viidates selle CLI või PowerShelli käsu. |

Ülejäänud muutujate malli sisaldavad saadud väärtusi, mis määratakse, kui Azure'i loob ressursside. Need ei muutu.
