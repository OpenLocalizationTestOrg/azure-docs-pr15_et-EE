<properties
   pageTitle="Luba rentnikuga rakendustes | Microsoft Azure'i"
   description="Kuidas teha autoriseerimine rentnikuga rakenduses"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Rollipõhine ja ressursside-põhiste autoriseerimine rentnikuga rakendustes

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

See artikkel on [osa sarjast]. Olemas on ka täieliku [valimi rakendus] , mis kaasneb selle sarja.

Meie [viide rakendamine] on rakendus ASP.net-i Core 1.0. Selles artiklis vaatame kaks üldist lähenemisviisi luba, et loa API-de ASP.net-i Core 1.0 ette.

-   **Rollipõhine autoriseerimine**. Toimingu kasutajale määratud rolle alusel lubatakse. Näiteks on vaja mõned toimingud administraatorirolli.
-   **Ressursside-põhiste autoriseerimine**. Toimingu kindla ressursi alusel lubatakse. Näiteks on iga ressursi omanik. Omanik saate kustutada ressurss; teised kasutajad ei saa.

Tüüpilised rakenduse võtab tööle kombineerida. Näiteks ressursi kustutamiseks kasutaja peab olema ressursi omanik _või_ administraatoriks.


## <a name="role-based-authorization"></a>Rollipõhine autoriseerimine

[Tailspin uuringute] [ Tailspin] rakenduse määratleb järgmisi rolle:

- Administraatori poole. Saate toiminguid kõigi CRUD mis tahes küsitlust, mis kuulub sellele rentnikule.
- Looja. Saate luua uusi küsitlusi
- Lugeja. Saate lugeda mis tahes küsitlused, mis kuuluvad sellele rentnikule

_Kasutajate_ rakenduse rakendada rollid. Kasutaja on rakenduse küsitlused, kas administraator, looja või lugeja.

Arutelu, kuidas määratlemine ja rollide haldamine, vaadake teemat [rakenduse rollid].

Sõltumata sellest, kuidas hallata rollid, sarnaneb teie kood. ASP.net-i Core 1.0 tutvustab mõne võtmiseks nimetatakse [autoriseerimine poliitikate][policies]. Selle funktsiooniga määratlemine autoriseerimine poliitikate kood ja seejärel rakendada neid kontrolleril toimingud. Poliitika on sidumata selle domeenikontrolleri.

### <a name="create-policies"></a>Poliitikate loomine

Poliitika määratlemiseks kõigepealt looma ainekursuse, mis rakendab `IAuthorizationRequirement`. On lihtsam, et tuletada `AuthorizationHandler`. Klõpsake soovitud `Handle` meetod uurida oluline siia.

Siin on näide rakendusest Tailspin uuringute:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Vt [SurveyCreatorRequirement.cs]

See määratleb kasutaja luua uue küsitluse nõue. Kasutaja peab olema SurveyAdmin või SurveyCreator roll.

Määratlege käivitus tunni, nimega poliitika, mis sisaldab ühe või mitme nõuetele. Kui loendis on mitu nõuded, kasutaja peab vastama _iga_ nõue lubada. Järgmine kood määratleb kahe poliitika:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Vt [Startup.cs]

Järgmine kood määrab ka autentimise skeem, mis näitab, milliseid autentimise vahevara peaks töötama, kui autoriseerimine nurjub ASP.net-i. Sel juhul, me määrata küpsise autentimine vahevara, küpsise autentimine vahevara saate suunata kasutaja "Keelatud" lehele. Keelatud lehe asukohta asub AccessDeniedPath suvandi küpsise vahevara; vt [vahevara autentimise konfigureerimine].

### <a name="authorize-controller-actions"></a>Lubada kontrolleril toimingud

Lõpuks lubada toimingu mõne MVC kontrolleril, seadke poliitika on `Authorize` atribuut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

ASP.net-i varasemates versioonides, peaksite atribuudi **rollid** atribuudi alusel:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

See on endiselt ASP.net-i Core 1.0 toetatud, kuid see on mõned puudused võrreldes autoriseerimine poliitikad:

