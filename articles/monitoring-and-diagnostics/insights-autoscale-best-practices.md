<properties
    pageTitle="Head tavad: Azure'i kuvari autoscaling. | Microsoft Azure'i"
    description="Siit saate teada, põhimõtted tõhusaks kasutamiseks autoscaling Azure jälgimine."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="best-practices-for-azure-monitor-autoscaling"></a>Azure'i kuvari autoscaling head tavad

Järgmistes jaotistes selles dokumendis aidata teil mõista paremini autoscale Azure. Pärast selle teabe, saate küll paremini tõhusaks kasutamiseks autoscale oma Azure'i infrastruktuuri.

## <a name="autoscale-concepts"></a>Autoscale mõisted

- Ressursi võib olla ainult *üks* autoscale säte
- Mõne sätte autoscale võib olla üks või mitu profiilid ja iga profiil võib olla üks või mitu autoscale reeglid.
- Mõne sätte autoscale skaala eksemplarid horisontaalselt, *mis on suurendades eksemplarid ja *klõpsake* vähendades eksemplaride arv* .
 Mõne sätte autoscale on maksimum-, miinimum- ja eksemplaride vaikeväärtus.
- Mõne autoscale töö loeb alati seostatud mõõdiku skaala järgi, kontrollimine, kui see on ületanud konfigureeritud läve skaala-või skaala sisse. Saate vaadata loendit, mõõdikute selle autoscale saate mastaapimiseks [Azure'i kuvari autoscaling levinud mõõdikute](insights-autoscale-common-metrics.md)juures.
- Kõik lävede arvutatakse eksemplari tasemel. Näiteks "skaala vähendamine 1 eksemplari, kui keskmine CPU > 80%, kui on arvu 2", kui keskmine CPU üle kõik eksemplarid, mis on suurem kui 80% skaala-out –.
- Saate alati e-posti teated nurjumise kohta. Täpsemalt omanik, kaasautor ja lugejad target ressursi vastu võtta. Saate alati ka *taastamine* e-posti, kui autoscale taastab tõrke ja käivitab normaalselt.
- Te saate valida puhul saada e-postiga ja webhooks eduka skaala toimingu teatis.

## <a name="autoscale-best-practices"></a>Autoscale head tavad

Järgmiste juhiste täitmisel kaudu saate autoscale.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Suurim ja väikseim väärtus on erinevad ja on nende vahel on piisavalt
Kui teil on säte, mis sisaldab vähemalt = 2 kuni = 2 ja praeguse eksemplari arv on 2, võivad tekkida skaala midagi. Säilita on piisavalt veerise maksimumi ja miinimumi astme loeb, mis ei kaasa arvatud vahel. Autoscale skaala alati nende piirväärtuse vahel.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Käsitsi skaleerimist lähtestatakse autoscale min ja max
Kui värskendate käsitsi eksemplari arv üles või alla suurim väärtus, skaala autoscale mootori automaatselt miinimum (kui all) või maksimum (kui kohal). Näiteks saate määrata vahemikus 3 – 6. Kui teil on ühe käivitatud eksemplari, autoscale mootori skaala 3 eksemplaridesse oma järgmise jooksma. See oleks skaala-samamoodi 8 eksemplari tagasi 6 oma järgmise Käivita.  Käsitsi skaleerimist on väga ajutine juhul, kui saate lähtestada autoscale reegleid kasutada ka.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Kasuta alati skaala-out ja skaala lisandmooduli reegli kombinatsioon, mis sooritavad mõne suurendamine ja vähendamine
Kui kasutate ainult üks osa kombinatsiooni, autoscale skaala – mis ühe välja või sisse, maksimum või miinimum, kuni on jõudnud.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Kui Autoscale haldamine ei Azure portaali ja Azure klassikaline portaali vaheldumisi aktiveerimine
Pilveteenustega ja rakenduse teenused (Web Apps), kasutage Azure portaali (portal.azure.com) luua ja hallata autoscale sätted. Virtuaalse masina skaala komplekti kasutamise PoSH, CLI või REST API luua ja hallata autoscale säte. Aktiveerib vaheldumisi Azure klassikaline portaali (manage.windowsazure.com) ja Azure portaali (portal.azure.com) kui autoscale konfiguratsioone haldamine. Azure'i klassikaline portaali ja selle aluseks oleva kirjutamata on piirangud. Liikumine Azure portaali haldamiseks autoscale graafilist liidest kasutades. Valikud on kasutada autoscale PowerShelli, CLI või REST API (Azure'i Resource Explorer) kaudu.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Valige sobiv statistiline oma diagnostika mõõdiku jaoks
Jaoks diagnostika mõõdikute, saate *Keskmine*, *miinimum*, *maksimum* ja *kokku* nimega mõõdiku skaala järgi. Kõige levinum statistiku *Keskmine*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Valige lävede hoolikalt argumendil kõigi jaoks
Soovitame hoolikalt erinevate piirmäärade skaala-out ja skaala-in põhjal olukordadest.

