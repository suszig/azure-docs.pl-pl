W wersji 3.0 modułu AzureRm.Resources wprowadzono istotne zmiany w sposobie pracy z tagami. Przed kontynuowaniem sprawdź swoją wersję:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Jeśli masz wersję 3.0 lub nowszą, przykłady z tego tematu zadziałają w Twoim środowisku. Jeśli nie masz wersji 3.0 lub nowszej, przed kontynuowaniem pracy z tym tematem [zaktualizuj swoją wersję](/powershell/azureps-cmdlets-docs/) za pomocą Galerii programu PowerShell lub Instalatora platformy sieci Web.

```powershell
Version
-------
3.5.0
```

Za każdym razem, gdy stosujesz tagi do zasobu lub grupy zasobów, istniejące tagi tego zasobu lub tej grupy zasobów są zastępowane. Dlatego konieczne jest różne podejście w zależności od tego, czy dany zasób lub dana grupa zasobów ma istniejące tagi, które chcesz zachować. Aby dodać tagi do:

* grupy zasobów bez istniejących tagów

  ```powershell
  Set-AzureRmResourceGroup -Name TagTestGroup -Tag @{ Dept="IT"; Environment="Test" }
  ```

* grupy zasobów z istniejącymi tagami

  ```powershell
  $tags = (Get-AzureRmResourceGroup -Name TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResourceGroup -Tag $tags -Name TagTestGroup
  ```

* zasobu bez istniejących tagów

  ```powershell
  Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

* zasobu z istniejącymi tagami

  ```powershell
  $tags = (Get-AzureRmResource -ResourceName storageexample -ResourceGroupName TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResource -Tag $tags -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

Aby zastosować wszystkie tagi z grupy zasobów do jej zasobów **bez zachowania tagów istniejących w zasobach**, użyj następującego skryptu:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Aby zastosować wszystkie tagi z grupy zasobów do jej zasobów **z zachowaniem tagów istniejących w zasobach, które nie są duplikatami**, użyj następującego skryptu:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Aby usunąć wszystkie tagi, przekaż pustą tablicę skrótów.

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name TagTestGgroup
```

Aby uzyskać grupy zasobów mające konkretny tag, użyj polecenia cmdlet `Find-AzureRmResourceGroup`.

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Aby uzyskać wszystkie zasoby mające konkretny tag i konkretną wartość, użyj polecenia cmdlet `Find-AzureRmResource`.

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```



<!--HONumber=Feb17_HO3-->


