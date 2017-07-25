---
title: "Eksportowanie szablonu usługi Azure Resource Manager | Microsoft Docs"
description: "Za pomocą usługi Azure Resource Manager można wyeksportować szablon z istniejącej grupy zasobów."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1801ef47e5b182e0bcd5b23970a2999633b4a852
ms.contentlocale: pl-pl
ms.lasthandoff: 07/08/2017


---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów
W tym artykule opisano sposób eksportowania szablonu usługi Resource Manager z istniejących zasobów w ramach subskrypcji. Wygenerowany szablon umożliwi lepsze zrozumienie składni szablonu.

Istnieją dwa sposoby eksportowania szablonu:

* Możesz wyeksportować **szablon, który faktycznie został użyty na potrzeby wdrożenia**. W wyeksportowanym szablonie wszystkie parametry i zmienne występują dokładnie tak, jak w oryginalnym szablonie. Takie podejście jest przydatne, jeśli zasoby zostały wdrożone za pośrednictwem portalu i chcesz zobaczyć szablon, na podstawie którego utworzono te zasoby. Ten szablon jest gotowy do użycia. 
* Możesz wyeksportować **wygenerowany szablon, który reprezentuje bieżący stan grupy zasobów**. Wyeksportowany szablon nie jest oparty na żadnym szablonie użytym do wdrożenia. Utworzony szablon będzie stanowić migawkę grupy zasobów. W wyeksportowanym szablonie zawartych jest wiele zakodowanych wartości i prawdopodobnie mniej parametrów, niż się zwykle definiuje. Takie podejście jest przydatne, jeśli grupę zasobów zmodyfikowano po wdrożeniu. Taki szablon zwykle wymaga modyfikacji, zanim będzie go można użyć.

W tym temacie opisano sposób użycia obu metod za pośrednictwem portalu.

## <a name="deploy-resources"></a>Wdrażanie zasobów
Zacznijmy od wdrożenia zasobów na platformie Azure, których będzie można użyć do wyeksportowania w postaci szablonu. Jeśli w swojej subskrypcji masz już grupę zasobów, którą chcesz wyeksportować do szablonu, możesz pominąć tę sekcję. W dalszej części tego artykułu przyjęto założenie, że została wdrożona aplikacja internetowa i rozwiązanie bazy danych SQL przedstawione w tej sekcji. Jeśli używasz innego rozwiązania, Twoje środowisko może się nieco różnić, ale procedura eksportowania szablonu jest taka sama. 

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz pozycję **Nowy**.
   
      ![wybieranie nowego elementu](./media/resource-manager-export-template/new.png)
2. Wyszukaj pozycję **Aplikacja sieci Web i baza danych SQL** i wybierz ją z dostępnych opcji.
   
      ![wyszukiwanie aplikacji internetowej i bazy danych SQL](./media/resource-manager-export-template/webapp-sql.png)

3. Wybierz pozycję **Utwórz**.

      ![wybieranie pozycji Utwórz](./media/resource-manager-export-template/create.png)

4. Podaj wartości wymagane dla aplikacji internetowej i bazy danych SQL. Wybierz pozycję **Utwórz**.

      ![podawanie wartości dla aplikacji internetowej i bazy danych SQL](./media/resource-manager-export-template/provide-web-values.png)

Wdrożenie może chwilę potrwać. Po zakończeniu wdrożenia Twoja subskrypcja będzie zawierać rozwiązanie.

## <a name="view-template-from-deployment-history"></a>Wyświetlanie szablonu z historii wdrożenia
1. Przejdź do bloku grupy zasobów dla nowej grupy zasobów. Zwróć uwagę, że blok ten zawiera wynik ostatniego wdrożenia. Wybierz ten link.
   
      ![blok grupy zasobów](./media/resource-manager-export-template/select-deployment.png)
2. Zostanie wyświetlona historia wdrożeń dla grupy. W Twoim przypadku w bloku będzie prawdopodobnie wyświetlone tylko jedno wdrożenie. Wybierz to wdrożenie.
   
     ![ostatnie wdrożenie](./media/resource-manager-export-template/select-history.png)
3. W tym bloku jest wyświetlone podsumowanie wdrożenia. Podsumowanie zawiera stan wdrożenia i jego operacji oraz wartości podanych parametrów. Aby wyświetlić szablon, który został użyty do wdrożenia, wybierz pozycję **Wyświetl szablon**.
   
     ![wyświetlanie podsumowania wdrożenia](./media/resource-manager-export-template/view-template.png)