-   Eeldatakse, et teatud kindlate taotluste tüüp. Poliitika saate otsida mis tahes taotluse tüüp. Rollid on nõude tüübist.
-   Rolli nimi on raske-koodiga sisse atribuuti. Poliitika, autoriseerimine loogika on ühes kohas, mis on lihtsam värskendamine või isegi laadi konfiguratsiooni sätted.
-   Poliitika võimaldab keerukamaid autoriseerimine otsuseid (vanus > = 21) ei saa mis väljendatud lihtsa rolli liikmestaatust.

## <a name="resource-based-authorization"></a>Loa ressurssi

_Ressursi vastavalt autoriseerimine_ ilmneb iga kord, kui luba sõltub teatud ressurss, mis mõjutavad toimingu. Rakenduses Tailspin Küsitlused on iga küsitluse omaniku ja null-mitmele osaliste.

-   Omanik saab lugeda, värskendada, kustutada, avaldamine ja avaldamise küsitlus.
-   Omanik saab määrata osaliste küsitlus.
-   Osaliste saate lugeda ja värskendada küsitlus.

Pange tähele, et "omanik" ja "kaasautor" ei ole rakenduse rollide; need on talletatud uuringu rakenduse andmebaasi kohta. Kontrollimaks, kas kasutaja saab kustutada küsitluse, näiteks rakendus kontrollib, kas kasutaja on omanik selle küsitluse jaoks.

ASP.net-i Core 1.0, rakendada ressursi-põhine loa tulenevad **AuthorizationHandler** ja alistamine **toime** meetod.

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Pange tähele, et see tund tungivalt tipitakse küsitluse objektide puhul.  Registreeruge ainekursuse DI käivitamisel:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Kasutage autoriseerimine kontrolle teha **IAuthorizationService** kasutajaliides, mis te saate annavad oma kontrollerid sisse. Järgmine kood kontrollib, kas kasutaja saab lugeda küsitluse.

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Kuna me edastada on `Survey` objekti, käivitub selle kõne on `SurveyAuthorizationHandler`.

Autoriseerimine koodi, hea lähenemine on aggregate kõik kasutaja Rollipõhine ja ressursside-põhiste õigusi, siis märkige ruut liitmise maha soovitud toimingu.
Siin on näide küsitluste rakenduse kaudu. Rakenduse määratleb õiguste Mitmesugust:

- Administraator
- Kaasautor
- Looja
- Omanik
- Lugeja

Rakendus on määratletud võimalike toimingute vaatluste:

- Loomine
- Lugemine
- Värskendamine
- Kustutamine
- Avaldamine
- Unpublsh

Järgmine kood tekitab õigused teatud kasutaja ja küsitluse loendit. Pange tähele, et kasutaja rakenduse rollid ja omanik/kaasautor väljad küsitlus näeb välja järgmine kood.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Lugege teemat [SurveyAuthorizationHandler.cs].

Mitme rentniku rakenduses, veenduge, et õigused ei "leak" teise rentniku andmetega. Rakenduses Küsitlused on lubatud kaasautori luba rentnikud &mdash; saate määrata kellelegi teisele rentnikus, kus on contriubutor. Muud tüüpi õigus on piiratud ressursse, mis kuuluvad selle kasutaja rentnik. Selle nõude jõustamiseks kood kontrollib rentniku ID enne selle õiguste andmist. (Selle `TenantId` välja antud küsitlus loomisel.)

Järgmiseks on kontrollida vastu õiguste toimingu (lugemine, värskendamine, kustutamine, jne). Küsitluste rakenduse rakendab selle etapi otsingutabel funktsioonide abil:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Järgmised sammud

- Järgmise artiklist selle sarja: [turvamine on kirjutamata web API rentnikuga rakenduses][web-api]
- ASP.net-i 1.0 Core loa ressurssi kohta leiate lisateavet teemast [Ressursside põhine loa][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[Sarja mittekuuluva]: guidance-multitenant-identity.md
[Rakenduse rollid]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[viide rakendamine]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Vahevara autentimise konfigureerimine]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[proovi taotluse]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
