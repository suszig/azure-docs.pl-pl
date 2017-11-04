---
title: "Rozwiązywanie problemów z ponownego trenowania usługi sieci web Azure Machine Learning klasycznego | Dokumentacja firmy Microsoft"
description: "Zidentyfikować i rozwiązać typowe problemy napotkano, gdy są ponownego trenowania modelu dla usługi sieci Web Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 85cf9175bb4a5f253c7b47b2edc3ac8b00616ba2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Rozwiązywanie problemów z ponownego trenowania usługi sieci Web Azure Machine Learning klasycznego
## <a name="retraining-overview"></a>Ponownego trenowania — omówienie
Podczas wdrażania eksperyment predykcyjny jako usługę sieci web oceniania jest statyczny modelu. Wraz ze wzrostem dostępności nowych danych lub w przypadku interfejsu API klienta ma własne dane modelu musi być retrained. 

Aby uzyskać pełny przewodnik ponownego trenowania proces usługi sieci Web klasycznego, zobacz [Retrain Machine Learning modeli programowo](retrain-models-programmatically.md).

## <a name="retraining-process"></a>Proces ponownego trenowania
Musisz ponownie ucz usługi sieci Web, należy dodać niektóre dodatkowe elementy:

* Usługa sieci Web wdrożone z eksperymentów szkolenia. Eksperyment musi mieć **wyjście usługi sieci Web** modułu dołączony do danych wyjściowych **Train Model** modułu.  
  
    ![Wyjście usługi sieci web należy dołączyć do modelu pociągu.][image1]
* Nowy punkt końcowy dodane do oceniania usługi sieci Web.  Można dodać punktu końcowego, programowo przy użyciu przykładowy kod odwołuje się do modeli Retrain Machine Learning programowo tematu lub za pośrednictwem klasycznego portalu Azure.

Następnie można przykładowy kod C# z strona pomocy interfejsu API usługi sieci Web szkolenia retrain modelu. Po zostały obliczone wyniki i uzyskaniu je, należy zaktualizować trenowanego modelu oceniania usługi sieci web przy użyciu nowego punktu końcowego, który został dodany.

Wszystkie elementy w miejscu główne kroki, które należy wykonać, aby ponownie ucz modelu są w następujący sposób:

1. Wywoływanie usługi sieci Web szkolenia: wywołanie jest do wsadowe wykonywanie usługi (BES), nie żądanie odpowiedzi usługi (RR). Przykładowy kod C# na stronie pomocy interfejsu API służy do wywoływania. 
2. Znajdź wartości dla *BaseLocation*, *RelativeLocation*, i *SasBlobToken*: te wartości są zwracane w danych wyjściowych z połączenia z usługą sieci Web szkolenia. 
   ![Wyświetlanie danych wyjściowych ponownego trenowania próbki i wartości BaseLocation, RelativeLocation i SasBlobToken.][image6]
3. Zaktualizuj dodano punkt końcowy usługi sieci web oceniania za pomocą nowego trenowanego modelu: przy użyciu przykładowy kod podany w modelach Retrain Machine Learning programowo, zaktualizuj nowy punkt końcowy, dodane do oceniania modelu z nowo trenowanego modelu z Usługa sieci Web szkolenia.

## <a name="common-obstacles"></a>Typowych wąskich gardeł
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Sprawdź, czy masz poprawny adres URL poprawki
POPRAWKA adres URL, którego używasz, musi być skojarzony z nowego oceniania punktu końcowego, dodane do oceniania usługi sieci Web. Istnieje wiele sposobów, aby uzyskać adres URL poprawki:

**Opcja 1: programowo**

Aby uzyskać poprawny adres URL poprawki:

1. Uruchom [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) przykładowy kod.
2. Z danych wyjściowych AddEndpoint znaleźć *HelpLocation* wartości i skopiuj adres URL.
   
   ![HelpLocation w danych wyjściowych próbki addEndpoint.][image2]
3. Wklej adres URL do przeglądarki, aby przejść do strony, która zawiera łącza pomocy dla usługi sieci Web.
4. Kliknij przycisk **aktualizacji zasobów** łącze, aby otworzyć stronę pomocy poprawki.

**Opcja 2: Użyj klasycznego portalu Azure**

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Otwórz kartę uczenia maszynowego. ![Karta oparcie maszyny.][image4]
3. Następnie kliknij nazwę obszaru roboczego **usług sieci Web**.
4. Kliknij przycisk oceniania usługi sieci Web, którą użytkownik pracuje z. (Jeśli nie zmienił domyślnej nazwy usługi sieci web, będą kończyć się [Exp oceniania.]).
5. Kliknij przycisk **dodać punkt końcowy**.
6. Po dodaniu punktu końcowego, kliknij nazwę punktu końcowego. Następnie kliknij przycisk **aktualizacji zasobów** otworzyć stosowania poprawek strony pomocy.

> [!NOTE]
> Jeśli dodano punkt końcowy z usługą sieci Web szkolenia zamiast predykcyjnej usługi sieci Web, zostanie wyświetlony następujący błąd, po kliknięciu **aktualizacji zasobów** łącza: Niestety, ale ta funkcja nie jest obsługiwana lub dostępna w tym kontekst. Ta usługa sieci Web ma nie nadaje się do aktualizacji zasobów. Firma Microsoft Przepraszamy za utrudnienia i działają na ułatwieniu ten przepływ pracy.
> 
> 

![Nowy punkt końcowy pulpitu nawigacyjnego.][image3]

Strona pomocy poprawki zawiera adres URL poprawka, należy użyć i udostępnia przykładowy kod, którego można użyć w celu wywołania go.

![Adres URL poprawki.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Sprawdź, czy aktualizujesz właściwego punktu końcowego oceniania
* Nie poprawka usługę sieci Web szkolenia: operacja poprawki musi zostać wykonana na oceniania usługi sieci Web.
* Poprawka nie jest domyślny punkt końcowy usługi sieci Web: operacja poprawki musi zostać wykonana na nowe oceniania końcowy usługi sieci Web dodany.

Możesz sprawdzić, które usługi sieci Web punktu końcowego jest w klasycznym portalu Azure. 

> [!NOTE]
> Upewnij się, że punkt końcowy jest dodawany do predykcyjnej usługi sieci Web nie usługę sieci Web szkolenia. Jeśli zostały poprawnie wdrożone zarówno szkolenia, jak i usługi sieci Web predykcyjnej, powinny pojawić się dwa oddzielne usług sieci Web na liście. Predykcyjnej usługi sieci Web powinien kończyć się "[exp predykcyjnych.]".
> 
> 

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Otwórz kartę uczenia maszynowego. ![Obszaru roboczego uczenia maszyny interfejsu użytkownika.][image4]
3. Wybierz obszar roboczy.
4. Kliknij przycisk **usług sieci Web**.
5. Wybierz usługę sieci Web predykcyjnej.
6. Sprawdź, czy nowy punkt końcowy został dodany do usługi sieci Web.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Sprawdź obszar roboczy usługi sieci web jest w celu zapewnienia, że jest poprawny region
1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Uczenie maszynowe, wybierz z menu.
   ![Machine learning region interfejsu użytkownika.][image4]
3. Sprawdź lokalizację swojego obszaru roboczego.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
