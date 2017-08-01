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

Aby wyświetlić istniejące tagi dla *grupy zasobów*, użyj:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Ten skrypt zwraca następujący format:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Aby wyświetlić istniejące tagi dla *zasobu o określonym identyfikatorze zasobu*, użyj:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Aby wyświetlić istniejące tagi dla *zasobu o określonej nazwie i grupie zasobów*, użyj:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Aby uzyskać *grupy zasobów, które mają konkretny tag*, użyj:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Aby uzyskać *zasoby, które mają konkretny tag*, użyj:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Za każdym razem, gdy stosujesz tagi do zasobu lub grupy zasobów, istniejące tagi tego zasobu lub tej grupy zasobów są zastępowane. Dlatego konieczne jest różne podejście w zależności od tego, czy dany zasób lub dana grupa zasobów ma istniejące tagi. 

Aby dodać tagi do *grupy zasobów bez istniejących tagów*, użyj:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Aby dodać tagi do *grupy zasobów z istniejącymi tagami*, pobierz istniejące tagi, dodaj nowy tag i ponownie zastosuj tagi:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Aby dodać tagi do *zasobu bez istniejących tagów*, użyj:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

Aby dodać tagi do *zasobu z istniejącymi tagami*, użyj:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

Aby zastosować wszystkie tagi z grupy zasobów do jej zasobów *bez zachowania tagów istniejących w zasobach*, użyj następującego skryptu:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Aby zastosować wszystkie tagi z grupy zasobów do jej zasobów *z zachowaniem tagów istniejących w zasobach, które nie są duplikatami*, użyj następującego skryptu:

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

Aby usunąć wszystkie tagi, przekaż pustą tablicę skrótów:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



