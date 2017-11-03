---
title: "Tworzenie punktów końcowych usługi sieci Web usługi Machine Learning | Dokumentacja firmy Microsoft"
description: "Tworzenie punktów końcowych usługi sieci Web w usłudze Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 821ad87fc10b2380e5ed89c037c335bc7747009e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="creating-endpoints"></a>Tworzenie punktów końcowych
> [!NOTE]
>  W tym temacie opisano techniki dotyczy **klasycznego** usługi Machine Learning w sieci Web.
> 
> 

Podczas tworzenia usług sieci Web, które sprzedaży do przodu do klientów, należy podać przeszkolone modeli do każdego klienta, nadal połączonych z doświadczenia, z którego utworzono usługę sieci Web. Ponadto wszelkie aktualizacje eksperymentu powinny być stosowane selektywnie punkt końcowy bez zastępowania dostosowania.

W tym celu uczenia maszynowego Azure umożliwia tworzenie wielu punktów końcowych dla wdrożonej usługi sieci Web. Każdy punkt końcowy usługi sieci Web jest niezależnie skierowana, ograniczenie i zarządzany. Każdy punkt końcowy jest unikatowy adres URL i klucza autoryzacji, którą można dystrybuować do klientów.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Dodawanie punktów końcowych usługi sieci Web
Istnieją trzy sposoby dodawania punktu końcowego usługi sieci Web.

* Programistycznie
* Za pomocą portalu usługi sieci Web systemu Azure Machine Learning
* Chociaż klasycznego portalu Azure

Po utworzeniu punktu końcowego, możesz pobrać go za pośrednictwem interfejsów API synchroniczne, partii interfejsów API i arkusze programu excel. Oprócz dodania punktów końcowych za pośrednictwem tego interfejsu użytkownika, umożliwia także interfejsów API Management punktu końcowego do programowego dodawania punktów końcowych.

> [!NOTE]
> Po dodaniu dodatkowe punkty końcowe z usługą sieci Web, nie można usunąć domyślnego punktu końcowego.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Programowe Dodawanie punktu końcowego
Możesz dodać punkt końcowy do usługi sieci Web, programowo przy użyciu [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) przykładowy kod.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Dodawanie punktu końcowego za pomocą portalu usługi sieci Web systemu Azure Machine Learning
1. W usłudze Machine Learning Studio w kolumnie nawigacji po lewej stronie kliknij usług sieci Web.
2. W dolnej części pulpitu nawigacyjnego usługi sieci Web, kliknij przycisk **zarządzanie punktami końcowymi**. Portal usługi sieci Web systemu Azure Machine Learning zostanie otwarta strona punkty końcowe usługi sieci Web.
3. Kliknij przycisk **Nowy**.
4. Wpisz nazwę i opis dla nowego punktu końcowego. Nazwy punktu końcowego musi być 24 znaków lub mniej długości i musi składać się z małych małych liter i cyfr. Wybierz poziom rejestrowania i czy jest włączone przykładowych danych. Aby uzyskać więcej informacji, zobacz [należy włączyć rejestrowanie dla usługi Machine Learning Web](web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Dodawanie punktu końcowego przy użyciu klasycznego portalu Azure
1. Zaloguj się do [klasycznego portalu Azure](http://manage.windowsazure.com), kliknij przycisk **uczenia maszynowego** w lewej kolumnie. Kliknij obszar roboczy, który zawiera usługę sieci Web, w której jesteś zainteresowany.
   
    ![Przejdź do obszaru roboczego](./media/create-endpoint/figure-1.png)
2. Kliknij przycisk **usług sieci Web**.
   
    ![Przejdź do usługi sieci Web](./media/create-endpoint/figure-2.png)
3. Kliknij usługę sieci Web, który chcesz wyświetlić listę dostępnych punktów końcowych.
   
    ![Przejdź do punktu końcowego](./media/create-endpoint/figure-3.png)
4. W dolnej części strony kliknij **Dodawanie punktu końcowego**. Wpisz nazwę i opis, upewnij się, że nie ma żadnych innych punktów końcowych o takiej samej nazwie w tej usługi sieci Web. W przypadku braku specjalnych wymagań pozostaw wartość domyślną poziom ograniczania. Aby dowiedzieć się więcej o ograniczaniu przepustowości, zobacz [skalowanie punkty końcowe interfejsu API](scaling-webservice.md).
   
    ![Tworzenie punktu końcowego](./media/create-endpoint/figure-4.png)

## <a name="next-steps"></a>Następne kroki
[Jak korzystać z usługi sieci Web Azure Machine Learning](consume-web-services.md).

