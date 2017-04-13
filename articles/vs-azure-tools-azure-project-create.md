<properties
   pageTitle="Visual Studio on Azure projekti loomine | Microsoft Azure'i"
   description="Visual Studio on Azure projekti loomine"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Visual Studio on Azure projekti loomine

Azure'i Tools for Visual Studio pakuvad Mall, mis võimaldab teil luua pilveteenus Azure. Tööriistade aitab teil konfigureerida, silumine ja juurutada Azure pilveteenusesse.

Lahenduse Azure pilveteenuse teenuse sisaldab järgmist tüüpi projektid.

- **Azure'i projekti**

    Azure'i projekt on ühendused rolli projektidele lahendus. See hõlmab ka teenuse määratlus ja teenuse konfiguratsiooni faile. Teenuse definitsioonifail määratleb käitusaja sätted, sealhulgas rollid, mis on nõutavad, lõpp-punktid ja virtuaalse masina suurus. Teenuse konfiguratsioonifail konfigureerib käivitatakse mitu eksemplari rollid ja sätete rollile määratud väärtused. Nende sätete kohta leiate lisateavet teemast [kohta: Azure'i pilveteenuses, Visual Studio rollid konfigureerimine](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Project Web roll**

    Töötaja roll teostab tausta töötlemine. Töötaja roll saate suhelda salvestusruumi teenustega ja muid Interneti-põhiste teenuste. Töötaja roll võib olla mis tahes arv HTTP, HTTPS või TCP lõpp-punktid.

    - **ASP.net-i Web roll**, hoone ASP.net-i rakenduse web vastendamisel
    - **ASP.net-i MVC5 Web roll**
    - **ASP.net-i MVC4 Web roll**
    - **ASP.net-i MVC3 Web roll**
    - **WCF-i teenuse Web roll**, hoone WCF-teenus
    - **Silverlighti Business rakenduse Web roll** (nõuab Visual Studio 2012)

- **Vahemälu töötaja roll**

    Roll, mis pakub asjakohast vahemälu rakenduse.

- **Töötaja rolli teenuse siini järjekord**

    Teenuse siini järjekorda, mis võimaldab sõnumi andmebaasitõrge funktsioonid suhelda töötaja protsessi. Lisateavet leiate teemast [kasutamine teenuse siini järjekorrad](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Visual Studio on Azure pilveteenuse teenuse projekti loomise kohta

1. Microsoft Visual Studio Käivita administraatorina.

1. Valige menüü **fail**, **Uus** **Projekt**.

1. **Projekti tüübid** paanil valige **pilve** Visual C# või Visual Basicu projekti malli sõlmed.

1. Valige paanil **Mallid** **Azure'i pilveteenuses**.

1. Määrata, millist versiooni soovite kasutada oma projekti .NET Frameworki.

1. Sisestage nimi ja projekti asukoht ja nimi lahendus. Klõpsake nuppu **OK** .

1. Dialoogiboksis **Uue Azure'i projekti** valige rollid, mida soovite lisada ja nende lisamine oma lahenduse nuppu paremnool. Saate lisada nii palju rollid, kui teil on vaja.

1. Roll, mille olete lisanud oma projekti ümbernimetamiseks hoidke kursorit dialoogiboksi **Uue Azure'i projekti** rolli ja valige roll paremal ikooni **ümber nimetada** . Saate ümbernimetamine rolli teie lahendus jooksul pärast selle lisamist.
