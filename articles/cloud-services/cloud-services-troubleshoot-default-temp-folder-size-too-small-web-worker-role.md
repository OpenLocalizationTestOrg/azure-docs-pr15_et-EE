<properties
   pageTitle="Vaikimisi TEMP kausta maht on liiga väike rolli | Microsoft Azure'i"
   description="Pilveteenuse teenuse roll on piiratud hulgal ruumi kausta TEMP. Selles artiklis on toodud mõned soovitused, kuidas vältida ruumipuuduse."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Kausta TEMP vaikesuurust on liiga väike pilvepõhise teenuse web/töötaja rollile

Pilveteenuse töötaja või web rolli ajutine vaikekataloogi on maksimaalne maht 100 MB, mis võivad muutuda täielik mingil hetkel. Selles artiklis kirjeldatakse, kuidas vältida ruumipuuduse ajutise kausta jaoks.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Miks otsa ruumi?

Standardse Windowsi keskkonna muutujad TEMP ja TMP on saadaval kood, kus teie rakendus töötab. Nii TEMP ja TMP osutage ühe kataloogi, mis on maksimaalne maht on 100 MB. Kõik selles kaustas talletatud andmetega mitte hoitakse kogu elutsükli pilveteenusesse; kui selle rolli aknad pilveteenus on taaskasutamine, on puhastada kataloogi.

## <a name="suggestion-to-fix-the-problem"></a>Päringusoovituste probleemi lahendamiseks

Rakendada üks järgmistest variantidest:

- Kohaliku ressursside ja juurdepääs sellele otse TEMP või TMP asemel. Juurdepääsu kohaliku ressursi kood, kus töötab teie rakenduses, helistage [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) meetod. 

- Kohaliku ressursside ja valige käsk TEMP ja TMP kataloogide osutamiseks kohalikku ressursi tee. See muudatus tuleb teha sees [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) meetod.

Järgmine kood näide näitab, kuidas muuta target kataloogid TEMP ja TMP kaudu sees OnStart meetodit:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Järgmised sammud

Lugege ajaveebi, kus kirjeldatakse, [Kuidas suurendada Azure Web rolli ASP.net-i ajutiste kausta](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Saate vaadata rohkem [artikleid tõrkeotsingu](/?tag=top-support-issue&product=cloud-services) pilveteenustega.

Saate teada, kuidas pilvepõhise teenuse roll probleemide tõrkeotsing, kasutades Azure PaaS arvuti diagnostika andmeid, vaadata [Kevin Williamson ajaveebi sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
