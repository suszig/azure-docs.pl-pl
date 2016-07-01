<properties
    pageTitle="Role Sieć Web i Proces roboczy języka Python z programem Python Tools 2.2 for Visual Studio | Microsoft Azure"
    description="Omówienie sposobu używania programu Python Tools for Visual Studio do tworzenia usług w chmurze platformy Azure, w tym ról Sieć Web i Proces roboczy."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/30/2015"
    ms.author="adegeo"/>




# Role Sieć Web i Proces roboczy języka Python z programem Python Tools 2.2 for Visual Studio

Ten artykuł zawiera omówienie sposobu użycia ról Sieć Web i Proces roboczy języka Python za pomocą programu [Python Tools for Visual Studio][].

## Wymagania wstępne

 - Program Visual Studio w wersji 2013 lub 2015
 - Program [Python Tools 2.2 for Visual Studio][] (PTVS)
 - Program [Azure SDK Tools for VS 2013][] lub [Azure SDK Tools for VS 2015][]
 - [32-bitowe środowisko Python w wersji 2.7][] lub [32-bitowe środowisko Python w wersji 3.4][]

[AZURE.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Co to są role Sieć Web i Proces roboczy języka Python?

Platforma Azure udostępnia trzy modele obliczeniowe na potrzeby uruchamiania aplikacji: [funkcja Web Apps w usłudze App Service][execution model-web sites], [usługa Azure Virtual Machines][execution model-vms] i [usługi Azure Cloud Services][execution model-cloud services]. Wszystkie trzy modele obsługują język Python. Usługi Cloud Services, które obejmują role Sieć Web i Proces roboczy, udostępniają rozwiązanie typu *Platforma jako usługa (Platform as a Service, PaaS)*. W ramach usługi w chmurze rola Sieć Web zapewnia dedykowany serwer sieci Web usług Internet Information Services (IIS), podczas gdy rola Proces roboczy może uruchamiać asynchroniczne, długotrwałe lub ciągłe zadania niezależne od działań użytkownika lub danych wejściowych.

Aby uzyskać więcej informacji, zobacz [Co to jest usługa w chmurze?]

> [AZURE.NOTE] *Chcesz utworzyć prostą witrynę sieci Web?*
Jeśli scenariusz obejmuje tylko prosty fronton witryny sieci Web, rozważ użycie lekkiej funkcji Web Apps w usłudze App Service. Możesz łatwo przeprowadzić uaktualnienie do usługi w chmurze w przypadku rozwoju witryny sieci Web lub zmiany wymagań. W <a href="/develop/python/">Centrum deweloperów języka Python</a> można znaleźć artykuły, które dotyczą funkcji Web Apps w usłudze App Service.
<br />


## Tworzenie projektu

W programie Visual Studio możesz wybrać pozycję **Usługa w chmurze platformy Azure** w oknie dialogowym **Nowy projekt** w obszarze **Python**.

![Okno dialogowe Nowy projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

W Kreatorze usługi w chmurze platformy Azure można utworzyć nowe role Sieć Web i Proces roboczy.

![Okno dialogowe Usługa w chmurze platformy Azure](./media/cloud-services-python-ptvs/new-service-wizard.png)

Szablon roli Proces roboczy zawiera schematyczny kod służący do nawiązywania połączenia z kontem magazynu Azure lub z usługą Azure Service Bus.

![Rozwiązanie usługi w chmurze](./media/cloud-services-python-ptvs/worker.png)

W każdej chwili możliwe jest dodanie roli Sieć Web lub Proces roboczy do istniejącej usługi w chmurze.  Możesz dodawać istniejące projekty do rozwiązania lub tworzyć nowe.

![Polecenie Dodaj rolę](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Usługa w chmurze może zawierać role zaimplementowane w różnych językach.  Na przykład można mieć rolę Sieć Web języka Python zaimplementowaną za pomocą środowiska Django, języka Python lub roli Proces roboczy języka C#.  Między rolami można się w łatwy sposób komunikować za pomocą kolejek usługi Service Bus lub kolejek magazynu.

## Uruchamianie lokalnie

Jeśli ustawisz projekt usługi w chmurze jako projekt startowy i naciśniesz klawisz F5, usługa w chmurze zostanie uruchomiona w lokalnym emulatorze platformy Azure.

Mimo że program PTVS obsługuje uruchamianie w emulatorze, debugowanie nie będzie działać (dotyczy to na przykład punktów przerwania).

Aby debugować role Sieć Web i Proces roboczy, możesz ustawić projekt roli jako projekt startowy i debugować go zamiast ról.  Można również ustawić wiele projektów startowych.  Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe**.

![Właściwości projektu startowego rozwiązania](./media/cloud-services-python-ptvs/startup.png)

## Publikowanie na platformie Azure

Aby przeprowadzić publikowanie, kliknij prawym przyciskiem myszy projekt usługi w chmurze w rozwiązaniu, a następnie wybierz pozycję **Publikuj**.

![Logowanie na potrzeby publikowania na platformie Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Na stronie ustawień wybierz usługę w chmurze, w której chcesz publikować.

![Ustawienia publikowania na platformie Microsoft Azure](./media/cloud-services-python-ptvs/publish-settings.png)

Możliwe jest utworzenie nowej usługi w chmurze, jeśli żadna usługa nie jest dostępna.

![Okno dialogowe Tworzenie usługi w chmurze](./media/cloud-services-python-ptvs/publish-create-cloud-service.png)

Przydatne jest również włączenie połączeń pulpitu zdalnego z komputerami w celu debugowania awarii.

![Okno dialogowe Konfigurowanie pulpitu zdalnego](./media/cloud-services-python-ptvs/publish-remote-desktop-configuration.png)

Po zakończeniu konfigurowania ustawień kliknij pozycję **Publikuj**.

W oknie danych wyjściowych będzie wyświetlany postęp, a następnie zostanie wyświetlone okno Dziennik aktywności platformy Microsoft Azure.

![Okno Dziennik aktywności platformy Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Wdrożenie potrwa kilka minut, a następnie role Sieć Web i/lub Proces roboczy będą działały na platformie Azure!

## Następne kroki

Bardziej szczegółowe informacje na temat pracy z rolami Sieć Web i Proces roboczy w ramach programu Python Tools for Visual Studio zawiera dokumentacja programu PTVS:

- [Projekty usługi w chmurze][]

Więcej szczegółów dotyczących korzystania z usług Azure na podstawie ról Sieć Web i Proces roboczy, czyli na przykład używanie usługi Azure Storage lub Service Bus, można znaleźć w następujących artykułach.

- [Usługa Blob][]
- [Usługa tabel][]
- [Usługa kolejki][]
- [Kolejki usługi Service Bus][]
- [Tematy dotyczące usługi Service Bus][]


<!--Link references-->

[Co to jest usługa w chmurze?]: ./cloud-services/cloud-services-choose-me.md
[execution model-web sites]: ./app-service-web/app-service-web-overview.md
[execution model-vms]: ./virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: ./cloud-services/cloud-services-choose-me.md
[Centrum deweloperów języka Python]: /develop/python/

[Usługa Blob]: ./storage/storage-python-how-to-use-blob-storage.md
[Usługa kolejki]: ./storage/storage-python-how-to-use-queue-storage.md
[Usługa tabel]: ./storage/storage-python-how-to-use-table-storage.md
[Kolejki usługi Service Bus]: ./service-bus/service-bus-python-how-to-use-queues.md
[Tematy dotyczące usługi Service Bus]: ./service-bus/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Dokumentacja narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Projekty usługi w chmurze]: http://go.microsoft.com/fwlink/?LinkId=624028
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[32-bitowe środowisko Python w wersji 2.7]: http://go.microsoft.com/fwlink/?LinkId=517190
[32-bitowe środowisko Python w wersji 3.4]: http://go.microsoft.com/fwlink/?LinkId=517191



<!--HONumber=Jun16_HO2-->


