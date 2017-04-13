<properties
    pageTitle="Uute kasutajate lisamine Azure Active Directory | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse uute kasutajate lisamiseks ja Azure Active Directory kasutajateabe muutmise."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Lisada uusi kasutajaid või Microsofti kontodega kasutajad Azure Active Directory

Kataloogi asustamiseks kasutajat lisada. Selles artiklis selgitatakse, kuidas lisada uusi kasutajaid ettevõtte ja kuidas kasutajad, kellel on Microsofti kontot lisada. Muud kataloogid Azure Active Directory kasutajate lisamise või partneri ettevõtetest kasutajate lisamise kohta lisateabe saamiseks vaadake teemat [kasutajate lisamine muude kataloogide või partnerid Azure Active Directory](active-directory-create-users-external.md). Lisatud kasutajate pole administraatoriõigused vaikimisi, kuid saate määrata rollid neile igal ajal.

## <a name="add-a-user"></a>Kasutaja lisamine

1. Logige sisse kontoga, millel on kataloogi üldadministraator [Azure klassikaline portaali](https://manage.windowsazure.com) .
2. Valige **Active Directory**ja seejärel valige oma ettevõtte kataloogi nimi.
3. Valige vahekaart **Kasutajad** ja seejärel käsuriba, valige **Lisa kasutaja**.
4. Valige lehel **meile selle kasutaja kohta** jaotises **kasutaja tüüp**, kas

    - **Ettevõttes uue kasutaja** – lisab uue kasutajakonto kataloogis.
    - **Kasutaja, kellel on Microsofti konto** – lisab olemasolevate Microsoft tavakasutajakonto kataloogi (nt Outlooki konto)

5. Sõltuvalt **kasutaja tüüp**, sisestage kasutaja nimi (uus kasutaja) või e-posti aadress (jaoks kasutaja, kellel on Microsofti konto).
6. Sisestage kasutaja **profiili** lehel ees- ja perekonnanimi, kasutajasõbralik nimi ja kasutajaroll, **rollide** loendist. Kasutaja ja administraatori rollid kohta leiate lisateavet teemast [administraatorirollide Azure AD](active-directory-assign-admin-roles.md). Määrata, kas soovite **Lubada Mitmikautentimise** kasutaja.
7. Valige lehel **ajutist parooli** **loomine**.

> [AZURE.IMPORTANT] Kui teie asutus kasutab rohkem kui üks domeen, peaks teadma järgmist kasutajakonto lisamisel:
>
> - Lisada kasutajakontosid sama kasutaja turvasubjektinimi (UPN) domeenides, **esimese** lisada, näiteks geoffgrisso@contoso.onmicrosoft.com, **, millele järgneb** geoffgrisso@contoso.com.
> - **Ära** lisa geoffgrisso@contoso.com enne lisamist geoffgrisso@contoso.onmicrosoft.com. Selles järjestuses on oluline ja võib olla tülikas tagasi võtta.

## <a name="change-user-information"></a>Kasutaja teabe muutmine

Saate muuta kasutaja atribuut välja arvatud objekti ID-ga.

1. Avage kataloogi.
2. Valige vahekaart **Kasutajad** ja valige kuvatav nimi kasutaja, mida soovite muuta.
3. Tehke soovitud muudatused ja klõpsake siis nuppu **Salvesta**.

Kui kasutaja muudetava on sünkroonitud kohapealse Active Directory teenust, ei saa muuta selle protseduuri kasutaja andmed. Saate muuta kasutaja kohapealse Active Directory Haldusriistad.

## <a name="guest-user-management-and-limitations"></a>Külalisena kasutajate haldamine ja -piirangud

Külaliskontod on kasutajate muude kataloogide kutsutud kataloogi juurdepääsuks SharePointi dokumentide, rakenduste või muud Azure ressursid. Külalisena konto kataloogis on selle aluseks oleva UserType atribuudi on seatud "Külaline". Tavaline kasutajad (täpsemalt kataloogi liikmed) on UserType atribuut "Liige."

Külastajad saavad piiratud õiguste kogumi kataloogis. Nende õiguste piiramiseks võimalus külalistele leida teavet teiste kasutajate kataloogis. Siiski külalisi saate endiselt suhelda kasutajad ja rühmad, mis on seotud ressursid, millega ta parajasti töötab. Külalisena kasutajad teha järgmist.

- Vaadata teiste kasutajate ja rühmade on määratud Azure tellimusega seotud
- Vaadake, kuhu nad kuuluvad rühma liikmete
- Vaadata teiste kasutajate kataloogis, kui nad ei tea kasutaja täielik meiliaadress
- Piiratud hulk nad näevad üles--piiratud kuvatav nimi, meiliaadress, kasutaja turvasubjektinimi (UPN) ja pisipiltide foto kasutajate atribuute vaadata
- Kinnitatud domeenid kataloogis loendi hankimine
- Rakenduste, andes neile liikmete sama juurdepääsu kataloogis nõusoleku

## <a name="set-guest-user-access-policies"></a>Seadmine Külastajate kasutaja juurdepääsu reeglid

**Konfigureerimine** vahekaarti kataloog sisaldab suvandeid, et määrata Külastajate kasutajate jaoks. Need suvandid saate muuta ainult Azure klassikaline portaali directory üldadministraator. Praegu ei ole PowerShelli või API meetodit.

Azure'i klassikaline portaalis **konfigureerimine** menüü avamiseks valige **Active Directory**ja valige kataloogi nimi.

![Azure Active Directory menüü konfigureerimine][1]

Seejärel saate redigeerida suvandid määrata Külastajate kasutajate jaoks.

![külalisi juhtelemendi juurdepääsusuvandid][2]


## <a name="whats-next"></a>Mis saab edasi

- [Muude kataloogide või partnerid Azure Active Directory kasutajate lisamine](active-directory-create-users-external.md)
- [Azure AD haldamine](active-directory-administer.md)
- [Azure AD paroolide haldamine](active-directory-manage-passwords.md)
- [Azure AD rühmade haldamiseks](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
