---
title: "Korzystanie z szablonu aplikacji sieci web usługi sieci web Machine Learning | Dokumentacja firmy Microsoft"
description: "Użyj szablonu aplikacji sieci web w portalu Azure Marketplace, aby korzystać z usługi sieci web predykcyjnej w usłudze Azure Machine Learning."
keywords: "Usługa sieci Web, operationalization, interfejsu API REST, uczenia maszynowego"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1c182403409966923440f359cb2514af7b7df9f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Korzystanie z usługi sieci Web Azure Machine Learning za pomocą szablonu aplikacji sieci Web

Po opracowany model predykcyjny i wdrożyć go jako usługi sieci web platformy Azure przy użyciu usługi Machine Learning Studio lub za pomocą narzędzi, takich jak R lub Python, są dostępne operationalized modelu przy użyciu interfejsu API REST.

Istnieje wiele sposobów, aby korzystać z interfejsu API REST i uzyskania dostępu do usługi sieci web. Na przykład pisania aplikacji w języku C#, R lub Python za pomocą przykładowy kod wygenerowany przez po wdrożeniu usługi sieci web (dostępne w [usługi Machine Learning portalu](https://services.azureml.net/quickstart) lub na pulpicie nawigacyjnym usługi sieci web w usłudze Machine Learning Studio). Lub może używać do przykładowego skoroszytu programu Microsoft Excel utworzony w tym samym czasie.

