---
title: Eksportowanie szablonu usługi Azure Resource Manager | Microsoft Docs
description: Za pomocą usługi Azure Resource Manager można wyeksportować szablon z istniejącej grupy zasobów.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/03/2016
ms.author: tomfitz

---
# Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów
Usługa Resource Manager umożliwia wyeksportowanie szablonu usługi Resource Manager z istniejących zasobów w ramach subskrypcji. Możesz użyć wygenerowanego szablonu, aby dowiedzieć się więcej o składni szablonu lub aby zautomatyzować ponowne wdrożenie rozwiązania, w razie potrzeby.

Należy pamiętać, że istnieją dwa różne sposoby eksportowania szablonu:

* Możesz wyeksportować szablon, który faktycznie został użyty na potrzeby wdrożenia. W wyeksportowanym szablonie wszystkie parametry i zmienne występują dokładnie tak, jak w oryginalnym szablonie. To pomocne rozwiązanie w przypadku, gdy zasoby wdrożono za pośrednictwem portalu. Przyda Ci się zatem wiedza na temat tego, jak skonstruować szablon umożliwiający utworzenie tych zasobów.
* Możesz wyeksportować szablon, który reprezentuje bieżący stan grupy zasobów. Wyeksportowany szablon nie jest oparty na żadnym szablonie użytym do wdrożenia. Utworzony szablon będzie stanowić migawkę grupy zasobów. W wyeksportowanym szablonie zawartych jest wiele zakodowanych wartości i prawdopodobnie mniej parametrów, niż się zwykle definiuje. To pomocne rozwiązanie w przypadku, gdy grupę zasobów zmodyfikowano za pośrednictwem portalu lub przy użyciu skryptów. Będzie więc trzeba przechwycić grupę zasobów jako szablon.

W tym temacie opisano obie te metody. W artykule [Dostosowywanie wyeksportowanego szablonu usługi Azure Resource Manager](resource-manager-customize-template.md) znajdują się informacje na temat tego, jak uczynić szablon wygenerowany na podstawie bieżącego stanu grupy zasobów bardziej przydatnym do ponownego wdrożenia Twojego rozwiązania.

W tym samouczku zalogujesz się w witrynie Azure Portal, utworzysz konto magazynu i wyeksportujesz szablon dla tego konta magazynu. Dodasz również sieć wirtualną w celu zmodyfikowania grupy zasobów. Na koniec wyeksportujesz nowy szablon, który reprezentuje jej bieżący stan. Chociaż ten artykuł skupia się na uproszczonej infrastrukturze, tych samych kroków można użyć do wyeksportowania szablonu dla bardziej skomplikowanego rozwiązania.

