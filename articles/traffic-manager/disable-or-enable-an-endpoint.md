<properties
   pageTitle="Keelamine või lubamine liikluse haldur endpoint | Microsoft Azure'i"
   description="See artikkel aitab keelamine või lubamine liikluse haldur profiili lõpp-punktid."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Liikluse haldur Endpoint lubamine ja keelamine

Samuti saate keelata üksikute lõpp-punktid, mis on osa liikluse haldur profiili. Lõpp-punktid kaasata nii pilveteenustega ja veebisaidid. Lõpp-punkti keelamine jätab profiili osana, kuid profiili toimib kui lõpp-punkti ei sisalda seda. See toiming on väga kasulik ajutiselt eemaldamine on hooldus režiimis või kogurahastus lõpp-punkt. Kui lõpp-punkti on uuesti ja töötab, saate lubada

>[AZURE.NOTE] **Lõpp-punkti keelamine ei ole midagi Azure juurutamise olekuga. Terve lõpp-punkti jäävad üles ja saada liikluse isegi siis, kui keelatud liikluse haldur. Lisaks keelamine lõpp üks profiil ei mõjuta selle mõne muu profiili olek.**

## <a name="to-disable-an-endpoint"></a>Lõpp-punkti keelamine

1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
1. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis kuuluvad teie konfiguratsiooni kuvamiseks.
1. Klõpsake nuppu lõpp-punkti, mida soovite keelata, ja klõpsake lehe allosas **Keela** .
1. Liikluse katkestab minevaid vastavalt selle DNS-i aeg-to-Live (TTL) domeeninimi halduri liikluse jaoks konfigureeritud lõpp-punkti. Saate muuta TTL liikluse haldur profiili lehel konfigureerimine.

## <a name="to-enable-an-endpoint"></a>Kui soovite lubada lõpp


1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
1. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis kuuluvad teie konfiguratsiooni kuvamiseks.
1. Klõpsake nuppu lõpp-punkti, mida soovite lubada, ja seejärel käsku **Luba** lehe allosas.
1. Teenusega uuesti, mille määrab profiili rakendamist alustatakse liikluse.

## <a name="next-steps"></a>Järgmised sammud

[Liiklus Manager - Keela Luba või profiili kustutamine](disable-enable-or-delete-a-profile.md)

[Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)

[Liikluse haldur jõudluse huvides](traffic-manager-performance-considerations.md)