---
title: Moduł programu PowerShell dla usługi Machine Learning | Microsoft Docs
description: Moduł programu PowerShell dla usługi Azure Machine Learning jest dostępny w trybie publicznej wersji zapoznawczej. Użyj programu PowerShell do tworzenia obszarów roboczych i zarządzania nimi, a także do tworzenia eksperymentów, usług sieci Web i wielu innych elementów.
keywords: experiment,linear regression,machine learning algorithms,machine learning tutorial,predictive modeling techniques,data science experiment
services: machine-learning
documentationcenter: ''
author: hning86
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/05/2016
ms.author: garye;haining

---
# Moduł programu PowerShell dla usługi Azure Machine Learning
Moduł programu PowerShell dla usługi Azure Machine Learning to zaawansowane narzędzie, które umożliwia używanie programu Windows PowerShell do zarządzania obszarami roboczymi, eksperymentami, zestawami danych, usługami sieci Web i innymi elementami.

Możesz wyświetlić dokumentację i pobrać moduł wraz z pełnym kodem źródłowym, korzystając z adresu [https://aka.ms/amlps](https://aka.ms/amlps). 

## Co to jest moduł programu PowerShell usługi Machine Learning?
Moduł programu PowerShell usługi Machine Learning to oparty na platformie .NET moduł DLL, który umożliwia pełne zarządzanie obszarami roboczymi, eksperymentami, zestawami danych, usługami sieci Web i punktami końcowymi usług sieci Web usługi Azure Machine Learning za pomocą programu Windows PowerShell. Wraz z modułem można pobrać pełny kod źródłowy zawierający klarownie oddzieloną [warstwę interfejsu API języka C#](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Oznacza to, że można odwoływać się do tej biblioteki DLL z projektu .NET i zarządzać usługą Azure Machine Learning za pomocą kodu platformy .NET. Ponadto biblioteka DLL zależy od podstawowych interfejsów API REST, które można wykorzystywać bezpośrednio z poziomu swojego ulubionego klienta.

## Co można zrobić za pomocą modułu programu PowerShell?
Oto niektóre zadania, które można wykonać za pomocą tego modułu programu PowerShell. Zapoznaj się z [pełną dokumentacją](https://aka.ms/amlps) dotyczącą tej i wielu innych funkcji.

* Aprowizacja nowego obszaru roboczego za pomocą certyfikatu zarządzania ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Eksportowanie i importowanie pliku JSON reprezentującego wykres eksperymentu ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) i [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Uruchamianie eksperymentu ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Tworzenie usługi sieci Web na podstawie eksperymentu predykcyjnego ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Tworzenie nowego punktu końcowego opublikowanej usługi sieci Web ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Wywoływanie punktu końcowego usługi sieci Web RRS i/lub BES ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) i [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Poniżej przedstawiono prosty przykład użycia programu PowerShell do uruchamiania istniejącego eksperymentu:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Aby uzyskać bardziej szczegółowy przypadek użycia, zapoznaj się z następującym artykułem dotyczącym używania modułu programu PowerShell w celu zautomatyzowania bardzo często żądanego zadania: [Create many Machine Learning models and web service endpoints from one experiment using PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md) (Tworzenie wielu modeli i punktów końcowych usługi sieci Web usługi Machine Learning na podstawie jednego eksperymentu przy użyciu programu PowerShell).

## Jak rozpocząć?
Aby rozpocząć korzystanie z programu PowerShell usługi Machine Learning, pobierz [pakiet wersji](https://github.com/hning86/azuremlps/releases) z serwisu GitHub i postępuj zgodnie z [instrukcjami instalacji](https://github.com/hning86/azuremlps/blob/master/README.md). Konieczne jest odblokowanie pobranej i rozpakowanej biblioteki DLL, a następnie zaimportowanie jej do środowiska programu PowerShell. Większość poleceń cmdlet wymaga podania identyfikatora obszaru roboczego, tokenu autoryzacji obszaru roboczego i regionu platformy Azure, w którym znajduje się obszar roboczy. Najprostszym sposobem dostarczenia takich informacji jest użycie domyślnego pliku config.json, który został szczegółowo opisany w instrukcjach instalacji. Oczywiście można również sklonować drzewo repozytorium git i zmodyfikować lub skompilować kod lokalnie przy użyciu programu Visual Studio.

## Następne kroki
Moduł PowerShell będzie stale udoskonalany i rozwijany w trakcie tego okresu korzystania z wersji zapoznawczej. Śledź [blog dotyczący pakietu Cortana Intelligence i usługi Machine Learning](https://blogs.technet.microsoft.com/machinelearning/), aby dowiadywać się o innych nowościach i uzyskiwać dodatkowe informacje.

<!--HONumber=Sep16_HO3-->