4. Usługa Resource Manager pobiera następujące siedem plików:
   
   1. **Szablon** — szablon, który definiuje infrastrukturę Twojego rozwiązania. Po utworzeniu konta magazynu za pośrednictwem portalu usługa Resource Manager użyła szablonu w celu jego wdrożenia i zapisała ten szablon do użytku w przyszłości.
   2. **Parametry** — plik parametrów, który służy do przekazywania wartości podczas wdrażania. Zawiera on wartości, które podano podczas pierwszego wdrażania. Podczas ponownego wdrażania szablonu można zmienić dowolne z tych wartości.
   3. **Interfejs wiersza polecenia** — plik skryptu interfejsu wiersza polecenia platformy Azure, którego możesz użyć do wdrożenia szablonu.
   3. **Interfejs wiersza polecenia w wersji 2.0** — plik skryptu interfejsu wiersza polecenia platformy Azure, którego możesz użyć do wdrożenia szablonu.
   4. **PowerShell** — plik skryptu programu Azure PowerShell, którego możesz użyć do wdrożenia szablonu.
   5. **.NET** — klasa platformy .NET, której możesz użyć do wdrożenia szablonu.
   6. **Ruby** — klasa języka Ruby, której możesz użyć do wdrożenia szablonu.
      
      Pliki te są dostępne za pośrednictwem linków w obrębie bloku. Domyślnie w bloku wyświetlany jest szablon.
      
       ![wyświetlanie szablonu](./media/resource-manager-export-template/see-template.png)
      
To prawdziwy szablon użyty do utworzenia aplikacji internetowej i bazy danych SQL. Zwróć uwagę, że zawiera on parametry umożliwiające podawanie różnych wartości podczas wdrażania. Aby uzyskać więcej informacji o strukturze szablonu, zobacz [Tworzenie szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).

## <a name="export-the-template-from-resource-group"></a>Eksportowanie szablonu na podstawie grupy zasobów
Jeśli ręcznie zmieniono zasoby lub dodano zasoby w wielu wdrożeniach, szablon pobrany z historii wdrożenia nie odzwierciedla bieżącego stanu grupy zasobów. W tej sekcji pokazano, jak wyeksportować szablon, który reprezentuje bieżący stan grupy zasobów. 

> [!NOTE]
> Nie można eksportować szablonu dla grupy zasobów zawierającej ponad 200 zasobów.
> 
> 

