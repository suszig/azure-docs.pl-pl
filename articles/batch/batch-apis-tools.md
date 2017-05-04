---
title: "Tworzenie dużych rozwiązań przetwarzania równoległego przy użyciu narzędzi i interfejsów API usługi Azure Batch | Microsoft Docs"
description: "Informacje na temat narzędzi i interfejsów API przeznaczonych do tworzenia rozwiązań korzystających z usługi Azure Batch."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 9d523044f5107eea9dfbba17564cc15ec05076c5
ms.lasthandoff: 04/27/2017

---


# <a name="overview-of-batch-apis-and-tools"></a>Omówienie narzędzi i interfejsów API usługi Batch

Przetwarzanie obciążeń równoległych przy użyciu usługi Azure Batch jest zazwyczaj wykonywane programowo przy użyciu jednego z [interfejsów API usługi Batch](#batch-development-apis). Aplikacja lub usługa kliencka, której autorem jest użytkownik, może używać interfejsów API usługi Batch do komunikacji z usługą Batch. Za pomocą interfejsów API usługi Batch można tworzyć pule węzłów obliczeniowych (maszyny wirtualne lub usługi w chmurze) i zarządzać nimi. Następnie można zaplanować wykonywanie zadań i podzadań w ramach tych węzłów. 

Można skutecznie przetwarzać duże obciążenia na potrzeby swojej organizacji lub zapewniać swoim klientom fronton usług, aby umożliwić im uruchamianie zadań i podzadań — na żądanie lub zgodnie z harmonogramem — w jednym węźle albo setkach lub nawet tysiącach węzłów. Usługę Azure Batch można także używać w ramach większego przepływu pracy zarządzanego za pomocą takich narzędzi, jak usługa [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Jeśli zechcesz dokładniej zapoznać się z interfejsem API usługi Batch i uzyskać bardziej szczegółowe informacje dotyczące funkcji, które on zapewnia, przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów).
> 
> 

## <a name="azure-accounts-youll-need"></a>Potrzebne konta platformy Azure
Podczas opracowywania rozwiązań usługi Batch potrzebne będą następujące konta na platformie Microsoft Azure.

* **Konto i subskrypcja platformy Azure** — Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN][msdn_benefits] lub utworzyć [bezpłatne konto platformy Azure][free_account]. Podczas tworzenia konta zostanie utworzona domyślna subskrypcja.
* **Konto usługi Batch** — zasoby usługi Batch, w tym pule, węzły obliczeniowe, zadania i podzadania są skojarzone z kontem usługi Azure Batch. Gdy aplikacja wykonuje żądanie względem usługi Batch, usługa ta uwierzytelnia żądanie przy użyciu nazwy konta usługi Batch, adresu URL konta i klucza dostępu. [Tworzenie konta usługi Batch](batch-account-create-portal.md) można wykonać w witrynie Azure Portal.
* **Konto usługi Storage** — Usługa Batch obejmuje wbudowaną obsługę pracy z plikami w usłudze [Azure Storage][azure_storage]. Usługa Azure Blob Storage jest używana prawie w każdym scenariuszu usługi Batch — do przemieszczania programów uruchamianych przez podzadania oraz danych, które one przetwarzają, oraz do przechowywania danych wyjściowych, które generują. Aby utworzyć konto usługi Storage, zobacz temat [Informacje o kontach magazynu Azure](../storage/storage-create-storage-account.md)

## <a name="batch-development-apis"></a>Interfejsy API programowania w usłudze Batch
Aplikacje i usługi mogą wykonywać bezpośrednie wywołania interfejsu API REST lub użyć co najmniej jednej z następujących bibliotek klienckich do uruchamiania obciążeń usługi Azure Batch i zarządzania nimi.

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu | Więcej informacji |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |Nie dotyczy |- |- | [Obsługiwane wersje](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Samouczek](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Informacje o wersji](https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/Client/changelog.md) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Samouczek](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Plik Readme](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Plik Readme](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Plik Readme][api_sample_java] | [Plik Readme](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-command-line-tools"></a>Narzędzia wiersza polecenia usługi Batch

Funkcjonalność zapewniana przez programistyczne interfejsy API jest również dostępna za pomocą narzędzi wiersza polecenia: 

* [Polecenia cmdlet programu PowerShell w usłudze Batch][batch_ps]: polecenia cmdlet usługi Azure Batch w module programu [Azure PowerShell](/powershell/azure/overview) umożliwiają zarządzanie zasobami usługi Batch za pomocą programu PowerShell.
* [Interfejs wiersza polecenia platformy Azure](/cli/azure/overview): interfejs wiersza polecenia platformy Azure (Azure CLI) jest działającym na wielu platformach zestawem narzędzi, który udostępnia polecenia powłoki do interakcji z wieloma usługami platformy Azure, w tym usługi Batch.

## <a name="batch-resource-management"></a>Zarządzanie zasobami usługi Batch

Interfejsy API usługi Azure Resource Manager dla usługi Batch zapewniają dostęp programowy do kont usługi Batch. Za pomocą tych interfejsów API można programowo zarządzać kontami usługi Batch, przydziałami i pakietami aplikacji.  

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu |
| --- | --- | --- | --- | --- |
| **Interfejsy REST menedżera zasobów usługi Batch** |[docs.microsoft.com][api_rest_mgmt] |Nie dotyczy |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Platforma .NET menedżera zasobów usługi Batch** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Samouczek](batch-management-dotnet.md) |[GitHub][api_sample_net] |


