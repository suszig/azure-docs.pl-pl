<properties
    pageTitle="Eksportowanie szablonu usługi Azure Resource Manager | System Microsoft Azure"
    description="Za pomocą usługi Azure Resource Manager można wyeksportować szablon z istniejącej grupy zasobów."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/10/2016"
    ms.author="tomfitz"/>

# Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów

Zrozumienie sposobu tworzenia szablonów usługi Azure Resource Manager może być czasochłonnym zadaniem. Na szczęście usługa Resource Manager ułatwia wykonanie tego zadania, ponieważ umożliwia wyeksportowanie szablonu z istniejących zasobów w Twojej subskrypcji. Możesz użyć wygenerowanego szablonu, aby dowiedzieć się więcej o składni szablonu lub aby zautomatyzować ponowne wdrożenie rozwiązania, w razie potrzeby.

W tym samouczku zalogujesz się do Portalu Azure, utworzysz konto magazynu i wyeksportujesz szablon dla tego konta magazynu. Dodasz również sieć wirtualną w celu zmodyfikowania grupy zasobów. Na koniec wyeksportujesz nowy szablon, który reprezentuje jej bieżący stan. Chociaż ten artykuł skupia się na uproszczonej infrastrukturze, tych samych kroków można użyć do wyeksportowania szablonu dla bardziej skomplikowanego rozwiązania.

## Tworzenie konta magazynu

