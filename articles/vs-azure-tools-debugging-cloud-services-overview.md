<properties 
   pageTitle="Azure'i pilveteenustega silumine | Microsoft Azure'i"
   description="Azure'i pilveteenustega silumine"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Pilveteenustega silumine

Erineval abil saate silumine Azure rakenduse Microsoft Visual Studio ja Azure SDK Azure'i tööriistade abil:

- Saate silumine Azure rakenduse Visual Studio, kui teil on tekkinud, nagu mis tahes rakenduses Visual C# või Visual Basic. Lisateabe saamiseks lugege teemat [silumine oma kohalikus arvutis pilveteenuses](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Azure'i diagnostika abil saate üksikasjalikku teavet koodi, mis töötab rollid, jooksul sisse logida, kas rollid töötab soovitud arenduskeskkond või Azure. Lisateabe saamiseks lugege teemat [kogumine logimine andmete Azure'i diagnostika abil](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Kui kasutate Visual Studio Enterprise kirjutamiseks suunatud .NET Framework 4 või .NET Framework 4.5 rollid, saate lubada IntelliTrace juurutada Azure pilveteenuse teenuse Visual Studio ajal. IntelliTrace pakub log, mida saab kasutada koos Visual Studio silumine rakenduse, kui see ei tööta Azure. Lisateavet leiate teemast [silumine avaldatud pilveteenuses IntelliTrace ja Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Saate lubada remote silumine oma cloud Services ajal, kui juurutate Visual Studio pilveteenusesse. Kui valite lubamiseks juurutamine remote silumine, Kaug silumine teenused on installitud virtuaalmasinates, mis töötavad iga rolli eksemplari. Nende teenuste msvsmon.exe, nt mõjutada jõudlust või tulemuseks kulude eest. Lisateavet leiate teemast [silumine pilveteenus Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