Me *ei soovita* autoscale sätteid nagu alla näidetega sama või väga sarnased läviväärtuse liugureid jaoks välja ja tingimused:

- Suurendamine 1 eksemplari loendamiseks, kui teema Count < = 600
- Vähendamine 1 eksemplari loendamiseks, kui teema Count > = 600

Vaatame näide, mis võib põhjustada käitumist, mis võib tunduda selgem. Cosider järgmist järjestust.

1. Oletame, on 2 juhtumeid alustada ja seejärel Teemad / kord keskmine arv kasvab 625.
2. Autoscale skaala välja lisamise 3 eksemplari.
3. Järgmiseks Oletame, et Keskmine jutulõnga count üle eksemplari langeb 575.
4. Enne skaala autoscale üritab prognoosimiseks lõplik olek on kui see mastaabitud sisse. Näiteks 575 x 3 (praeguse eksemplari arv) = 1725 / 2 (juhul, kui neid lõplik arv) = 862.5 teemad. See tähendab, et autoscale oleks kohe skaala-out uuesti isegi juhul, kui see ülespoole, kui keskmine jutulõnga arv ei muutu või isegi langeb ainult väike summa. Juhul, kui see ülespoole uuesti kogu protsessi oleks korrake, viib silmuses.
5. Olukord (nimetatakse "lehvitamine") vältimiseks autoscale skaala üldse. Selle asemel ignoreerib ja reevaluates tingimus uuesti järgmine kord, kui teenuse töö käivitab. See võib paljude inimeste segadusse, kuna autoscale ei näe, kui teema keskmine arv oli 575.

Hindamisel skaala-in on mõeldud "flappy" olukordade vältimiseks. Alles jätta meeles, kui valite skaala-out sama piirmäärad ja klõpsake seda käitumist.

Soovitame on piisavalt veerise vahel skaala-out ja lävede. Näiteks, võtke arvesse järgmist parem reegli ühendamine.

- Suurendamine 1 eksemplari loendamiseks, kui CPU % > = 80
- Vähendamine 1 eksemplari loendamiseks, kui CPU % < = 60

Sel juhul  

1. Oletame, et on 2 juhtumeid alustada.
2. Kui 80% Keskmine Protsessori mitmes eksemplaris, skaala autoscale välja lisamise kolmanda eksemplari.
3. Oletame, et aja jooksul kuulub CPU % 60.
4. Autoscale's skaala lisandmooduli reegli hinnangul lõplik olek oleks skaala-in. Näiteks 60 x 3 (praeguse eksemplari arv) = 180 / 2 (juhul, kui neid viimasel arv) = 90. Nii, et autoscale pole skaala sisse Kuna oleks skaala-out kohe uuesti. Selle asemel ignoreerib skaleerimist.
5. Järgmise aja autoscale kontrollib, CPU jätkuvalt 50. Hindab uuesti - 50 x 3 eksemplari = 150 / 2 eksemplari = 75, mis on alla 80, skaala välja nii, et see skaala edukalt 2 eksemplaridesse.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Kaalutlused skaleerimist läviväärtuse liugureid teisiti mõõdikute jaoks
 Teisiti mõõdikute, nt salvestusruumi või teenuse siini järjekorra pikkus meetermõõdustik, on keskmine arv sõnumite eurot praeguse eksemplaride arv. Hoolikalt valige valige selle mõõdiku lävi väärtus.

Vaatame illustreerimine näite mõistate parem käitumine tagamiseks.

- Suurendada eksemplaride arv 1, kui salvestusruumi järjekorda sõnumi count > = 50
- Vähendamiseks eksemplaride arv 1 kui salvestusruumi järjekorda sõnumi count < = 10

Võtke arvesse järgmist järjestust.

1. On 2 salvestusruumi järjekorda juhtumeid.
2. Sõnumite ikka ja salvestusruumi kuhjuda läbi koguarv loeb 50. Võib endale, selle autoscale peab algama skaala-out toiming. Kuid Pange tähele, et endiselt 50/2 = 25 sõnumite kohta eksemplari. Jah, ei esine skaala välja. Esimese skaala-välja juhtub, tuleks sõnumite koguarv salvestusruumi järjekorda 100.
3. Järgmiseks Oletame, et sõnumite koguarv jõuab 100.
4. 3 talletusmahu järjekorda eksemplar on lisatud skaala-out toimingu tõttu.  Järgmise toimingu skaala-out ei juhtu, kuni sõnumite koguarv järjekorda ulatub 150, kuna 150/3 = 50.
5. Nüüd väiksemaks sõnumite järjekorras. 3 eksemplari, kus esimene skaala toimingu juhtub, kui kõik järjekorrad kokku sõnumid lisada kuni 30, kuna 30/3 = 10 sõnumite eksemplari, mis on skaala läviväärtuse kohta.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Kaalutlused mastaapimist, kui autoscale seade on konfigureeritud mitu profiili

