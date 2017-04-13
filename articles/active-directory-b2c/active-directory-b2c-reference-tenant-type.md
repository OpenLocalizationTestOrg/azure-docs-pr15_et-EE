<properties
    pageTitle="Azure Active Directory B2C: Tootmisvõimsust vs eelvaade B2C rentnikukontodele | Microsoft Azure'i"
    description="Teema Azure Active Directory B2C rentnikud tüübid"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Tootmisvõimsust vs eelvaade B2C rentnikukontodele

Kui kavatsete kirjutada tootmise rakenduse Azure Active Directory (Azure AD) B2C, peate olla kindel, et teil on õige rentniku "tüüp" liikumiseks klõpsake reaalajas. Näha, mida teil on, tehke liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure portaali ja vaadake jaotist **rentniku tüüp**.

## <a name="summary"></a>Kokkuvõte

Azure'i AD B2C toetab ainult sees **- tootmisvõimsust** B2C rentnikukontodele Põhja-Ameerikas tootmise rakendused.

| Rentniku tüüp | Riigid/regioonid | Üldiselt-saadaval? |
| ----------- | -------------- | --------------------- |
| **Tootmisvõimsust – rentniku** | Põhja-Ameerika riigid/regioonid | Jah |
| **Tootmisvõimsust – rentniku** | Kõik riigid/regioonid, välja arvatud Põhja-Ameerika | Ei |
| **Rentniku eelvaade** | Kõik riigid/regioonid | Ei |

> [AZURE.NOTE]
Azure'i AD B2C rentnikud (tarbija puhul) pole praegu saadaval mõne riikidest ja piirkondadest, kus on saadaval Azure AD rentnikud (töötajate jaoks). Lugege lisateavet järgmistest jaotistest.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Tootmisvõimsust B2C rentniku Põhja-Ameerika

Kui olete [loonud teie B2C rentniku](active-directory-b2c-get-started.md) Põhja-Ameerikas, st ühes järgmistest riigid või regioonid: Ameerika Ühendriikides, Kanadas, Costa Rica, Dominikaani Vabariik, El Salvador, Guatemala, Mehhiko, Panama, Puerto Rico ja Trinidad ja Tobago, ja **rentniku tüüp** oma B2C administraator UI ütleb **tootmisvõimsust**, teie rentnikukontoga saab kasutada tootmise rakendused.

> [AZURE.NOTE]
Tootmisvõimsust rentnikud suudavad mastaapimise 100s miljoneid tarbija identiteedid rentniku kohta.

![Kuvatõmmis – tootmisvõimsust rentniku jaoks](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Eelvaate B2C rentniku iga riigi/regiooni

Kui olete loonud B2C rentniku Azure AD B2C eelvaade perioodi jooksul, on tõenäoline, et teie **rentniku tippige** ütleb **eelvaate rentniku**. Kui see on nii, peate kasutama oma rentniku ainult arendamise ja testimiseks, mitte tootmise rakendused.

> [AZURE.IMPORTANT]
On pole migreerimise tee eelvaade B2C rentniku-tootmisvõimsust B2C rentniku jaoks. Pange tähele, et on teadaolevad probleemid kui kustutate eelvaade B2C rentniku ja uuesti luua-tootmisvõimsust B2C rentniku domeeni nimi. Teil on muu domeeninimega-tootmisvõimsust B2C rentniku loomine.

![Kuvatõmmis – rentniku eelvaade](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Tootmisvõimsust B2C rentniku väljaspool Põhja-Ameerika

Azure'i AD B2C praegu ei üldiselt-saadaval väljaspool Põhja-Ameerika. Siiski saate luua ja kasutada-tootmisvõimsust rentnikud, katsetamine eesmärgil ühel järgmised riigid või regioonid: Alžeeria, Austria, Aserbaidžaan, Bahrein, Valgevene, Belgia, Bulgaaria, Horvaatia, Küpros, Tšehhi Vabariik, Taani, Egiptus, Eesti, Soome, Prantsusmaa, Saksamaa, Kreeka, Ungari, Island, Iirimaa, Iisrael, Itaalia, Jordaania, Kasahstan, Keenia, Kuveit, Läti, Liibanon, Liechtenstein, Lituania, Luksemburg, Makedoonia Vabariik, Malta, Montenegro, Maroko, Holland, Nigeeria, Norra , Omaan, Pakistan, Poola, Portugali, Katar, Rumeenia, vene, Saudi Araabia, Serbia, Slovakkia, Sloveenia, Lõuna-Aafrika, Hispaania, Rootsi, Šveits, Tuneesia, Türgi, Ukraina, Araabia Ühendemiraadid ja Ühendkuningriik.

Pärast Azure AD B2C teatab üldiselt kättesaadav kohal riikidest ja piirkondadest, võite kasutada nende tootmisvõimsust rentnikud ja minge live tootmise rakendustega, ilma andmete kadumise.

## <a name="availability-of-b2c-tenants"></a>B2C rentnikud kättesaadavus

B2C rentnikud on praegu saadaval järgmised riigid või regioonid: Afganistanis, Argentina, Austraalia, Brasiilia, Tšiili, Colombia, Ecuador, Hongkongi erihalduspiirkond, India, Indoneesia, Iraak, Jaapani, Korea, Malaisia, Uus-Meremaa, Paraguay, Peruu, Filipiinid, Singapur, Filipiinid, Taiwan, Tai, Uruguay ja Venezuela. Plaanis need tulevikus kaasata.
