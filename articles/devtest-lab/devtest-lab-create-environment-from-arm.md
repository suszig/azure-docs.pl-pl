---
title: "Tworzenie środowisk wielu maszyn wirtualnych i PaaS zasobów przy użyciu szablonów usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia środowisk wielu maszyn wirtualnych i zasoby PaaS w usłudze Azure DevTest Labs z szablonem usługi Azure Resource Manager"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: tarcher
ms.openlocfilehash: 4e1aae6c041e4572e7e2281203f969e7649e1480
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Tworzenie środowisk wielu maszyn wirtualnych i PaaS zasobów przy użyciu szablonów usługi Azure Resource Manager

[Portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) umożliwia łatwe [Utwórz i Dodaj Maszynę wirtualną w laboratorium](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-add-vm). To działa dobrze w przypadku tworzenia jednej maszyny Wirtualnej w czasie. Jednak jeśli środowisko zawiera wiele maszyn wirtualnych, każda maszyna wirtualna musi być indywidualnie utworzony. W takich scenariuszach wielowarstwową aplikację sieci Web lub farmy programu SharePoint mechanizm jest potrzebna, aby umożliwić tworzenie wielu maszyn wirtualnych w ramach jednego kroku. Przy użyciu szablonów usługi Azure Resource Manager, można definiować infrastrukturze i konfiguracji rozwiązania Azure i wielokrotnie wdrażać wiele maszyn wirtualnych w spójnym stanie. Ta funkcja zapewnia następujące korzyści:

- Szablony usługi Azure Resource Manager są ładowane bezpośrednio z Twoim repozytorium kontroli źródła (GitHub lub zespołu usługi Git).
- Po skonfigurowaniu użytkowników można utworzyć środowisko, wybierając szablon usługi Azure Resource Manager z portalu Azure jako współpracujemy z innych typów [maszyny Wirtualnej podstawy](./devtest-lab-comparing-vm-base-image-types.md).
- W środowisku z szablonem usługi Azure Resource Manager oprócz maszyny wirtualne IaaS można zainicjować obsługi administracyjnej zasobów PaaS platformy Azure.
- Koszt środowisk można śledzić w laboratorium, oprócz poszczególnych maszyn wirtualnych utworzonych przez inne typy zasad.
- PaaS zasoby są tworzone i będą widoczne w koszt śledzenia; jednak maszyna wirtualna automatycznego zamykania nie ma zastosowania do zasobów typu PaaS.
- Użytkownicy mają tej samej kontroli zasad maszyny Wirtualnej dla środowisk mają dla maszyn wirtualnych z jednego laboratorium.

Dowiedz się więcej o wielu [zalet przy użyciu szablonów usługi Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) można wdrożyć, zaktualizować lub usunąć wszystkich zasobów laboratorium w ramach jednej operacji.

> [!NOTE]
> Gdy używasz szablonu usługi Resource Manager jako podstawy do tworzenia laboratorium więcej maszyn wirtualnych istnieją pewne różnice, które należy wziąć pod uwagę, czy podczas tworzenia maszyn wirtualnych na wielu lub maszyn wirtualnych na jednym. Użyj maszyny wirtualnej tych różnic bardziej szczegółowo opisano szablonu usługi Azure Resource Manager.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Skonfiguruj repozytoria szablonu usługi Azure Resource Manager

Jako jeden z najlepszych rozwiązań z infrastruktury jako kod i kod jako konfiguracji środowiska szablony mają być zarządzane w kontroli źródła. Azure DevTest Labs następuje takie rozwiązanie i ładuje wszystkie szablony usługi Azure Resource Manager bezpośrednio z repozytoriami GitHub lub VSTS Git. W związku z tym Menedżer zasobów szablony mogą być używane w cyklu całej wersji ze środowiska testowego do środowiska produkcyjnego.

Istnieje kilka reguł, aby wykonać, aby uporządkować usługi Azure Resource Manager w repozytorium:

