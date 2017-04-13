<properties
   pageTitle="Keelata, lubada või kustutada liikluse haldur profiili | Microsoft Azure'i"
   description="See artikkel aitab teil töötada teie haldur liikluse profiilid."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Keelata, lubada või profiili kustutamine


Olemasolevale profiilile liikluse haldur saate keelata nii, et see ei viita kasutaja taotlused selle konfigureeritud lõpp-punktid. Kui keelate liikluse haldur profiili, ise profiili ja profiili andmed jäävad muutmata ja kasutajaliidese liikluse haldur saab redigeerida. Kui soovite uuesti lubamiseks profiili, saate hõlpsasti teha nii, et selle Azure jätkub portaali ja viited. Azure'i portaalis liikluse haldur profiili loomisel on see automaatselt lubatud.

## <a name="to-disable-a-profile"></a>Profiili keelamine

1. Muutke oma Interneti DNS-i serveris kasutada vastav kirje tüüp ja teise nime või IP-aadress kindlasse asukohta kursori Internetis DNS-i ressursikirje. Teisisõnu, muuta DNS-i ressursikirje Interneti DNS-i serverisse, et ta enam kasutab ressursi CNAME-kirje, mis viitab domeeninimi halduri liikluse profiili.
1. Liikluse katkestab suunatud liikluse haldur profiili sätete kaudu lõpp-punktid.
1. Valige eemaldatav profiil, mida soovite keelata. Valige eemaldatav profiil lehel liikluse Manager esiletõstmine profiili, klõpsates veeru kõrval profiili nimi. Klõpsake nime profiili või nime kõrval olevat noolt, nagu see viib teid profiili lehel sätted.
1. Pärast profiili valimist klõpsake lehe allosas Keela.

## <a name="to-enable-a-profile"></a>Profiili lubamiseks

1. Valige eemaldatav profiil, mida soovite lubada. Valige eemaldatav profiil lehel liikluse Manager esiletõstmine profiili, klõpsates veeru kõrval profiili nimi. Klõpsake nime profiili või nime kõrval olevat noolt, nagu see viib teid profiili lehel sätted.
1. Pärast profiili valimist klõpsake nuppu Luba lehe allosas.
1. Muutke oma Interneti DNS-i serveris kasutada CNAME-i kirje tüüp, mis kaardid teie ettevõtte domeeninime domeeninimi halduri liikluse profiili DNS-i ressursikirje. Lisateabe saamiseks vt [punkt ettevõtte Interneti-domeeni domeeniga liikluse haldur](traffic-manager-point-internet-domain.md).
1. Liikluse hakatakse uuesti suunata lõpp-punktid.

## <a name="delete-a-profile"></a>Profiili kustutamine


1. Veenduge, et DNS-i ressursikirje Interneti DNS-i serverisse enam kasutab ressursi CNAME-kirje, mis viitab domeeninimi halduri liikluse profiili.
1. Valige eemaldatav profiil, mille soovite kustutada. Valige eemaldatav profiil lehel liikluse Manager tõstke esile profiili järgi
1. Klõpsake veergu profiili kõrval. Klõpsake nime profiili või nime kõrval olevat noolt, nagu see viib teid profiili lehel sätted.
1. Pärast profiili valimist klõpsake nuppu Kustuta lehe allosas.

## <a name="next-steps"></a>Järgmised sammud

[Liikluse haldur - keelamine või lubamine lõpp](disable-or-enable-an-endpoint.md)

[Tõrkesiirde marsruutimise meetod konfigureerimine](traffic-manager-configure-failover-routing-method.md)

[Round jaan marsruutimise meetod konfigureerimine](traffic-manager-configure-round-robin-routing-method.md)

[Jõudluse marsruutimise meetod konfigureerimine](traffic-manager-configure-performance-routing-method.md)

[Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)

