## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Valmistatakse Azure ressursihaldur taotluste kinnitamiseks

Tuleb autentida kõik toimingud, mida ressursse kasutades [Azure ressursihaldur] [ lnk-authenticate-arm] koos Azure Active Directory (AD). Seadistada see lihtsaim viis on kasutada PowerShelli või Azure CLI.

Installige [Azure PowerShell 1.0] [ lnk-powershell-install] või uuem versioon enne jätkamist.

Järgmised juhised kirjeldavad, kuidas luua parooli autentimine taotluse AD PowerShelli abil. Käivitada need käsud standard PowerShelli seansis.

1. Logi sisse oma Azure'i abonementide järgmise käsuga:

    ```
    Login-AzureRmAccount
    ```

2. Märgi üles oma **TenantId** ja **SubscriptionId**. Te peate neid hiljem.

3. Luua uue Azure Active Directory taotluse järgmise käsuga, asendades koha omanikud:

    - **{Kuvatav nimi}:** nt **MySampleApp** kuvatav nimi
    - **{Avalehe URL}:** oma rakenduse, näiteks **http://mysampleapp/home**avalehe URL. See URL ei pruugi käsk otse taotluse.
    - **{Application identifier}:** Kordumatu identifikaatori, näiteks **http://mysampleapp**. See URL ei pruugi käsk otse taotluse.
    - **{Parooli}:** Parool, mida kasutate kinnitamiseks oma app.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Märkige **ApplicationId** loodud taotluse. On vaja see hiljem.

5. Uue teenuse peamine järgmise käsuga, asendades **{MyApplicationId}** **ApplicationId** eelmises juhises loomine

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Järgmise käsuga, asendades **{MyApplicationId}** teie **ApplicationId**rollimäärangu setup.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Olete nüüd lõpetanud loomine Azure AD rakendus, mis võimaldab teil oma kohandatud C# taotluse kinnitamiseks. Hilisemas õpetamisel on vaja järgmisi väärtusi:

- TenantId
- SubscriptionId
- ApplicationId
- Parooli

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
