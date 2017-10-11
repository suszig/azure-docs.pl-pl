---
title: "Utwórz szablony wdrażania dla usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie szablonów usługi Azure Resource Manager dla usługi logic apps Zarządzanie wdrożenia i wersji"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9cfbb294010d48deaf4b4c78c6a6bcd59a387d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>Tworzenie szablonów dla usługi logic apps Zarządzanie wdrożenia i wersji

Po utworzeniu aplikacji logiki, możesz utworzyć jako szablonu usługi Azure Resource Manager.
Dzięki temu można łatwo rozmieścić aplikacji logiki do dowolnego środowiska lub grupy zasobów może być konieczne jego.
Aby uzyskać więcej informacji o szablonach usługi Resource Manager, zobacz [tworzenia szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) i [wdrażanie zasobów przy użyciu szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Szablon wdrożenia aplikacji logiki

Aplikacja logiki ma trzy podstawowe składniki:

* **Zasobów aplikacji logiki**: zawiera informacje dotyczące np. cennik planu, lokalizacji i definicji przepływu pracy.
* **Definicji przepływu pracy**: opisano kroki przepływu pracy aplikacji logiki i jak mają być wykonywane przepływu pracy aplikacji logiki aparatu.
Ta definicja można wyświetlić w aplikacji logiki **widoku kodu** okna.
W zasobie aplikacji logiki, można znaleźć tej definicji w `definition` właściwości.
* **Połączenia**: odwołuje się do oddzielania zasobów, które są bezpiecznie przechowywane metadane dotyczące połączeń łącznika, takie jak ciąg połączenia i tokenu dostępu.
W zasobie aplikacji logiki, aplikację logiki odwołuje się do tych zasobów `parameters` sekcji.

Wszystkie te elementy istniejących aplikacji logiki można wyświetlić przy użyciu narzędzia, takiego jak [Eksploratora zasobów Azure](http://resources.azure.com).

Aby utworzyć szablon aplikacji logiki do użycia z wdrożenia grupy zasobów, należy zdefiniować zasoby i parametryzacja zgodnie z potrzebami.
Na przykład jeśli wdrażasz do rozwoju, testów i środowiska produkcyjnego, prawdopodobnie chcesz użyć ciągów innego połączenia z bazą danych SQL w każdym środowisku.
Można też wdrożyć w ramach różnych subskrypcji lub grupy zasobów.  

## <a name="create-a-logic-app-deployment-template"></a>Tworzenie szablonu wdrożenia aplikacji logiki

Najprostszym sposobem Szablon wdrożenia jest prawidłowy logiki aplikacji jest użycie [Visual Studio Tools dla aplikacji logiki](logic-apps-deploy-from-vs.md).
Visual Studio tools wygenerować szablon wdrożenia nieprawidłowy, używany w żadnych subskrypcji lub lokalizacji.

Jak utworzyć szablon wdrożenia aplikacji logiki może pomóc kilka innych narzędzi.
Można tworzyć ręcznie, oznacza to, za pomocą zasobów już omówione w tym miejscu, aby utworzyć parametry zgodnie z potrzebami.
Inną opcją jest użycie [twórcy szablon aplikacji logiki](https://github.com/jeffhollan/LogicAppTemplateCreator) modułu programu PowerShell. Ten moduł open source najpierw ocenia aplikacji logiki i wszystkie połączenia go używa, a następnie generuje zasobów szablonu z wymaganych parametrów wdrożenia.
Na przykład jeśli masz aplikację logiki, który odbiera komunikat z kolejki usługi Azure Service Bus i dodaje dane do bazy danych Azure SQL, narzędzie zachowuje całą logikę aranżacji i parameterizes parametry połączenia SQL i usługi Service Bus, dzięki czemu można ustawić na wdrożenie.

> [!NOTE]
> Połączenia muszą być w tej samej grupie zasobów co aplikacji logiki.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Zainstaluj moduł programu PowerShell szablon aplikacji logiki
Najprostszym sposobem zainstalowania modułu odbywa się za pośrednictwem [galerii programu PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), za pomocą polecenia `Install-Module -Name LogicAppTemplate`.  

Ponadto można zainstalować moduł PowerShell ręcznie:

1. Pobierz najnowszą wersję [twórcy szablon aplikacji logiki](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Wyodrębnij folderu w folderze modułu programu PowerShell (zazwyczaj `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Dla modułu do pracy z dowolnej dzierżawy i subskrypcji dostęp do tokenu, zaleca się używania jej z [ARMClient](https://github.com/projectkudu/ARMClient) narzędzia wiersza polecenia.  To [wpis w blogu](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) omówiono bardziej szczegółowo ARMClient.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generuj szablon aplikacji logiki przy użyciu programu PowerShell
Po zainstalowaniu programu PowerShell, można wygenerować szablonu, za pomocą następującego polecenia:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`to identyfikator subskrypcji platformy Azure. Ten wiersz najpierw pobiera token za pośrednictwem ARMClient, dostępu, a następnie przewody ją przy użyciu skryptu programu PowerShell, a następnie tworzy szablon w pliku JSON.

## <a name="add-parameters-to-a-logic-app-template"></a>Dodawanie parametrów do szablon aplikacji logiki
Po utworzeniu szablonu aplikacji logiki, można dodać lub zmodyfikować parametrów, które mogą wymagać. Na przykład jeśli definicja zawiera identyfikator zasobów Azure funkcję lub zagnieżdżony przepływ pracy, który ma zostać umieszczony w jednym wdrożeniu, możesz dodać więcej zasobów do szablonu i parametryzacja identyfikatory zgodnie z potrzebami. To samo dotyczy wszelkie odwołania do niestandardowych interfejsów API i struktury Swagger punkty końcowe oczekiwanych do wdrożenia z każdą grupą zasobów.

## <a name="deploy-a-logic-app-template"></a>Wdróż szablon aplikacji logiki

Szablonu można wdrożyć przy użyciu dowolnego narzędzia, takie jak środowiska PowerShell, interfejsu API REST, [programu Visual Studio Team Services Release Management](#team-services)i Szablon wdrożenia za pomocą portalu Azure.
Ponadto do przechowywania wartości parametrów, zalecamy utworzenie [pliku parametrów](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Dowiedz się, jak [wdrażanie zasobów przy użyciu szablonów usługi Azure Resource Manager i programu PowerShell](../azure-resource-manager/resource-group-template-deploy.md) lub [wdrażanie zasobów przy użyciu szablonów usługi Azure Resource Manager i portalu Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>Autoryzowania połączeń OAuth

Po wdrożeniu aplikacji logiki działa end-to-end z prawidłowe parametry.
Jednak należy nadal autoryzowania połączeń OAuth do wygenerowania tokena dostępu prawidłowe.
Do autoryzowania połączeń OAuth, Otwórz aplikację logiki w Projektancie aplikacji logiki i autoryzowania tych połączeń. Lub dla automatycznego wdrażania, można za pomocą skryptu wyrażenia zgody na każde połączenie OAuth.
Przykładowy skrypt jest w witrynie GitHub pod [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projektu.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services Release Management

Typowy scenariusz wdrażania i zarządzania nimi w środowisku jest za pomocą narzędzia, takiego jak Zarządzanie zleceniami w programie Visual Studio Team Services Szablon wdrożenia aplikacji logiki. Visual Studio Team Services zawiera [wdrażanie grupy zasobów platformy Azure](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) zadań można dodać do dowolnego kompilacji lub zwolnij potoku. Musisz mieć [nazwy głównej usługi](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) dla autoryzacji do wdrożenia, a następnie można wygenerować definicji wersji.

1. Release Management wybierz **pusty** tak, aby utworzyć pustą definicję.

    ![Utwórz pustą definicję][1]

2. Wybierz wszystkie zasoby potrzebne w tym celu najprawdopodobniej tym szablon aplikacji logiki, generowany ręcznie lub jako część procesu kompilacji.
3. Dodaj **wdrożenie grupy zasobów Azure** zadań.
4. Konfigurowanie przy użyciu [nazwy głównej usługi](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)i plików szablonu i parametrów szablonu.
5. Kontynuować tworzenie czynności w procesie wersji dla dowolnego środowiska, testów automatycznych lub osób zatwierdzających zgodnie z potrzebami.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