## Tworzenie konta magazynu
1. W [Portalu Azure](https://portal.azure.com) wybierz pozycję **Nowy** > **Dane i usługa Storage** > **Konto usługi Storage**.
   
      ![tworzenie magazynu](./media/resource-manager-export-template/create-storage.png)
2. Utwórz konto magazynu o nazwie **magazyn** z Twoimi inicjałami i aktualną datą. Nazwa konta magazynu musi być unikatowa w obrębie platformy Azure. Jeśli na początku podasz nazwę, która jest już używana, spróbuj podać inny wariant tej nazwy. Dla grupy zasobów użyj wartości **ExportGroup**. Dla innych właściwości możesz użyć wartości domyślnych. Wybierz pozycję **Utwórz**.
   
      ![podawanie wartości magazynu](./media/resource-manager-export-template/provide-storage-values.png)

Po zakończeniu wdrożenia Twoja subskrypcja będzie zawierać konto magazynu.

## Eksportowanie szablonu z historii wdrożenia
1. Przejdź do bloku grupy zasobów dla nowej grupy zasobów. Zwróć uwagę, że blok ten zawiera wynik ostatniego wdrożenia. Wybierz ten link.
   
      ![blok grupy zasobów](./media/resource-manager-export-template/resource-group-blade.png)
2. Zostanie wyświetlona historia wdrożeń dla grupy. W Twoim przypadku w bloku będzie prawdopodobnie wyświetlone tylko jedno wdrożenie. Wybierz to wdrożenie.
   
     ![ostatnie wdrożenie](./media/resource-manager-export-template/last-deployment.png)
3. W tym bloku jest wyświetlone podsumowanie wdrożenia. Podsumowanie zawiera stan wdrożenia i jego operacji oraz wartości podanych parametrów. Aby wyświetlić szablon, który został użyty do wdrożenia, wybierz pozycję **Wyświetl szablon**.
   
     ![wyświetlanie podsumowania wdrożenia](./media/resource-manager-export-template/deployment-summary.png)
4. Usługa Resource Manager pobiera następujące sześć plików:
   
   1. **Szablon** — szablon, który definiuje infrastrukturę Twojego rozwiązania. Po utworzeniu konta magazynu za pośrednictwem portalu usługa Resource Manager użyła szablonu w celu jego wdrożenia i zapisała ten szablon do użytku w przyszłości.
   2. **Parametry** — plik parametrów, który służy do przekazywania wartości podczas wdrażania. Zawiera wartości podane podczas pierwszego wdrażania, które można dowolnie zmieniać podczas ponownego wdrażania szablonu.
   3. **Interfejs wiersza polecenia** — plik skryptu interfejsu wiersza polecenia platformy Azure, którego możesz użyć do wdrożenia szablonu.
   4. **PowerShell** — plik skryptu programu Azure PowerShell, którego możesz użyć do wdrożenia szablonu.
   5. **.NET** — klasa platformy .NET, której możesz użyć do wdrożenia szablonu.
   6. **Ruby** — klasa języka Ruby, której możesz użyć do wdrożenia szablonu.
      
      Pliki te są dostępne za pośrednictwem linków w obrębie bloku. Domyślnie w bloku wyświetlany jest szablon.
      
       ![wyświetlanie szablonu](./media/resource-manager-export-template/view-template.png)
      
      Zwróćmy teraz szczególną uwagę na szablon. Szablon powinien być podobny do tego wyświetlonego poniżej:
      
        {     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",     "contentVersion": "1.0.0.0",     "parameters": {       "name": {         "type": "String"       },       "accountType": {         "type": "String"       },       "location": {         "type": "String"       },       "encryptionEnabled": {         "defaultValue": false,         "type": "Bool"       }     },     "resources": [       {         "type": "Microsoft.Storage/storageAccounts",         "sku": {           "name": "[parameters('accountType')]"         },         "kind": "Storage",         "name": "[parameters('name')]",         "apiVersion": "2016-01-01",         "location": "[parameters('location')]",         "properties": {           "encryption": {             "services": {               "blob": {                 "enabled": "[parameters('encryptionEnabled')]"               }             },             "keySource": "Microsoft.Storage"           }         }       }     ]   }

To prawdziwy szablon użyty do utworzenia Twojego konta magazynu. Zwróć uwagę, że zawiera on parametry umożliwiające wdrażanie różnych typów kont magazynu. Aby uzyskać więcej informacji o strukturze szablonu, zobacz [Tworzenie szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md). Aby uzyskać pełną listę funkcji, których można użyć w szablonie, zobacz [Azure Resource Manager template functions](resource-group-template-functions.md) (Funkcje szablonu usługi Azure Resource Manager).

## Dodawanie sieci wirtualnej
Szablon pobrany w poprzedniej sekcji reprezentował infrastrukturę dla tego oryginalnego wdrożenia. Nie będzie on jednak uwzględniać żadnych zmian wprowadzonych po wdrożeniu.
Aby zilustrować ten problem, zmodyfikujmy grupę zasobów przez dodanie sieci wirtualnej za pośrednictwem portalu.

1. W bloku grupy zasobów wybierz pozycję **Dodaj**.
   
      ![dodawanie zasobu](./media/resource-manager-export-template/add-resource.png)
2. Z dostępnych zasobów wybierz pozycję **Sieć wirtualna**.
   
      ![wybieranie sieci wirtualnej](./media/resource-manager-export-template/select-vnet.png)
3. Sieci wirtualnej nadaj nazwę **VNET**, a dla innych właściwości użyj wartości domyślnych. Wybierz pozycję **Utwórz**.
   
      ![ustawianie alertu](./media/resource-manager-export-template/create-vnet.png)
4. Po pomyślnym wdrożeniu sieci wirtualnej w grupie zasobów spójrz ponownie na historię wdrażania. Są w niej teraz wyświetlone dwa wdrożenia. Jeśli drugie wdrożenie nie jest widoczne, może być konieczne zamknięcie i ponowne otwarcie bloku grupy zasobów. Wybierz nowsze wdrożenie.
   
      ![historia wdrażania](./media/resource-manager-export-template/deployment-history.png)
5. Spójrz na szablon tego wdrożenia. Zwróć uwagę, że definiuje on tylko zmiany wprowadzone w celu dodania sieci wirtualnej.

Zazwyczaj najlepszym rozwiązaniem jest praca z szablonem, za pomocą którego wdrażana jest cała infrastruktura dla danego rozwiązania w ramach jednej operacji. To bardziej niezawodne podejście niż praca z wieloma różnymi szablonami służącymi do przeprowadzenia wdrażania.

## Eksportowanie szablonu na podstawie grupy zasobów
Mimo że dla każdego wdrożenia wyświetlane są tylko zmiany wprowadzone w danej grupie zasobów, w dowolnym momencie możliwe jest wyeksportowanie szablonu w celu wyświetlenia atrybutów całej grupy zasobów.  

> [!NOTE]
> Nie można eksportować szablonu dla grupy zasobów zawierającej ponad 200 zasobów.
> 
> 

1. Aby wyświetlić szablon dla grupy zasobów, wybierz pozycję **Skrypt automatyzacji**.
   
      ![eksportowanie grupy zasobów](./media/resource-manager-export-template/export-resource-group.png)
   
     Nie wszystkie typy zasobów obsługują funkcję eksportowania szablonu. Jeśli dana grupa zasobów zawiera tylko konto magazynu i sieć wirtualną przedstawione w tym artykule, nie zostanie wyświetlony błąd. Jeśli jednak utworzono inne typy zasobów, możesz zobaczyć komunikat o błędzie informujący o wystąpieniu problemu z eksportowaniem. Sposoby rozwiązywania takich problemów poznasz w sekcji [Rozwiązywanie problemów z eksportowaniem](#fix-export-issues).
2. Ponownie będzie wyświetlonych sześć plików, których można użyć w celu ponownego wdrożenia rozwiązania, ale tym razem szablon jest nieco inny. Ten szablon ma tylko dwa parametry: jeden dla nazwy konta magazynu i jeden dla nazwy sieci wirtualnej.
   
        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },
   
     Usługa Resource Manager nie pobrała szablonów, które były używane podczas wdrażania. Zamiast tego usługa ta wygenerowała nowy szablon na podstawie bieżącej konfiguracji zasobów. Na przykład w szablonie ustawiono wartość lokalizacji i replikacji konta magazynu na następującą wartość:
   
        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },
3. Pobierz szablon, aby móc na nim pracować lokalnie.
   
      ![pobieranie szablonu](./media/resource-manager-export-template/download-template.png)
4. Znajdź pobrany plik zip i wyodrębnij jego zawartość. Za pomocą tego pobranego szablonu możesz ponowne wdrożyć swoją infrastrukturę.

## Rozwiązywanie problemów z eksportowaniem
Nie wszystkie typy zasobów obsługują funkcję eksportowania szablonu. Usługa Resource Manager nie eksportuje niektórych typów zasobów, aby zapobiec ujawnieniu poufnych danych. Na przykład jeśli konfiguracja witryny zawiera parametry połączenia, to najprawdopodobniej nie chcesz, aby były jawnie wyświetlane w wyeksportowanym szablonie. Ten problem można rozwiązać poprzez ręczne dodanie brakujących zasobów do szablonu.

> [!NOTE]
> Problemy związane z eksportowaniem występują tylko podczas eksportowania szablonu na podstawie grupy roboczej, a nie z historii wdrożenia. Jeśli ostatnie wdrożenie dokładnie reprezentuje bieżący stan grupy zasobów, należy wyeksportować szablon z historii wdrożenia — nie na podstawie grupy zasobów. Eksportowania na podstawie grupy zasobów dokonuje się tylko wtedy, gdy w grupie zasobów wprowadzono zmiany, które nie są zdefiniowane w pojedynczym szablonie.
> 
> 

Na przykład jeśli eksportujesz szablon dla grupy zasobów zawierającej aplikację sieci Web, bazę danych SQL i parametry połączenia w konfiguracji witryny, zostanie wyświetlony następujący komunikat.

![wyświetlany błąd](./media/resource-manager-export-template/show-error.png)

Wybranie komunikatu spowoduje wyświetlenie dokładnych typów zasobów, które nie zostały wyeksportowane. 

![wyświetlany błąd](./media/resource-manager-export-template/show-error-details.png)

W tym temacie przedstawiono następujące typowe poprawki. Aby zaimplementować te zasoby, należy dodać parametry do szablonu. Aby uzyskać więcej informacji, zobacz [Dostosowywanie i ponowne wdrażanie wyeksportowanego szablonu](resource-manager-customize-template.md).

### Parametry połączenia
W zasobie witryny sieci Web dodaj do bazy danych definicję parametrów połączenia:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### Rozszerzenie witryny sieci Web
W zasobie witryny sieci Web dodaj definicję na potrzeby zainstalowania kodu:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### Rozszerzenie maszyny wirtualnej
Przykłady rozszerzeń maszyny wirtualnej można znaleźć w artykule [Azure Windows VM Extension Configuration Samples](virtual-machines/virtual-machines-windows-extensions-configuration-samples.md) (Przykłady konfiguracji rozszerzeń Azure dla maszyny wirtualnej systemu Windows).

### Brama sieci wirtualnej
Dodaj jako typ zasobu bramę sieci wirtualnej.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### Brama sieci lokalnej
Dodaj jako typ zasobu bramę sieci lokalnej.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### Połączenie
Dodaj połączenie jako typ zasobu.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## Następne kroki
Gratulacje! Wiesz już, jak wyeksportować szablon na podstawie zasobów, które zostały utworzone w portalu.

* W drugiej części tego samouczka dostosujesz pobrany szablon, dodając większą liczbę parametrów i wdrażając go ponownie za pomocą skryptu. Zobacz [Dostosowywanie i ponowne wdrażanie wyeksportowanego szablonu](resource-manager-customize-template.md).
* Aby poznać sposób eksportowania szablonu za pomocą programu PowerShell, zobacz [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Azure Resource Manager).
* Aby poznać sposób eksportowania szablonu za pomocą interfejsu wiersza polecenia platformy Azure, zobacz [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager).

<!--HONumber=Sep16_HO4-->


