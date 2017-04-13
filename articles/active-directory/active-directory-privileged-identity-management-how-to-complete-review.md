<properties
   pageTitle="Kuidas täita ülevaate access | Microsoft Azure'i"
   description="Kui käivitasite Azure AD õigustega identiteedi haldamine Accessi ülevaate, saate teada, kuidas see lõpetamine ja tulemusi vaadata"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Kuidas täita ülevaate access Azure AD õigustega identiteedi haldamine


Õigustega rolli administraatorid saate vaadata õigustega juurdepääsu kord [Turvalisus läbivaatus käivitas](active-directory-privileged-identity-management-how-to-start-security-review.md). E-posti, mis annab märku kasutajad üle vaadata oma Accessi saadab automaatselt Azure AD õigustega identiteetide haldus (PIM). Kui kasutaja ei saanud e-posti, saate saata neid juhiseid [Turvalisus läbi sooritamiseks](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Pärast turvalisus läbivaatus lõppu või kõigi kasutajate lõpetamist nende omas läbivaatus, järgige arvustus hallata ning vaadata tulemusi selles artiklis toodud juhiseid.

## <a name="manage-security-reviews"></a>Arvamused haldamine

1. [Azure'i portaal](https://portal.azure.com/) ja valige **Azure AD õigustega identiteedi haldamise** rakendus oma armatuurlaud.
2. Valige **Access kontrollib** armatuurlaua osa.
3. Valige Accessi läbivaatus, mida soovite hallata.

Accessi läbivaatus detail tera on arvude suvandid haldamise läbivaatamine.

![PIM Accessi läbivaatus nuppude - kuvatõmmis][1]

### <a name="remind"></a>Lisage selgitus

Kui access ülevaate on häälestatud nii, et kasutajad vaadata ise, nupp **Remind** saadab välja teatis. 

### <a name="stop"></a>Stopp

Kõik Accessi kommentaare on lõppkuupäev, kuid saate seda lõpetada ennetähtaegselt nupp **Peata** . Kui kõik kasutajad pole vaadanud sel ajal, nad ei saa pärast seda, kui lõpetate arvustus. Arvustust ei saa taaskäivitada, kui see on peatatud.

### <a name="apply"></a>Rakendamine

Kui access ülevaate on lõpule jõudnud, kas jõudnud lõppkuupäev või seda käsitsi, on peatatud nuppu **Rakenda** rakendab läbivaatuse tulemuste. Kui kasutaja juurdepääs on keelatud ülevaates, see on toiming, mis eemaldab oma rolli määramine.  

### <a name="export"></a>Eksportimine

Kui soovite rakendada turvalisus läbivaatuse tulemuste käsitsi, saate eksportida läbivaatamine. Nupp **ekspordi** hakkavad CSV-faili allalaadimine. Saate hallata tulemused Excelis või muudes programmides, mis avaneb CSV-failid.

### <a name="delete"></a>Kustutamine

Kui olete huvitatud läbivaatus täiendavate, kustutage see. Nupp **Kustuta** eemaldab arvustus PIM rakendus.

> [AZURE.IMPORTANT] Te ei hoiatus enne esimest kustutamise, et olla kindel, kas soovite kustutada selle läbivaatus.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