- Plik szablonu musi mieć nazwę `azuredeploy.json`. 

    ![Pliki szablonów klucza usługi Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Jeśli chcesz użyć wartości parametrów zdefiniowanych w pliku parametrów, musi mieć nazwę pliku parametrów `azuredeploy.parameters.json`.
- Można używać parametrów `_artifactsLocation` i `_artifactsLocationSasToken` do skonstruowania wartości identyfikatora URI parametersLink, dzięki czemu DevTest Labs automatycznie zarządzać zagnieżdżone szablony. Zobacz [jak Azure DevTest Labs ułatwia zagnieżdżonych Menedżera zasobów szablonu wdrożenia dla środowisk testowych](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) Aby uzyskać więcej informacji.
- Aby określić nazwę wyświetlaną szablonu i opis można zdefiniować metadanych. Te metadane musi znajdować się w pliku o nazwie `metadata.json`. Następujący przykładowy plik metadanych ilustruje sposób określ nazwę wyświetlaną i opis: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of the template>"
 
}
```

Następujące kroki prowadzące przez dodawanie repozytorium do laboratorium przy użyciu portalu Azure. 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
1. Z listy labs wybierz żądany laboratorium.   
1. W bloku laboratorium, wybierz **konfiguracji i zasadach**.

    ![Konfiguracja i zasady](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Z **konfiguracji i zasadach** ustawienia z listy wybierz **repozytoria**. **Repozytoria** bloku listy repozytoriów, które zostały dodane do laboratorium. Repozytorium o nazwie `Public Repo` jest generowane automatycznie dla wszystkich laboratoriów i łączy się z [repozytorium DevTest Labs GitHub](https://github.com/Azure/azure-devtestlab) zawierający kilku artefaktów maszyny Wirtualnej do użycia.

    ![Publicznego repozytorium](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Wybierz **Dodaj +** do dodania do repozytorium szablonu usługi Azure Resource Manager.
1. Gdy drugi **repozytoria** zostanie otwarty blok, wprowadź niezbędne informacje w następujący sposób:
    - **Nazwa** — wprowadź nazwę repozytorium, która jest używana w środowisku laboratoryjnym.
    - **Adres URL klonowania Git** — wprowadź adres URL klonowania GIT HTTPS z usługi GitHub lub Visual Studio Team Services.  
    - **Gałąź** — wprowadź nazwę gałęzi, aby uzyskiwać dostęp do definicji szablonu usługi Azure Resource Manager. 
    - **Osobisty token dostępu** -osobisty token dostępu jest używany do bezpiecznego dostępu do repozytorium. Aby uzyskać token z Visual Studio Team Services, wybierz  **&lt;twojanazwa >> Mój profil > Zabezpieczenia > token dostępu publicznego**. Aby uzyskać token z serwisu GitHub, wybierz Awatar, a następnie wybierając **Ustawienia > token dostępu publicznego**. 
    - **Ścieżka folderu** — przy użyciu jednej z dwóch pól wejściowych, wprowadź ścieżkę folderu, który rozpoczyna się od ukośnika - i - i jest określana względem programu Git clone URI jednej definicji artefaktów (pierwsze pole wejściowe) lub do definicji szablonu usługi Azure Resource Manager .   
    
        ![Publicznego repozytorium](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Gdy wszystkie wymagane pola są wprowadzane i przeszedł sprawdzania poprawności, wybierz **zapisać**.

Następna sekcja przeprowadzi Cię przez proces tworzenia środowisk z szablonem usługi Azure Resource Manager.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Utwórz środowisko na podstawie szablonu usługi Azure Resource Manager

Repozytorium szablonu usługi Azure Resource Manager został skonfigurowany w laboratorium, użytkownicy laboratorium tworzyć środowiska przy użyciu portalu Azure następujące czynności:

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
1. Z listy labs wybierz żądany laboratorium.   
1. W bloku laboratorium, wybierz **Dodaj +**.
1. **Wybierz podstawowej** bloku wyświetla obrazy podstawowe korzystania z szablonów usługi Azure Resource Manager wyświetlana na początku listy. Wybierz odpowiedniego szablonu usługi Azure Resource Manager.

    ![Wybierz podstawowej](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Na **Dodaj** bloku, wprowadź **Nazwa środowiska** wartość. Nazwa środowiska to, co jest wyświetlane użytkownikom w laboratorium. Pozostałe pola wejściowe są definiowane w szablonie usługi Azure Resource Manager. Jeśli wartości domyślne są zdefiniowane w szablonie lub `azuredeploy.parameter.json` plik jest obecny, wartości domyślne są wyświetlane w tych pól wejściowych. Dla parametrów typu *bezpieczny ciąg*, można użyć hasła przechowywane w laboratorium [magazynie osobistym tajny](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Dodawanie bloku](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Istnieje kilka wartości parametrów, które — nawet wtedy, gdy określono — są wyświetlane jako wartości puste. W związku z tym jeśli użytkownicy przypisać te wartości parametrów szablonu usługi Azure Resource Manager, DevTest Labs nie zawiera wartości. Zamiast tego pokazujący puste pola wejściowe gdzie użytkownikami laboratorium należy wprowadzić wartość podczas tworzenia środowiska.
    > 
    > - GEN UNIKATOWY
    > - GEN - UNIKATOWY — [N]
    > - GEN--PUB-KLUCZA SSH
    > - GEN HASŁA 
 
1. Wybierz **Dodaj** można utworzyć środowiska. Środowisko rozpoczyna się natychmiast dostarczania z wyświetlanie stanu w **Moje maszyny wirtualne** listy. Nowa grupa zasobów jest tworzony automatycznie przez laboratorium w celu zapewnienia wszystkich zasobów, które są zdefiniowane w szablonie usługi Azure Resource Manager.
1. Po utworzeniu środowiska, wybierz środowisko w **Moje maszyny wirtualne** listy, aby otworzyć blok grupy zasobów i Przeglądaj wszystkie zasoby udostępniane w środowisku.
    
    ![Lista maszyn wirtualnych](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Można również rozwinąć środowiska, aby wyświetlić tylko listę maszyn wirtualnych, które są udostępniane w środowisku.
    
    ![Lista maszyn wirtualnych](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Kliknij dowolny z tych środowisk, aby wyświetlić dostępne akcje — takie jak stosowanie artefakty dołączanie dysków z danymi, zmiana czasu automatycznego zamykania i inne.

    ![Akcje środowiska](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Następne kroki
* Po utworzeniu maszyny Wirtualnej można Połącz się z maszyną Wirtualną, wybierając **Connect** w bloku maszyny Wirtualnej.
* Wyświetlanie i zarządzanie zasobami w środowisku, wybierając środowisko w **Moje maszyny wirtualne** listy w laboratorium. 
* Eksploruj [szablonów usługi Azure Resource Manager z galerii szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates)
