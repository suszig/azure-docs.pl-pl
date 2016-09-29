<properties
    pageTitle="Zmiana identyfikatora dzierżawy magazynu kluczy po przeniesieniu subskrypcji | Microsoft Azure"
    description="Dowiedz się, jak przełączyć identyfikator dzierżawy klucza magazynu po przeniesieniu subskrypcji do innej dzierżawy"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>


# Zmiana identyfikatora dzierżawy magazynu kluczy po przeniesieniu subskrypcji
### Pytanie: Moja subskrypcja została przeniesiona z dzierżawy A do dzierżawy B. Jak zmienić identyfikator dzierżawy dla istniejącego magazynu kluczy i ustawić prawidłowe listy ACL dla nazw głównych w dzierżawie B?

Po utworzeniu nowego magazynu kluczy w subskrypcji zostaje on automatycznie powiązany z domyślnym identyfikatorem dzierżawy usługi Azure Active Directory dla tej subskrypcji. Wszystkie wpisy zasad dostępu również zostają powiązane z tym identyfikatorem dzierżawy. Po przeniesieniu subskrypcji platformy Azure z dzierżawy A do dzierżawy B istniejące magazyny kluczy są niedostępne dla nazw głównych (użytkowników i aplikacji) w dzierżawie B. Aby rozwiązać ten problem, należy:

- zmienić identyfikator dzierżawy skojarzony ze wszystkimi istniejącymi magazynami kluczy w tej subskrypcji na dzierżawę B,
- usunąć wszystkie istniejące wpisy zasad dostępu,
- dodać nowe wpisy zasad dostępu skojarzone z dzierżawą B.

Jeśli na przykład masz magazyn kluczy „myvault” w subskrypcji przeniesionej z dzierżawy a do dzierżawy B, oto jak zmienić identyfikator dzierżawy dla tego magazynu kluczy i usunąć stare zasady dostępu.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId $vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties $vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId $vault.Properties.AccessPolicies = @() Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Ponieważ ten magazyn przed przeniesieniem był w dzierżawie A, pierwotna wartość **$vault.Properties.TenantId** to dzierżawa A, podczas gdy **(Get-AzureRmContext).Tenant.TenantId** to dzierżawa B.

Po skojarzeniu magazynu z odpowiednim identyfikatorem dzierżawy i usunięciu starych wpisów zasad dostępu ustaw nowe wpisy zasad dostępu przy użyciu polecenia [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## Następne kroki

- Jeśli masz pytania dotyczące usługi Key Vault, odwiedź [forum usługi Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)



<!--HONumber=Sep16_HO3-->


