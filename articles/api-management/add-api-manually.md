---
title: "Dodaj interfejs API ręcznie przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak dodać interfejs API ręcznie za pomocą interfejsu API zarządzania (APIM)."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 9426839f88daece1bb688a2079b7854ccaebdc57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="add-an-api-manually"></a>Ręcznie Dodaj interfejs API 

Kroki opisane w tym artykule pokazano, jak przy użyciu portalu Azure Dodaj interfejs API ręcznie do wystąpienia interfejsu API zarządzania (APIM). Typowy scenariusz, jeśli chcesz utworzyć pusty interfejsu API i ręczne definiowanie jest, aby mock interfejsu API. Aby uzyskać szczegółowe informacje o mocking interfejsu API, zobacz [odpowiedzi Mock interfejsu API](mock-api-responses.md).

Jeśli chcesz zaimportować istniejące interfejsu API, zobacz [Tematy pokrewne](#related-topics) sekcji.

W tym artykule, możemy utworzyć pusty interfejsu API i określić [httpbin.org](http://httpbin.org) (publiczne usługi testowania) jako interfejs API zaplecza.

## <a name="prerequisites"></a>Wymagania wstępne

Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Tworzenie interfejsu API

1. Wybierz **interfejsów API** zgodnie z **zarządzanie interfejsami API**.
2. Wybierz z menu po lewej stronie **+ Dodaj interfejsu API**.
3. Wybierz **puste API** z listy.

    ![Pusty interfejs API](media/add-api-manually/blank-api.png)
4. Wprowadź ustawienia interfejsu API.

    ![Ustawienia](media/add-api-manually/settings.png)

    |**Nazwa**|**Wartość**|**Opis**|
    |---|---|---|
    |**Nazwa wyświetlana**|"*Puste interfejsu API*" |Ta nazwa jest wyświetlana w portalu dla deweloperów.|
    |**Adres URL usługi sieci Web** (opcjonalnie)| "*http://httpbin.org*"| Jeśli chcesz mock interfejsu API, może nie można wprowadzić żadnych. <br/>W takim przypadku możemy wprowadzić [http://httpbin.org](http://httpbin.org). Jest to publicznej usługi testowych. <br/>Jeśli chcesz zaimportować interfejs API, który jest zamapowany na zaplecze automatycznie znajduje się w tematach w [Tematy pokrewne](#related-topics) sekcji.|
    |**Schemat adresu URL**|"*HTTPS*"|W takim przypadku nawet jeśli wewnętrznej ma dostęp HTTP niezabezpieczone, określono bezpiecznego dostępu HTTPS APIM do wewnętrznej. <br/>Tego rodzaju w scenariuszu (HTTPS http), jest nazywany zakończenia połączenia HTTPS. Można to zrobić, jeśli istnieje interfejsu API w sieci wirtualnej (Jeżeli wiadomo, że dostęp jest bezpieczne, nawet jeśli nie jest używany protokół HTTPS). <br/>Możesz chcieć użyć "Zakończenia połączenia HTTPS" można zapisać na kilka cykli Procesora.|
    |**Sufiks adresu URL**|"*hbin*"| Sufiks to nazwa, która określa tego interfejsu API w tym wystąpieniu APIM. Musi być unikatowa w tym wystąpieniu APIM.|
    |**Produkty**|"*Nieograniczone*" |Opublikuj interfejsu API interfejsu API można skojarzyć z produktem. Jeśli chcesz dla interfejsu API do opublikowania i być dostępne dla deweloperów, należy ją dodać do produktu. Można to zrobić podczas tworzenia interfejsu API lub ustaw ją później.<br/><br/>Produkty są skojarzenia jeden lub więcej interfejsów API. Można zawierają wiele interfejsów API i ich oferty dla deweloperów za pośrednictwem portalu dla deweloperów. <br/>Deweloperzy muszą najpierw subskrybować produktu, aby uzyskać dostęp do interfejsu API. Gdy subskrybujesz one, otrzymują klucz subskrypcji, który ułatwia jakiegokolwiek interfejsu API w tym produkcie. Jeśli utworzono wystąpienie APIM jesteś administratorem już, aby zasubskrybować każdego produktu domyślnie.<br/><br/> Domyślnie każde wystąpienie interfejsu API zarządzania jest dostarczany z dwóch produktów próbki: **Starter** i **nieograniczone**.| 
5. Wybierz pozycję **Utwórz**.

W tym momencie nie masz żadnych operacji APIM, które są mapowane na operacje interfejsu API zaplecza. Jeśli należy wywołać operację, która jest uwidaczniany za pomocą wewnętrznej, ale nie za pośrednictwem APIM, możesz uzyskać **404**. 

>[!NOTE] 
> Domyślnie po dodaniu interfejsu API, nawet jeśli jest on podłączony do niektórych usługi zaplecza APIM zostanie nie uwidacznia żadnych operacji do listy dozwolonych możesz je. Operacja usługi zaplecza listą dozwolonych adresów IP Utwórz operację APIM mapowana na operację zaplecza.
>

## <a name="add-and-test-an-operation"></a>Dodawanie i testowanie operacji

W tej sekcji przedstawiono sposób dodawania operacji "/ get" do mapowania go na operację "http://httpbin.org/get" zaplecza.

### <a name="add-the-operation"></a>Dodaj operację

1. Wybierz interfejs API utworzony w poprzednim kroku.
2. Kliknij przycisk **+ Dodaj operację**.
3. W **adres URL**, wybierz pozycję **UZYSKAĆ** , a następnie wprowadź "*/get*" w zasobie.
4. Wprowadź "*FetchData*" dla **Nazwa wyświetlana**.
5. Wybierz pozycję **Zapisz**.

### <a name="test-the-operation"></a>Operacja testowania

Operacja testowania w portalu Azure. Alternatywnie można przetestować go w **portalu dla deweloperów**.

1. Wybierz **testu** kartę.
2. Wybierz **FetchData**.
3. Naciśnij klawisz **wysyłania**.

Pojawi się odpowiedź, który generuje operacji "http://httpbin.org/get". Do przekształcenia działania, zobacz temat [transformacji i chronić interfejsu API](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Dodawanie i testowanie sparametryzowane operacji

W tej sekcji przedstawiono sposób dodawania operację, która przyjmuje parametr. W takim przypadku możemy mapy operacji "http://httpbin.org/status/200".

### <a name="add-the-operation"></a>Dodaj operację

1. Wybierz interfejs API utworzony w poprzednim kroku.
2. Kliknij przycisk **+ Dodaj operację**.
3. W **adres URL**, wybierz pozycję **UZYSKAĆ** , a następnie wprowadź "*/status/ {code}*" w zasobie. Opcjonalnie możesz podać niektóre informacje skojarzone z tym parametrem. Wprowadź na przykład "*numer*" dla **typu**, "*200*" (domyślna), aby uzyskać **wartości**.
4. Wprowadź "GetStatus" **Nazwa wyświetlana**.
5. Wybierz pozycję **Zapisz**.

### <a name="test-the-operation"></a>Operacja testowania 

Operacja testowania w portalu Azure.  Alternatywnie można przetestować go w **portalu dla deweloperów**.

1. Wybierz **testu** kartę.
2. Wybierz **GetStatus**. Domyślnie ma ustawioną wartość kodu "*200*". Można zmienić, aby przetestować innych wartości. Na przykład wpisz "*418*".
3. Naciśnij klawisz **wysyłania**.

    Pojawi się odpowiedź, który generuje operacji "http://httpbin.org/status/200". Do przekształcenia działania, zobacz temat [transformacji i chronić interfejsu API](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przekształcanie i chronić opublikowanych interfejsu API](transform-api.md)
