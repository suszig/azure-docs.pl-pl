
# [Dokumentacja usługi Batch](index.yml)

# Omówienie
## [Co to jest Azure Batch?](batch-technical-overview.md)

# Przewodniki Szybki start
## [Run a Batch job - CLI (Uruchamianie zadania usługi Batch — interfejs wiersza polecenia)](quick-create-cli.md)
## [Run a Batch job - Portal (Uruchamianie zadania usługi Batch — witryna Portal)](quick-create-portal.md)
## [Run a Batch job - .NET (Uruchamianie zadania usługi Batch — .NET)](quick-run-dotnet.md)
## [Run a Batch job - Python (Uruchamianie zadania usługi Batch — Python)](quick-run-python.md)


# Samouczki

## [Parallel file processing - .NET (Równoległe przetwarzanie plików — .NET)](tutorial-parallel-dotnet.md)
## [Parallel file processing - Python (Równoległe przetwarzanie plików — Python)](tutorial-parallel-python.md)
## [Scene rendering with Arnold (Renderowanie scen za pomocą aplikacji Arnold)](tutorial-rendering-cli.md)
## [Parallel R simulation (Równoległa symulacja w środowisku R)](tutorial-r-doazureparallel.md)

# Przykłady
## Przykłady kodu
### [Przykłady kodu platformy Azure](https://azure.microsoft.com/resources/samples/?service=batch)
### [Batch samples repo (Repozytorium przykładów usługi Batch)](https://github.com/Azure/azure-batch-samples)
## [Interfejs wiersza polecenia platformy Azure](cli-samples.md)

# Pojęcia
## [Developer features (Funkcje dla deweloperów)](batch-api-basics.md)
## [Interfejsy API i narzędzia](batch-apis-tools.md)
## [Limity przydziału i ograniczenia](batch-quota-limit.md)

# Przewodniki z instrukcjami
## Zarządzanie kontami usługi Batch
### [Manage Batch accounts with the portal (Zarządzanie kontami usługi Batch przy użyciu portalu)](batch-account-create-portal.md)
### [Zarządzanie kontami usługi Batch przy użyciu funkcji zarządzania partiami platformy .NET](batch-management-dotnet.md)

## Uwierzytelnianie za pomocą usługi Azure AD
### [Usługa Azure AD z usługą Batch](batch-aad-auth.md)
### [Usługa Azure AD z zarządzaniem usługą Batch](batch-aad-auth-management.md)

## [Create application packages (Tworzenie pakietów aplikacji)](batch-application-packages.md)

## Tworzenie pul i zarządzanie nimi
### [Automatyczne skalowanie węzłów obliczeniowych](batch-automatic-scaling.md)
### [Maszyny wirtualne intensywnie korzystające z mocy obliczeniowej](batch-pool-compute-intensive-sizes.md)
### [Konfigurowanie dostępu do węzłów obliczeniowych](pool-endpoint-configuration.md)
### [Tworzenie puli za pomocą niestandardowego obrazu](batch-custom-images.md)
### [Tworzenie puli w sieci wirtualnej](batch-virtual-network.md)
### [Węzły obliczeniowe z systemem Linux](batch-linux-nodes.md)
### [Maszyny wirtualne o niskim priorytecie](batch-low-pri-vms.md)

## Zarządzanie zadaniami i zadaniami podrzędnymi
### [Zadania związane z przygotowywaniem i kończeniem prac](batch-job-prep-release.md)
### [Równoczesne zadania w węzłach](batch-parallel-node-tasks.md)
### [Liczba zadań według stanu](batch-get-task-counts.md)
### [Zależności zadań podrzędnych](batch-task-dependencies.md)
### [Konta użytkowników do uruchamiania zadań](batch-user-accounts.md)
### [Run Batch jobs without code (Uruchamianie zadań wsadowych bez kodu)](batch-cli-templates.md)


## [Utrwalanie danych wyjściowych zadań i zadań podrzędnych](batch-task-output.md)
### [Utrwalanie danych wyjściowych za pomocą interfejsu API usługi Batch](batch-task-output-files.md)
### [Utrwalanie danych wyjściowych za pomocą biblioteki konwencji plików](batch-task-output-file-conventions.md)

## Uruchamianie obciążeń
### [MPI jobs (Zadania MPI)](batch-mpi.md)
### [Obciążenia kontenera](batch-docker-container-workloads.md)
### [Renderowanie](batch-rendering-service.md)
### [Przetwarzanie danych za pomocą usług Batch i Data Factory](../data-factory/v1/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json)

## Monitorowanie zasobów
### [Rejestrowanie diagnostyczne](batch-diagnostics.md)
### [Wydajne zapytania dotyczące list](batch-efficient-list-queries.md)

## Korzystanie z narzędzi deweloperskich
### [Use Batch .NET (Korzystanie z usługi Batch — .NET)](batch-dotnet-get-started.md)
### [Use Batch Python (Korzystanie z usługi Batch — Python)](batch-python-tutorial.md)
### [Use Batch Node.js (Korzystanie z usługi Batch — Node.js)](batch-nodejs-get-started.md)
### [Use PowerShell cmdlets (Korzystanie z poleceń cmdlet programu PowerShell)](batch-powershell-cmdlets-get-started.md)
### [Korzystanie z interfejsu wiersza polecenia platformy Azure](batch-cli-get-started.md)

# Dokumentacja

## [Interfejs wiersza polecenia platformy Azure](/cli/azure/batch)
## [Azure PowerShell](/powershell/module/azurerm.batch)
## [.NET](/dotnet/api/overview/azure/batch)
## [Java](/java/api/overview/azure/batch)
## [Node.js](/javascript/api/overview/azure/batch)
## [Python](/python/api/overview/azure/batch)
## REST
### [Usługa Batch](/rest/api/batchservice)
### [Batch Management (Zarządzanie usługą Batch)](/rest/api/batchmanagement/)
## [Analiza danych usługi Batch](batch-analytics.md)
### [Zdarzenie utworzenia puli](batch-pool-create-event.md)
### [Zdarzenie rozpoczęcia usuwania puli](batch-pool-delete-start-event.md)
### [Zdarzenie zakończenia usuwania puli](batch-pool-delete-complete-event.md)
### [Zdarzenie rozpoczęcia zmiany rozmiaru puli](batch-pool-resize-start-event.md)
### [Zdarzenie zakończenia zmiany rozmiaru puli](batch-pool-resize-complete-event.md)
### [Zdarzenie rozpoczęcia zadania](batch-task-start-event.md)
### [Zdarzenie ukończenia zadania](batch-task-complete-event.md)
### [Zdarzenie niepowodzenia zadania](batch-task-fail-event.md)
## [Zmienne środowiskowe węzła obliczeniowego usługi Batch](batch-compute-node-environment-variables.md)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/)
## [Forum usługi Batch](https://social.msdn.microsoft.com/Forums/en-us/home?forum=azurebatch)
## [Usługa Batch Shipyard](https://github.com/Azure/batch-shipyard)
## [Blog](https://blogs.technet.microsoft.com/windowshpc/)
## [HPC solutions in Azure (Rozwiązania HPC na platformie Azure)](../virtual-machines/linux/high-performance-computing.md?toc=%2fazure%2fbatch%2ftoc.json)
## [Cennik](https://azure.microsoft.com/pricing/details/batch/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Szablon usługi Resource Manager](/azure/templates/microsoft.batch/batchaccounts)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=batch&updatetype=&platform=)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-batch)
## [Wideo](https://azure.microsoft.com/documentation/videos/index/?services=batch)