Autoscale-säte, saate valida vaikeprofiili, mis rakendatakse alati ilma mis tahes sõltuvus ajakava või kellaaeg, või võite korduva profiili või profiili tähtajaline kuupäeva ja kellaaja vahemikus.

Kui autoscale teenuse töötleb neid, alati kontrollib järgmises järjestuses:

1. Kindla kuupäeva profiil
2. Korduva profiil
3. Vaikimisi profiil ("alati")

Kui profiili tingimus on täidetud, ei kontrolli autoscale järgmise profiili tingimus selle all. Autoscale töötleb ainult üks profiil korraga. See tähendab, et kui soovite kaasata ka töötlemise tingimus madalama taseme profiili, peate lisama need reeglid ka praeguse profiili.

Vaatame seda kasutamise näide:

Alloleval pildil on autoscale säte koos vaikeprofiili minimaalne eksemplaride = 2 ja maksimaalse eksemplarid = 10. Selles näites reeglid on konfigureeritud skaala välja, kui sõnumi arvu järjekorras on suurem kui 10 ja skaala kohta, kui sõnumi järjekorda arvestus on väiksem kui 3. Nüüd saate ressursi skaala 2-10 eksemplari vahel.

Lisaks on korduva profiili esmaspäev seadmine. See on seatud minimaalne eksemplaride = 2 ja maksimaalse eksemplarid = 12. See tähendab, et esmaspäeval, see tingimus kontrollib esimest korda autoscale kui eksemplari arv on 2, see skaala uue lühima 3. Kui autoscale ei lahene, leida see profiili tingimus täidetud (Esmaspäev), ainult töötleb CPU-põhiste skaala välja/lisandmooduli reegleid seda profiili jaoks konfigureeritud. Sel ajal, seda ei kontrolli järjekorra pikkus. Aga kui ka soovite järjekorda pikkus tingimus kontrollima, saate peaks sisaldama vaikeprofiili need reeglid ka esmaspäev profiili.

Samamoodi kui autoscale lülitub tagasi vaikeprofiili, see esmalt külastatud, kui miinimum- ja tingimused on täidetud. Kui ajal eksemplaride arv on 12, see skaala on lubatud vaikeprofiili piirmäära 10.

![autoscale sätted](./media/insights-autoscale-best-practices/insights-autoscale-best-practices.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Kaalutlused skaleerimist mitme reeglid konfigureerimisel profiili
On juhtumeid, kus peate mitme reeglid loonud profiili. Järgmised autoscale reeglikomplekti kasutatakse teenuste kasutamine, kui mitu reeglid on seatud.

Autoscale töötab *skaala välja*, kui mõni reegel on täidetud.
*Skaala lisandmooduli*autoscale nõuda kõik reeglid tuleb täita.

Illustreerimiseks, Oletame, et on 4 autoscale järgmiselt:

- Kui CPU < 30%, skaala-1
- Kui mälu < 50%, skaala-1
- Kui CPU > 75%, skaala läbi 1
- Kui mälu > 75%, skaala läbi 1

Seejärel ilmneb järgi:

- Kui CPU on 76% ja mälu on 50%, saame skaala välja.
- Kui CPU 50% ja mälu on 76% me skaala välja.

Teisalt, kui CPU on 25% ja mälu on 51% autoscale ei **ole** skaala-in. Selleks, et skaala-, CPU peab olema 29% ja mälu 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Valige alati turvaliste vaikimisi eksemplari arv
Vaikimisi eksemplari arv on oluline autoscale skaala teenust selle arvestatakse kui mõõdikute pole saadaval. Seetõttu, valige vaikimisi eksemplari arv, mis on kaitstud oma töökoormus.

### <a name="configure-autoscale-notifications"></a>Autoscale teatiste konfigureerimine
Autoscale teavitab administraatorid ja osaliste ressursi e-posti, kui teil tekib mõni järgmistest tingimustest:

- autoscale teenus ei võta toimingu.
- Mõõdikute ei ole saadaval autoscale teenuse teeb skaala.
- Mõõdikud on saadaval (taastamine) skaala otsust uuesti teha.
Lisaks ülaltoodud tingimustel, saate konfigureerida e-posti või webhook teatised, et saada teada eduka skaala toimingute jaoks.
