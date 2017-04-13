<properties
    pageTitle="Azure'i AD teenusest Auth OAuth2.0 abil | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas kasutada HTTP sõnumite rakendada teenusest autentimise abil OAuth2.0 kliendi identimisteabe anda voogu."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="service-to-service-calls-using-client-credentials"></a>Teenuse teenuse kõnedele kliendi mandaadi abil

OAuthi 2.0 kliendi identimisteabe anda Flow võimaldab veebiteenuse ( *konfidentsiaalse klient*) autentimiseks, kui olete kasutaja asemel mõne muu veebiteenuse kutsumine oma mandaadi abil. Selle stsenaariumi korral kliendi on tavaliselt Kesk-astme veebiteenuse, daemon teenuse või veebisaiti.

## <a name="client-credentials-grant-flow-diagram"></a>Kliendi identimisteabe anda vooskeemi

Järgmine diagramm selgitab, kuidas anda kliendi mandaadi meilivoo töötab Azure Active Directory (Azure AD).

![OAuth2.0 kliendi identimisteabe anda meilivoo](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Klientrakenduse autendib Azure AD Turbeloa emissiooni lõpp-punkti ja taotleb juurdepääsu sümboolse.
2. Azure AD Turbeloa emissiooni lõpp-punkti probleemid juurdepääsu luba.
3. Luba juurdepääs kasutatakse turvalise ressursile autentida.
4. Andmete turvalise ressursi tagastatakse veebirakenduse.

## <a name="register-the-services-in-azure-ad"></a>Teenuses Azure AD register

Registreerida nii helistaja teenuse ja vastuvõtmise teenuse Azure Active Directory (Azure AD). Üksikasjalikud juhised leiate teemast [lisamine, värskendamine, ja rakenduse eemaldamine](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Taotleda juurdepääsu sümboolse

Taotleda juurdepääsu sümboolse, kasutage HTTP POSTITUSKOHT rentniku kohased Azure AD lõpp-punkti.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Juurdepääs teenusest Turbeloa taotlus

Teenusest – juurdepääs Turbeloa taotlus sisaldab järgmisi.

| Parameetri | | Kirjeldus |
|-----------|------|------------|
| response_type | nõutav | Saate määrata soovitud vastuse tüüp. Väärtus peab olema kliendi identimisteabe anda meilivoo, **client_credentials**.|
| client_id | nõutav | Määrab Azure AD kliendi id helistaja veebiteenuse. Leiate Azure'i haldusportaal helistaja rakenduse kliendi ID, klõpsake **Active Directory**, klõpsake kataloogi, valige rakendus ja klõpsake nuppu **Konfigureeri**.|
| client_secret | nõutav |  Sisestage Azure AD helistaja veebiteenuse registreeritud. Azure'i haldusportaal klahvi, loomiseks klõpsake **Active Directory**, klõpsake kataloogi, klõpsake selle rakenduse ja klõpsake nuppu **Konfigureeri**. |
| ressurss | nõutav | Sisestage rakenduse ID URI vastuvõtmise veebiteenuse. Leiate Azure'i haldusportaal rakenduse ID URI, klõpsake **Active Directory**, klõpsake kausta, valige rakendus ja klõpsake nuppu **Konfigureeri**. |

## <a name="example"></a>Näide

Järgmised HTTP postituse taotleb juurdepääsu sümboolse https://service.contoso.com/ veebiteenuse. Funktsiooni `client_id` tuvastab veebiteenus, mis taotleb juurdepääsu luba.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Teenusest juurdepääsu loa vastuse

Edu vastus sisaldab järgmisi JSON OAuth 2.0 vastust.

| Parameetri   | Kirjeldus |
|-------------|-------------|
|access_token |Nõutud juurdepääsu luba. Funktsiooni helistaja veebiteenuse abil saate selle märgiks vastuvõtmise veebiteenuse autentida. |
|access_type  | Näitab, et loa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on **esitaja**. Esitaja sõned kohta leiate lisateavet teemast soovitud [OAuthi 2.0 autoriseerimine raames: esitaja Turbeloa kasutus (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Kui kaua kehtib juurdepääsu luba (sekundites).|
|expires_on   |Kellaaeg, millal juurdepääsu luba on aegunud. Kuupäev esitatakse arvuna sekundite arv 1970-01-01T0:0:0Z UTC kuni järgneb. Seda väärtust kasutatakse vahemällu talletatud sõned kestuse määramiseks. |
|ressurss     | Rakenduse ID URI vastuvõtmise veebiteenuse. |

## <a name="example"></a>Näide

Järgmises näites on kujutatud edu vastuseks juurdepääsu sümboolse edastamine veebiteenusele taotluse.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Vt ka

* [Azure AD OAuthi 2.0](active-directory-protocols-oauth-code.md)
