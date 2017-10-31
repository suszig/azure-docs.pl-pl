---
title: "Interfejsy API i narzędzia dla deweloperów usługi Azure Batch | Microsoft Docs"
description: "Informacje na temat narzędzi i interfejsów API przeznaczonych do tworzenia rozwiązań korzystających z usługi Azure Batch."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 10/12/2017
ms.author: tamram
ms.openlocfilehash: a17856013c8db2e671b8f5201fbcc9223953ab6f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>Omówienie narzędzi i interfejsów API usługi Batch

Przetwarzanie obciążeń równoległych przy użyciu usługi Azure Batch jest zazwyczaj wykonywane programowo przy użyciu jednego z [interfejsów API usługi Batch](#batch-development-apis). Aplikacja lub usługa kliencka, której autorem jest użytkownik, może używać interfejsów API usługi Batch do komunikacji z usługą Batch. Za pomocą interfejsów API usługi Batch można tworzyć pule węzłów obliczeniowych (maszyny wirtualne lub usługi w chmurze) i zarządzać nimi. Następnie można zaplanować wykonywanie zadań i podzadań w ramach tych węzłów. 

Można skutecznie przetwarzać duże obciążenia na potrzeby swojej organizacji lub zapewniać swoim klientom fronton usług, aby umożliwić im uruchamianie zadań i podzadań — na żądanie lub zgodnie z harmonogramem — w jednym węźle albo setkach lub nawet tysiącach węzłów. Usługę Azure Batch można także używać w ramach większego przepływu pracy zarządzanego za pomocą takich narzędzi, jak usługa [Azure Data Factory](../data-factory/v1/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Jeśli zechcesz dokładniej zapoznać się z interfejsem API usługi Batch i uzyskać bardziej szczegółowe informacje dotyczące funkcji, które on zapewnia, przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Konta platformy Azure na potrzeby programowania w usłudze Batch
Podczas opracowywania rozwiązań usługi Batch potrzebne są następujące konta na platformie Microsoft Azure.

* **Konto i subskrypcja platformy Azure** — jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów programu Visual Studio][msdn_benefits] lub utworzyć [bezpłatne konto platformy Azure][free_account]. Podczas tworzenia konta zostanie utworzona domyślna subskrypcja.
* **Konto usługi Batch** — zasoby usługi Batch, w tym pule, węzły obliczeniowe, zadania i podzadania są skojarzone z kontem usługi Azure Batch. Gdy aplikacja wykonuje żądanie względem usługi Batch, usługa ta uwierzytelnia żądanie przy użyciu nazwy konta usługi Batch, adresu URL konta oraz klucza dostępu albo tokenu usługi Azure Active Directory. [Konto usługi Batch możesz utworzyć](batch-account-create-portal.md) w witrynie Azure Portal lub programowo.
* **Konto usługi Storage** — Usługa Batch obejmuje wbudowaną obsługę pracy z plikami w usłudze [Azure Storage][azure_storage]. Usługa Azure Blob Storage jest używana prawie w każdym scenariuszu usługi Batch — do przemieszczania programów uruchamianych przez podzadania oraz danych, które one przetwarzają, oraz do przechowywania danych wyjściowych, które generują. Aby utworzyć konto usługi Storage, zobacz temat [Informacje o kontach magazynu Azure](../storage/common/storage-create-storage-account.md)

## <a name="batch-service-apis"></a>Interfejsy API usługi Batch

Aplikacje i usługi mogą wykonywać bezpośrednie wywołania interfejsu API REST lub użyć co najmniej jednej z następujących bibliotek klienckich do uruchamiania obciążeń usługi Azure Batch i zarządzania nimi.

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu | Więcej informacji |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[docs.microsoft.com][batch_rest] |Nie dotyczy |- |- | [Obsługiwane wersje](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Samouczek](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Informacje o wersji](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Samouczek](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Plik Readme](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Samouczek](batch-nodejs-get-started.md) |- | [Plik Readme](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Plik Readme][api_sample_java] | [Plik Readme](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Interfejsy API usługi Batch Management

Interfejsy API usługi Azure Resource Manager dla usługi Batch zapewniają dostęp programowy do kont usługi Batch. Za pomocą tych interfejsów API można programowo zarządzać kontami usługi Batch, przydziałami i pakietami aplikacji.  

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu |
| --- | --- | --- | --- | --- |
| **Interfejsy REST menedżera zasobów usługi Batch** |[docs.microsoft.com][api_rest_mgmt] |Nie dotyczy |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Platforma .NET menedżera zasobów usługi Batch** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Samouczek](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Narzędzia wiersza polecenia usługi Batch

Te narzędzia wiersza polecenia zawierają te same funkcje co interfejsy API usług Batch i Batch Management: 

* [Polecenia cmdlet programu PowerShell w usłudze Batch][batch_ps]: polecenia cmdlet usługi Azure Batch w module programu [Azure PowerShell](/powershell/azure/overview) umożliwiają zarządzanie zasobami usługi Batch za pomocą programu PowerShell.
* [Interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/overview): interfejs wiersza polecenia platformy Azure (Azure CLI) jest działającym na wielu platformach zestawem narzędzi, który udostępnia polecenia powłoki do interakcji z wieloma usługami platformy Azure, w tym usługi Batch i usługi Batch Management. Zobacz [Zarządzanie zasobami usługi Batch przy użyciu interfejsu wiersza polecenia platformy Azure](batch-cli-get-started.md), aby uzyskać więcej informacji o korzystaniu z interfejsu wiersza polecenia platformy Azure z usługą Batch.

## <a name="other-tools-for-application-development"></a>Inne narzędzia do opracowywania aplikacji

Oto niektóre dodatkowe narzędzia, które mogą być przydatne do budowania i debugowania aplikacji i usług tworzonych za pomocą usługi Batch:

* [Witryna Azure Portal][portal]: możesz tworzyć, monitorować i usuwać pule oraz zadania usługi Batch w witrynie Azure Portal. Można wyświetlić informacje o stanie dla tych i innych zasobów podczas uruchamiania zadań, a nawet pobierać pliki z węzłów obliczeniowych w pulach. Na przykład podczas rozwiązywania problemów można pobrać plik `stderr.txt` zadania zakończonego niepowodzeniem. Można również pobrać pliki Remote Desktop (RDP) umożliwiające logowanie się do węzłów obliczeniowych.
* [Azure BatchLabs][batch_labs]: BatchLabs to bezpłatne, mające bogate funkcje, autonomiczne narzędzie klienta pomagające tworzyć, debugować i monitorować aplikacje usługi Azure Batch. Pobierz [pakiet instalacyjny](https://azure.github.io/BatchLabs/) dla komputerów Mac lub systemu Linux albo Windows.
* [Microsoft Azure Storage Explorer][storage_explorer]: chociaż program Storage Explorer nie jest właściwie narzędziem usługi Azure Batch, jest to kolejne przydatne narzędzie, które warto mieć podczas opracowywania i debugowania rozwiązań usługi Batch.

## <a name="additional-resources"></a>Dodatkowe zasoby

- Aby dowiedzieć się więcej o rejestrowaniu zdarzeń z aplikacji usługi Batch, zobacz [Log events for diagnostic evaluation and monitoring of Batch solutions](batch-diagnostics.md) (Rejestrowanie zdarzeń na potrzeby ewaluacji diagnostycznej i monitorowania rozwiązań usługi Batch). Aby zapoznać się z dokumentacją zdarzeń zgłaszanych przez usługę Batch, zobacz [Analiza danych usługi Batch](batch-analytics.md).
- Aby uzyskać informacje o zmiennych środowiskowych dla węzłów obliczeniowych, zobacz [Zmienne środowiskowe węzła obliczeniowego usługi Azure Batch](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Następne kroki

* Przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów) zawierający informacje kluczowe dla wszystkich osób przygotowujących się do korzystania z usługi Batch. Ten artykuł zawiera bardziej szczegółowe informacje o zasobach usługi Batch, takich jak pule, węzły i zadania oraz wielu funkcjach API, których można używać podczas kompilowania aplikacji usługi Batch.
* Przeczytaj artykuł [Get started with the Azure Batch library for .NET](batch-dotnet-get-started.md) (Wprowadzenie do biblioteki usługi Azure Batch dla platformy .NET), aby dowiedzieć się, jak używać języka C# i biblioteki usługi Batch dla środowiska .NET w celu wykonania prostego obciążenia przy użyciu popularnego przepływu pracy usługi Batch. Artykuł ten powinien być jednym z pierwszych etapów podczas nauki korzystania z usługi Batch. Dostępna jest również wersja samouczka dla [języka Python](batch-python-tutorial.md) i [Node.js](batch-nodejs-get-started.md).
* Pobierz [przykłady kodu w serwisie GitHub][github_samples], aby zobaczyć, jak kod C# i Python może współpracować z usługą Batch w celu planowania i przetwarzania przykładowych obciążeń.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/client
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /nodejs/api/overview/azure/batch
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/azurerm.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchLabs/
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
