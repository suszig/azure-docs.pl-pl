---
title: "Jak zarządzać dostępem urządzenia inicjowania obsługi usługi Azure IoT Hub urządzenia | Dokumentacja firmy Microsoft"
description: "Jak odwołać dostęp do urządzeń z usługą punktu dystrybucji w portalu Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6f1dad648b228163219c8f722eed3897f4ba4d22
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Jak odwołać dostęp urządzenia do inicjowania obsługi usługi w portalu Azure

Właściwego zarządzania poświadczenia urządzeń ma podstawowe znaczenie dla systemów wysokiej jakości, takie jak rozwiązania IoT. Najlepszym rozwiązaniem dla takich systemów ma Wyczyść plan jak odwołanie dostępu dla urządzeń w przypadkach, w którym swoje poświadczenia, czy SAS token lub certyfikatu X.509, mogą zostać przejęte. W tym artykule opisano sposób odwołać dostęp do urządzeń w kroku inicjowania obsługi administracyjnej.

Aby dowiedzieć się więcej o odwołaniu dostępu urządzenia do Centrum IoT po urządzenia zainicjowano. zobacz [wyłączyć urządzenia](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Należy pamiętać o zasady ponawiania urządzeń, które odwołać dostęp. Na przykład urządzenie z zasady ponawiania nieskończone stale spróbuj zarejestrować w usłudze udostępniania zużywające zasoby usługi i prawdopodobnie wpływające na wydajność.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Czarna lista urządzeń z wpisem poszczególnych rejestracji

Poszczególne rejestracji zastosować do jednego urządzenia i może używać certyfikatów X.509 lub tokeny sygnatury dostępu Współdzielonego (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizm zaświadczania. (Urządzeń używających tokeny sygnatury dostępu Współdzielonego, zgodnie z ich mechanizm zaświadczania można alokować tylko za pomocą poszczególnych rejestracji). Aby wyeliminować urządzenie ma poszczególnych rejestracji, możesz wyłączyć lub usunąć jego wpis rejestracji: 

- Aby tymczasowo wyeliminować urządzenia, można wyłączyć jego wpis rejestracji. 

    1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** z menu po lewej stronie.
    2. Kliknij usługę inicjowania obsługi administracyjnej, aby wyeliminować urządzenie z listy zasobów.
    3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **poszczególnych rejestracji** kartę.
    4. Kliknij polecenie wpisu rejestracji dla urządzenia, które chcesz wyeliminować, aby go otworzyć. 
    5. Kliknij przycisk **wyłączyć** w dolnej części listy wpisu rejestracji, następnie kliknij przycisk **zapisać**.  

        ![Wyłącz wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Aby trwale wyeliminować urządzenia, możesz usunąć jego wpis rejestracji.

    1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** z menu po lewej stronie.
    2. Kliknij usługę inicjowania obsługi administracyjnej, aby wyeliminować urządzenie z listy zasobów.
    3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **poszczególnych rejestracji** kartę.
    4. Zaznacz pole obok wpisu rejestracji dla urządzenia, które chcesz wyeliminować. 
    5. Kliknij przycisk **usunąć** u góry okna, następnie kliknij przycisk **tak** aby upewnić się, że chcesz usunąć rejestrację. 

        ![Usuń wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Po zakończeniu działania zostanie wyświetlony wpis usunięty z listy poszczególnych rejestracji.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Wyeliminować X.509 pośredniego lub certyfikatu głównego urzędu certyfikacji za pomocą grupy rejestracji

Certyfikaty X.509 zwykle są rozmieszczone w łańcuchu zaufania certyfikatu. Naruszenie certyfikatów na każdym etapie w łańcuchu staje się relacja zaufania zostanie zerwana, a certyfikat musi być na liście zabronionych numerów zapobiegające urządzenia w strumieniu w łańcucha zawierający certyfikat z inicjowana przez usługę inicjowania obsługi urządzeń. Aby dowiedzieć się więcej na temat certyfikatów X.509 oraz sposób ich użycia w usłudze udostępniania, zobacz [certyfikatów X.509](./concepts-security.md#x509-certificates). 

Wpis dla urządzeń, które udostępniają wspólnego mechanizmu zaświadczania podpisane przez ten sam pośrednich certyfikatów X.509 lub główny urząd certyfikacji jest grupą rejestracji. Wpis grupy rejestracji jest skonfigurowany przy użyciu certyfikatu X.509 skojarzone z pośredniego lub głównego urzędu certyfikacji, a także żadnych konfiguracji wartości, takich jak stan dwie i połączenie z Centrum IoT, które są udostępniane przez urządzenia z tego certyfikatu w certyfikacie łańcuch. Aby wyeliminować certyfikat, możesz wyłączyć lub usunąć jej grupy rejestracji:

- Aby tymczasowo wyeliminować certyfikatu, można wyłączyć jej grupy rejestracji. 

    1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** z menu po lewej stronie.
    2. Kliknij usługę inicjowania obsługi administracyjnej, aby wyeliminować certyfikatu podpisywania z na liście zasobów.
    3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **grup rejestracji** kartę.
    4. Kliknij grupę rejestracji certyfikatu, który chcesz wyeliminować, aby go otworzyć.
    5. Kliknij przycisk **Edytuj grupę** w lewym górnym wpisu rejestracji grupy.
    6. Aby wyłączyć wpisu rejestracji, wybierz pozycję **wyłączyć** na **włączyć wpis** przełącznik, a następnie kliknij przycisk **zapisać**.  

        ![Wyłączenie rejestracji grupy wpisu w portalu](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Aby wyeliminować trwale certyfikatu, można usunąć jej grupy rejestracji.

    1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** z menu po lewej stronie.
    2. Kliknij usługę inicjowania obsługi administracyjnej, aby wyeliminować urządzenie z listy zasobów.
    3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **grup rejestracji** kartę.
    4. Zaznacz pole obok grupy rejestracji certyfikatu, który chcesz wyeliminować. 
    5. Kliknij przycisk **usunąć** u góry okna, następnie kliknij przycisk **tak** potwierdzenie usunięcia grupy rejestracji. 

        ![Usuń wpis grupy rejestracji w portalu](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. Po zakończeniu działania zostanie wyświetlony wpis usunięte z listy grup rejestracji.  

> [!NOTE]
> Jeśli usuniesz grupę rejestracji certyfikatu, urządzeń mających ten certyfikat w łańcuchu certyfikatów, ich może nadal mieć możliwość rejestrowania, jeśli grupa włączone rejestracji certyfikatu głównego lub innego certyfikatu pośredniego się wyżej w certyfikacie istnieje łańcuch.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Czarna lista określonych urządzeń w grupie rejestracji

Urządzenia, które implementuje mechanizm zaświadczania X.509 używają łańcucha certyfikatów urządzenia i klucz prywatny do uwierzytelniania. Gdy urządzenie łączy i służy do uwierzytelniania przy użyciu usługi inicjowania obsługi urządzeń, usługi najpierw wyszukiwana poszczególnych rejestracji dopasowania przed wyszukiwaniem rejestracji grup w celu określenia, czy urządzenie można udostępnić poświadczeń urządzenia. Jeśli usługa znajdzie wyłączone rejestracji poszczególne urządzenia, uniemożliwia urządzeniu połączenie, nawet jeśli istnieje grupa rejestracji włączone dla pośredniego lub głównego urzędu certyfikacji w łańcuchu certyfikatów urządzenia. Aby wyeliminować poszczególne urządzenia w grupie rejestracji, wykonaj następujące kroki:

1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** z menu po lewej stronie.
2. Z listy zasobów kliknij usługę inicjowania obsługi administracyjnej, zawierający grupę rejestracji dla urządzenia, które chcesz wyeliminować.
3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **poszczególnych rejestracji** kartę.
4. Kliknij przycisk **Dodaj** na górze. 
5. Wybierz **X.509** jako mechanizm zabezpieczeń urządzenia i przekazywanie certyfikatu tego urządzenia. Jest to certyfikat podpisany jednostki końcowej zainstalowane na urządzeniu, która jest używana do generowania certyfikatów dla uwierzytelniania.
6. Wprowadź **identyfikator urządzenia IoT Hub** dla urządzenia. 
7. Aby wyłączyć wpisu rejestracji, wybierz pozycję **wyłączyć** na **włączyć wpis** przełącznika. 
8. Kliknij pozycję **Zapisz**. O pomyślnym utworzeniu informacji o rejestracji, powinien zostać wyświetlony urządzenia są wyświetlane w obszarze **poszczególnych rejestracji** kartę. 

    ![Wyłącz wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