1. Aby wyświetlić szablon dla grupy zasobów, wybierz pozycję **Skrypt automatyzacji**.
   
      ![eksportowanie grupy zasobów](./media/resource-manager-export-template/select-automation.png)
   
     Menedżer zasobów szacuje zasoby w grupie zasobów i generuje szablon dla tych zasobów. Nie wszystkie typy zasobów obsługują funkcję eksportowania szablonu. Może zostać wyświetlony komunikat o błędzie informujący, że istnieje problem z eksportem. Sposoby rozwiązywania takich problemów poznasz w sekcji [Rozwiązywanie problemów z eksportowaniem](#fix-export-issues).
2. Ponownie zobaczysz sześć plików, których możesz użyć do ponownego wdrożenia rozwiązania. Jednak tym razem szablon jest nieco inny. Zwróć uwagę, że wygenerowany szablon zawiera mniej parametrów niż szablon z poprzedniej sekcji. Ponadto wiele wartości (na przykład lokalizacji i jednostki SKU) są zakodowane na stałe w tym szablonie, zamiast przyjmować wartość parametru. Przed ponownym użyciem tego szablonu możesz go zmodyfikować, aby lepiej wykorzystać parametry. 
   
3. Istnieje kilka opcji umożliwiających dalszą pracę z tym szablonem. Szablon można pobrać i pracować nad nim lokalnie w edytorze JSON. Można też zapisać szablon w bibliotece i pracować nad nim za pośrednictwem portalu.
   
     Jeśli praca w edytorze JSON, takim jak [VS Code](https://code.visualstudio.com/) lub [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), nie sprawia Ci problemów, lepszym rozwiązaniem może być lokalne pobranie szablonu i skorzystanie z tego edytora. Aby pracować lokalnie, wybierz pozycję **Pobierz**.
   
      ![pobieranie szablonu](./media/resource-manager-export-template/download-template.png)
   
     Jeśli nie korzystasz z edytora JSON, możesz preferować edytowanie szablonu za pośrednictwem portalu. W pozostałej części tego tematu założono, że szablon został zapisany w bibliotece w portalu. Do szablonu wprowadza się jednak te same zmiany składniowe niezależnie od trybu pracy — lokalnie w edytorze JSON bądź za pośrednictwem portalu. Aby pracować za pośrednictwem portalu, wybierz pozycję **Dodaj do biblioteki**.
   
      ![dodawanie do biblioteki](./media/resource-manager-export-template/add-to-library.png)
   
     Podczas dodawania szablonu do biblioteki nadaj mu nazwę i opis. Następnie wybierz pozycję **Zapisz**.
   
     ![ustawianie wartości szablonu](./media/resource-manager-export-template/save-library-template.png)
4. Aby wyświetlić szablon zapisany w bibliotece, wybierz pozycję **Więcej usług**, wpisz ciąg **Szablony** w celu filtrowania wyników, a następnie wybierz pozycję **Szablony**.
   
      ![znajdowanie szablonów](./media/resource-manager-export-template/find-templates.png)
5. Wybierz szablon o zapisanej przez Ciebie nazwie.
   
      ![wybieranie szablonu](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a>Dostosowywanie szablonu
Wyeksportowany szablon nadaje się do utworzenia takiej samej aplikacji internetowej i bazy danych SQL dla każdego wdrożenia. Usługa Resource Manager oferuje jednak opcje, dzięki którym można wdrażać szablony ze znacznie większą elastycznością. W tym artykule przedstawiono sposób dodawania parametrów dla nazwy administratora i hasła bazy danych. W ten sam sposób możesz zwiększyć elastyczność podawania innych wartości w szablonie.

1. Aby dostosować szablon, wybierz pozycję **Edytuj**.
   
     ![wyświetlanie szablonu](./media/resource-manager-export-template/select-edit.png)
2. Wybierz szablon.
   
     ![edytowanie szablonu](./media/resource-manager-export-template/select-added-template.png)
3. Aby móc przekazać wartości, które można określać podczas wdrażania, dodaj następujące dwa parametry w sekcji **parameters** w szablonie:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. Aby móc korzystać z tych nowych parametrów, zastąp definicję serwera SQL w sekcji **resources**. Zwróć uwagę, że właściwości **administratorLogin** i **administratorLoginPassword** korzystają teraz z wartości parametrów.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Po zakończeniu edycji szablonu wybierz pozycję **OK**.
7. Wybierz przycisk **Zapisz**, aby zapisać zmiany wprowadzone w szablonie.
   
     ![zapisywanie szablonu](./media/resource-manager-export-template/save-template.png)
8. Aby ponownie wdrożyć zaktualizowany szablon, wybierz pozycję **Wdróż**.
   
     ![wdrażanie szablonu](./media/resource-manager-export-template/redeploy-template.png)
9. Podaj wartości parametrów, a następnie wybierz grupę zasobów, w której mają zostać wdrożone zasoby.


## <a name="fix-export-issues"></a>Rozwiązywanie problemów z eksportowaniem
Nie wszystkie typy zasobów obsługują funkcję eksportowania szablonu. Aby rozwiązać ten problem, ręcznie dodaj brakujące zasoby do szablonu. Komunikat o błędzie zawiera typy zasobów, których nie można wyeksportować. Znajdź ten typ zasobów w [dokumentacji szablonu](/azure/templates/). Aby na przykład ręcznie dodać bramę sieci wirtualnej, zobacz [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways) (Dokumentacja szablonu Microsoft.Network/virtualNetworkGateways).

> [!NOTE]
> Problemy związane z eksportowaniem występują tylko podczas eksportowania szablonu na podstawie grupy roboczej, a nie z historii wdrożenia. Jeśli ostatnie wdrożenie dokładnie reprezentuje bieżący stan grupy zasobów, należy wyeksportować szablon z historii wdrożenia — nie na podstawie grupy zasobów. Eksportowania na podstawie grupy zasobów dokonuje się tylko wtedy, gdy w grupie zasobów wprowadzono zmiany, które nie są zdefiniowane w pojedynczym szablonie.
> 
> 

## <a name="next-steps"></a>Następne kroki
Wiesz już, jak wyeksportować szablon na podstawie zasobów, które zostały utworzone w portalu.

* Szablon można wdrożyć przy użyciu [programu PowerShell](resource-group-template-deploy.md), [interfejsu wiersza polecenia platformy Azure](resource-group-template-deploy-cli.md) lub [interfejsu API REST](resource-group-template-deploy-rest.md).
* Aby poznać sposób eksportowania szablonu za pomocą programu PowerShell, zobacz [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Azure Resource Manager).
* Aby poznać sposób eksportowania szablonu za pomocą interfejsu wiersza polecenia platformy Azure, zobacz [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager).