## <a name="batch-tools"></a>Narzędzia usługi Batch
Chociaż narzędzia te nie są wymagane do tworzenia rozwiązań, które korzystają z usługi Batch, są one przydatne podczas tworzenia i debugowania aplikacji i usług w ramach usługi Batch.

* [Witryna Azure Portal][portal]: możesz tworzyć, monitorować i usuwać pule i zadania usługi Batch w blokach usługi Batch witryny Azure Portal. Można wyświetlić informacje o stanie dla tych i innych zasobów podczas uruchamiania zadań, a nawet pobierać pliki z węzłów obliczeniowych w pulach (można na przykład pobrać plik `stderr.txt` zadania zakończonego niepowodzeniem podczas rozwiązywania problemów). Można również pobrać pliki Remote Desktop (RDP) umożliwiające logowanie się do węzłów obliczeniowych.
* [Azure Batch Explorer][batch_explorer]: narzędzie Batch Explorer zapewnia podobne funkcje zarządzania zasobami usługi Batch, co witryna Azure Portal, lecz w samodzielnej aplikacji klienckiej Windows Presentation Foundation (WPF). Jest to jedna z przykładowych aplikacji .NET usługi Batch dostępnych w serwisie [GitHub][github_samples]. Można ją skompilować przy użyciu programu Visual Studio 2015 lub nowszego i użyć jej do przeglądania zasobów konta usługi Batch i zarządzania nimi podczas tworzenia i debugowania rozwiązań usługi Batch. Wyświetlaj szczegóły dotyczące zadania, puli i podzadań, pobieraj pliki z węzłów obliczeniowych lub łącz się z węzłami zdalnie przy użyciu plików usług Remote Desktop (RDP), które można pobrać przy użyciu programu Batch Explorer.
* [Microsoft Azure Storage Explorer][storage_explorer]: chociaż program Storage Explorer nie jest właściwie narzędziem usługi Azure Batch, jest to kolejne przydatne narzędzie, które warto mieć podczas opracowywania i debugowania rozwiązań usługi Batch.

## <a name="next-steps"></a>Następne kroki

* Przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów) zawierający informacje kluczowe dla wszystkich osób przygotowujących się do korzystania z usługi Batch. Ten artykuł zawiera bardziej szczegółowe informacje o zasobach usługi Batch, takich jak pule, węzły i zadania oraz wielu funkcjach API, których można używać podczas kompilowania aplikacji usługi Batch.
* Przeczytaj artykuł [Get started with the Azure Batch library for .NET](batch-dotnet-get-started.md) (Wprowadzenie do biblioteki usługi Azure Batch dla platformy .NET), aby dowiedzieć się, jak używać języka C# i biblioteki usługi Batch dla środowiska .NET w celu wykonania prostego obciążenia przy użyciu popularnego przepływu pracy usługi Batch. Artykuł ten powinien być jednym z pierwszych etapów podczas nauki korzystania z usługi Batch. Istnieje również [wersja samouczka dla języka Python](batch-python-tutorial.md).
* Pobierz [przykłady kodu w serwisie GitHub][github_samples], aby zobaczyć, jak kod C# i Python może współpracować z usługą Batch w celu planowania i przetwarzania przykładowych obciążeń.
* Zapoznaj się ze [ścieżką szkoleniową usługi Batch][learning_path] i uzyskaj informacje na temat dostępnych zasobów pomocnych podczas uczenia się pracy z usługą Batch.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