1. W [Portalu Azure](https://portal.azure.com) wybierz pozycję **Nowy** > **Dane i usługa Storage** > **Konto usługi Storage**.

      ![tworzenie magazynu](./media/resource-manager-export-template/create-storage.png)

2. Utwórz konto magazynu o nazwie **magazyn** z Twoimi inicjałami i aktualną datą. Nazwa konta magazynu musi być unikatowa w obrębie platformy Azure. Jeśli na początku podasz nazwę, która jest już używana, spróbuj podać inny wariant tej nazwy. Dla grupy zasobów użyj wartości **ExportGroup**. Dla innych właściwości możesz użyć wartości domyślnych. Wybierz pozycję **Utwórz**.

      ![podawanie wartości magazynu](./media/resource-manager-export-template/provide-storage-values.png)

Po zakończeniu wdrożenia Twoja subskrypcja będzie zawierać konto magazynu.

## Eksportowanie szablonu na potrzeby wdrożenia

1. Przejdź do bloku grupy zasobów dla nowej grupy zasobów. Można będzie zauważyć, że podany jest wynik ostatniego wdrożenia. Wybierz ten link.

      ![blok grupy zasobów](./media/resource-manager-export-template/resource-group-blade.png)

2. Zostanie wyświetlona historia wdrożeń dla grupy. W Twoim przypadku będzie wyświetlone prawdopodobnie tylko jedno wdrożenie. Wybierz to wdrożenie.

     ![ostatnie wdrożenie](./media/resource-manager-export-template/last-deployment.png)

3. Zostanie wyświetlone podsumowanie wdrożenia. Podsumowanie zawiera stan wdrożenia i jego operacji oraz wartości podanych parametrów. Aby wyświetlić szablon, który został użyty do wdrożenia, wybierz pozycję **Wyświetl szablon**.

     ![wyświetlanie podsumowania wdrożenia](./media/resource-manager-export-template/deployment-summary.png)

4. Usługa Resource Manager pobiera następujące pięć plików:

   - Szablon, który definiuje infrastrukturę Twojego rozwiązania. Po utworzeniu konta magazynu za pośrednictwem portalu usługa Resource Manager użyła szablonu w celu jego wdrożenia i zapisała ten szablon do użytku w przyszłości.

   - Plik parametrów, który służy do przekazywania wartości podczas wdrażania. Zawiera wartości podane podczas pierwszego wdrażania, które można dowolnie zmieniać podczas ponownego wdrażania szablonu.

   - Plik skryptu programu Azure PowerShell, którego możesz użyć do wdrożenia szablonu.

   - Plik skryptu interfejsu wiersza polecenia platformy Azure, którego możesz użyć do wdrożenia szablonu.

   - Klasa platformy .NET, której możesz użyć do wdrożenia szablonu.

     Pliki te są dostępne za pośrednictwem linków w obrębie bloku. Domyślnie wybrany jest szablon.

       ![wyświetlanie szablonu](./media/resource-manager-export-template/view-template.png)

     Zwróćmy teraz szczególną uwagę na szablon. Szablon powinien być podobny do tego wyświetlonego poniżej:

        {      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",      "contentVersion": "1.0.0.0",      "parameters": {        "name": {          "type": "String"        },        "accountType": {          "type": "String"        },        "location": {          "type": "String"        },        "encryptionEnabled": {          "defaultValue": false,          "type": "Bool"        }      },      "resources": [        {          "type": "Microsoft.Storage/storageAccounts",          "sku": {            "name": "[parameters('accountType')]"          },          "kind": "Storage",          "name": "[parameters('name')]",          "apiVersion": "2016-01-01",          "location": "[parameters('location')]",          "properties": {            "encryption": {              "services": {                "blob": {                  "enabled": "[parameters('encryptionEnabled')]"                }              },              "keySource": "Microsoft.Storage"            }          }        }      ]    }

     Należy zwrócić uwagę, że szablon definiuje parametry dla nazwy, typu i lokalizacji konta magazynu. Parametr wskazuje także, czy jest włączone szyfrowanie. Wartość domyślna to **false**. W sekcji **resources** zobaczysz definicję konta magazynu do wdrożenia.

Nawiasy kwadratowe zawierają wyrażenie, które jest oceniane podczas wdrażania. Wyrażenia w nawiasach kwadratowych w szablonie są używane do uzyskania wartości parametrów podczas wdrażania. Możliwe jest użycie dodatkowych wyrażeń — ich przykłady zostaną podane w dalszej części tego artykułu. Aby uzyskać pełną listę takich wyrażeń, zobacz [Funkcje szablonu usługi Azure Resource Manager](resource-group-template-functions.md).

Aby uzyskać więcej informacji o strukturze szablonu, zobacz [Tworzenie szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).

## Dodawanie sieci wirtualnej

Szablon pobrany w poprzedniej sekcji reprezentował infrastrukturę dla tego oryginalnego wdrożenia, ale nie będzie on uwzględniać żadnych zmian wprowadzonych po wdrożeniu.
Aby zilustrować ten problem, zmodyfikujmy grupę zasobów przez dodanie sieci wirtualnej za pośrednictwem portalu.

1. W bloku grupy zasobów wybierz pozycję **Dodaj**, a następnie z dostępnych zasobów wybierz **sieć wirtualną**.

2. Sieci wirtualnej nadaj nazwę **VNET**, a dla innych właściwości użyj wartości domyślnych. Wybierz pozycję **Utwórz**.

      ![ustawianie alertu](./media/resource-manager-export-template/create-vnet.png)

3. Po pomyślnym wdrożeniu sieci wirtualnej w grupie zasobów spójrz ponownie na historię wdrażania. Będą w niej teraz wyświetlone dwa wdrożenia. Wybierz nowsze wdrożenie.

      ![historia wdrażania](./media/resource-manager-export-template/deployment-history.png)

4. Spójrz na szablon tego wdrożenia. Zwróć uwagę, że definiuje on tylko zmiany wprowadzone w celu dodania sieci wirtualnej.

Zazwyczaj najlepszym rozwiązaniem jest praca z szablonem, za pomocą którego wdrażana jest cała infrastruktura dla danego rozwiązania w ramach jednej operacji, a nie praca z wieloma różnymi szablonami służącymi do przeprowadzenia wdrażania.


## Eksportowanie szablonu dla grupy zasobów

Mimo że dla każdego wdrożenia wyświetlane są tylko zmiany wprowadzone w danej grupie zasobów, w dowolnym momencie możliwe jest wyeksportowanie szablonu w celu wyświetlenia atrybutów całej grupy zasobów.  

1. Aby wyświetlić szablon dla grupy zasobów, wybierz pozycję **Eksportuj szablon**.

      ![eksportowanie grupy zasobów](./media/resource-manager-export-template/export-resource-group.png)

2. Ponownie zostanie wyświetlonych pięć plików, których można użyć w celu ponownego wdrożenia rozwiązania, ale tym razem szablon jest nieco inny. Ten szablon ma tylko dwa parametry: jeden dla nazwy konta magazynu i jeden dla nazwy sieci wirtualnej.

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

     Usługa Resource Manager nie pobrała szablonów, które były używane podczas wdrażania. Zamiast tego usługa ta wygenerowała nowy szablon na podstawie bieżącej konfiguracji zasobów. Usługa Resource Manager nie może określić wartości, które mają być przekazywane jako parametry, dlatego większość wartości jest na stałe umieszczana w kodzie i bazują one na wartościach pobranych z grupy zasobów. Na przykład wartość lokalizacji i replikacji konta magazynu ustawiono na następującą wartość:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Pobierz szablon, aby móc na nim pracować lokalnie.

      ![pobieranie szablonu](./media/resource-manager-export-template/download-template.png)

4. Znajdź pobrany plik zip i wyodrębnij jego zawartość. Za pomocą tego pobranego szablonu możesz ponowne wdrożyć swoją infrastrukturę.

## Następne kroki

Gratulacje! Wiesz już, jak wyeksportować szablon na podstawie zasobów, które zostały utworzone w portalu.

- W drugiej części tego samouczka dostosujesz pobrany właśnie szablon, dodając większą liczbę parametrów i wdrażając go ponownie za pomocą skryptu. Zobacz [Dostosowywanie i ponowne wdrażanie wyeksportowanego szablonu](resource-manager-customize-template.md).
- Aby poznać sposób eksportowania szablonu za pomocą programu PowerShell, zobacz [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Azure Resource Manager).
- Aby poznać sposób eksportowania szablonu za pomocą interfejsu wiersza polecenia platformy Azure, zobacz [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager).



<!--HONumber=Jun16_HO2-->