Ale jest najszybszym i Najprostszym sposobem uzyskania dostępu do usługi sieci web za pomocą szablonów aplikacji sieci Web dostępnych w [Azure Marketplace aplikacji sieci Web](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure Machine Learning szablonów aplikacji sieci Web
Szablony aplikacji sieci web dostępne w portalu Azure Marketplace można utworzyć aplikacji sieci web niestandardowego, który zna usługi sieci web dane wejściowe i oczekiwanych rezultatów. Wszystko, co należy zrobić to uzyskania dostępu do aplikacji sieci web do usługi sieci web i danych, a reszta szablonu.

Dostępne są dwa szablony:

* [Szablon aplikacji sieci Web usługi Azure ML żądań i odpowiedzi](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Szablon aplikacji partii zadań Azure ML wykonywania usługi sieci Web](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Każdy szablon tworzy przykładowej aplikacji platformy ASP.NET, za pomocą interfejsu API identyfikatora URI i klucz dla usługi sieci web i wdraża go jako witryny sieci web na platformie Azure. Szablon usługi żądań i odpowiedzi (RR) służy do tworzenia aplikacji sieci web, która pozwala na wysyłanie pojedynczy wiersz danych z usługą sieci web, umożliwiającej uzyskanie pojedynczego wyniku. Szablon usługi wykonywania wsadowego (BES) służy do tworzenia aplikacji sieci web, która pozwala na wysyłanie wiele wierszy danych, aby uzyskać wyniki wielu.

Kodowanie nie jest wymagana do używania tych szablonów. Wystarczy podać klucz interfejsu API i identyfikator URI, a szablon tworzy aplikację.

Aby uzyskać klucz interfejsu API i identyfikator URI żądania usługi sieci web:

1. W [Portal usługi sieci Web](https://services.azureml.net/quickstart), nowej usługi sieci web, kliknij przycisk **usług sieci Web** u góry. Lub kliknij usługi sieci web klasycznego **klasycznym usługi sieci Web**.
2. Kliknij opcję usługi sieci web, którą chcesz uzyskać dostęp.
3. Kliknij punkt końcowy, który ma dostęp do usługi sieci web klasycznego.
4. Kliknij przycisk **Consume** u góry.
5. Kopiuj **głównej** lub **klucza pomocniczego** i zapisz go.
6. Jeśli tworzysz szablon żądania i odpowiedzi usługi (RR), skopiuj **żądań i odpowiedzi** identyfikatora URI i zapisz go. Jeśli tworzysz szablon usługi wykonywania wsadowego (BES), skopiuj **żądań wsadowych** identyfikatora URI i zapisz go.


## <a name="how-to-use-the-request-response-service-rrs-template"></a>Jak używać szablonu żądań i odpowiedzi usługi (RR)
Wykonaj następujące kroki, aby użyć szablonu aplikacji sieci web rekordów zasobów, jak pokazano na poniższym diagramie.

![Proces szablon rekordy zasobów sieci web][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Przejdź do [portalu Azure](https://portal.azure.com), **logowania**, kliknij przycisk **nowy**, wyszukaj i wybierz **aplikacji sieci Web usługi Azure ML żądanie-odpowiedź**, następnie kliknij przycisk **Utwórz**. 
   
   * Nadaj unikatową nazwę aplikacji sieci web. Adres URL aplikacji sieci web będzie tej nazwy, a następnie `.azurewebsites.net.` na przykład`http://carprediction.azurewebsites.net.`
   * Wybierz subskrypcję platformy Azure i usługi, w których jest uruchomiona usługa sieci web.
   * Kliknij przycisk **Utwórz**.
     
     ![Tworzenie aplikacji sieci Web][image5]

4. Po zakończeniu Azure podczas wdrażania aplikacji sieci web, kliknij przycisk **adres URL** w ustawieniach aplikacji sieci web strony na platformie Azure, lub wprowadź adres URL w przeglądarce sieci web. Na przykład: `http://carprediction.azurewebsites.net.`
5. Podczas pierwszego uruchomienia aplikacji sieci web go wyświetli monit o **adresu URL przesyłania interfejsu API** i **klucz interfejsu API**.
   Wprowadź wartości został wcześniej zapisany (**przez identyfikator URI żądania** i **klucz interfejsu API**odpowiednio).
     
     Kliknij przycisk **przesłać**.
     
     ![Wprowadź Post identyfikator URI i klucz interfejsu API][image6]

6. Aplikacja sieci web jest wyświetlana jego **konfiguracji aplikacji sieci Web** strony z bieżącymi ustawieniami usługi sieci web. Tutaj można wprowadzić zmiany w ustawieniach używany przez aplikację sieci web.
   
   > [!NOTE]
   > Zmiana ustawień w tym miejscu zmienia tylko ich tej aplikacji sieci web. Go nie powoduje zmiany ustawień domyślnych usługi sieci web. Na przykład, jeśli zmienisz **opis** w tym miejscu nie zmienia opis wyświetlane na pulpicie nawigacyjnym usługi sieci web w usłudze Machine Learning Studio.
   > 
   > 
   
    Gdy wszystko będzie gotowe, kliknij przycisk **zapisać zmiany**, a następnie kliknij przycisk **przejdź do strony głównej**.

7. Na stronie głównej można wprowadzić wartości do wysłania do usługi sieci web. Kliknij przycisk **przesyłania** gdy wszystko będzie gotowe, i zostanie zwrócony wynik.

Jeśli chcesz powrócić do **konfiguracji** strony, przejdź do `setting.aspx` strony aplikacji sieci web. Na przykład: `http://carprediction.azurewebsites.net/setting.aspx.` pojawi się monit, aby ponownie wprowadź klucz interfejsu API — należy to dostęp do strony Ustawienia i aktualizować.

Można zatrzymać, ponownie uruchomić lub usunąć aplikacji sieci web w portalu Azure, takich jak każda inna aplikacja sieci web. Tak długo, jak działa można przejść do adresu internetowego macierzystego i wprowadź nowe wartości.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Jak używać szablonu usługi wykonywania wsadowego (BES)
Można użyć szablonu aplikacji sieci web usługi BES w taki sam sposób jak szablon rekordy zasobów, z tym, że aplikacja sieci web, która jest tworzona umożliwi przesłać wiele wierszy danych i odbierania wiele wyników.

Wartości wejściowe dla usługi sieci web wykonywania wsadowego mogą pochodzić z magazynu Azure lub plikiem lokalnym; wyniki są przechowywane w kontenerze magazynu Azure.
Tak będziesz potrzebować kontenera magazynu Azure do przechowywania wyników zwróconych przez aplikację sieci web i konieczne będzie przygotowywanie danych wejściowych.

![Proces, aby użyć szablonu sieci web usługi BES][image2]

1. Postępuj zgodnie z tą samą procedurą, aby utworzyć aplikację sieci web usługi BES, podobnie jak w przypadku szablonu rekordów zasobów, z wyjątkiem przejdź do [szablonu aplikacji sieci Web usługi wykonywania Azure ML partii](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Otwórz szablon usługi BES w witrynie Azure Marketplace, a następnie kliknij przycisk **tworzenie aplikacji sieci Web**.

2. Aby określić, w którym wyniki przechowywane, wprowadź informacje o kontenerze docelowego na stronie głównej aplikacji sieci web. Również określić, gdzie aplikacja sieci web można uzyskać wartości wejściowe w pliku lokalnym lub kontener magazynu Azure.
   Kliknij przycisk **przesłać**.
   
    ![Informacje o magazynu][image7]

Aplikacja sieci web zostanie wyświetlona strona o stanie zadania.
Po ukończeniu zadania będziesz mieć lokalizacji wyników w magazynie obiektów blob platformy Azure. Istnieje również możliwość pobierania wyniki do pliku lokalnego.

## <a name="for-more-information"></a>Aby uzyskać więcej informacji
Aby dowiedzieć się więcej o...

* Tworzenie eksperymentu uczenia maszynowego o usłudze Machine Learning Studio, zobacz [Tworzenie pierwszego eksperymentu w usłudze Azure Machine Learning Studio](create-experiment.md)
* jak wdrożyć jako usługę sieci web eksperymentu uczenia maszynowego, zobacz [wdrażanie usługi sieci web Azure Machine Learning](publish-a-machine-learning-web-service.md)
* inne sposoby uzyskania dostępu do usługi sieci web, zobacz [jak korzystać z usługi Azure Machine Learning w sieci Web](consume-web-services.md)

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
