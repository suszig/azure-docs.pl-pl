---
title: "Umożliwia utworzenie Centrum IoT w portalu Azure | Dokumentacja firmy Microsoft"
description: "Sposób tworzenia, zarządzania i usuwania centra Azure IoT za pośrednictwem portalu Azure. Zawiera informacje na temat warstw cenowych, skalowania, zabezpieczeń i konfiguracji do obsługi komunikatów."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: ac1a52355ffa5354bebe3b98fdb75783bcd57697
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Tworzenie Centrum IoT przy użyciu portalu Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

W tym artykule opisano:

* Jak znaleźć usługi Centrum IoT w portalu Azure.
* Jak utworzyć i zarządzać centra IoT.

## <a name="where-to-find-the-iot-hub-service"></a>Gdzie można znaleźć usługi Centrum IoT

Usługa Centrum IoT można znaleźć w następujących lokalizacjach w portalu:

* Wybierz **+ nowy**, a następnie wybierz **Internetu rzeczy**.
* W witrynie Marketplace, wybierz **Internetu rzeczy**.

## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT

Można utworzyć Centrum IoT przy użyciu następujących metod:

* **+ Nowy** opcji Otwiera blok pokazano na poniższym zrzucie ekranu. Procedura tworzenia Centrum IoT, za pomocą tej metody i za pośrednictwem portalu marketplace są identyczne.
* W witrynie Marketplace, wybierz **Utwórz** aby otworzyć blok pokazano na poniższym zrzucie ekranu.

W poniższych sekcjach opisano kilka czynności, aby utworzyć Centrum IoT:

### <a name="choose-the-name-of-the-iot-hub"></a>Wybierz nazwę Centrum IoT

Aby utworzyć Centrum IoT, nazwę Centrum IoT. Ta nazwa musi być unikatowa w wszystkie centra IoT.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Wybierz warstwę cenową

Możesz wybrać spośród czterech warstw: **wolne**, **Standard 1** i **standardowy 2**, i **standardowa S3**. Warstwa bezpłatna umożliwia tylko 500 urządzeń do podłączenia do Centrum IoT i maksymalnie 8000 wiadomości na dzień.

**Standardowa S1**: użycie wersji S1 dla rozwiązania IoT z dużą liczbą urządzeń, że wygenerowany dla każdego niewielkich ilości danych. Każda jednostka wersji S1 umożliwia przesłanie maksymalnie 400 000 komunikatów dziennie przez wszystkie połączone urządzenia.

**Standardowa S2**: użycie wersji S2 dla rozwiązania IoT, w których urządzenie wygenerować dużych ilości danych. Każda jednostka S2 edition umożliwia wiadomości do 6 mln dziennie między wszystkich połączonych urządzeń.

**Standardowa S3**: użycie wersji S3 dla rozwiązania IoT, które generują dużych ilości danych. Każda jednostka S3 edition pozwala maksymalnie 300 milionów dziennie między wszystkich połączonych urządzeń.

![][4]

> [!NOTE]
> Centrum IoT umożliwia tylko koncentratorze wolne dla subskrypcji platformy Azure.

### <a name="iot-hub-units"></a>Jednostki Centrum IoT

Liczbę wiadomości dozwolonych dla jednej jednostki dziennie zależy od warstwy cenowej Twoje Centrum. Na przykład jeśli chcesz, aby Centrum IoT, aby obsługiwać ruch przychodzący 700 000 komunikatów, wybierz dwie jednostki warstwy S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Urządzenia do chmury partycji i grupy zasobów

Można zmienić liczbę partycji dla Centrum IoT. Domyślny numer partycji jest 4, z listy rozwijanej można wybrać inny numer.

Nie trzeba jawnie utworzyć pustej grupy zasobów. Podczas tworzenia zasobu, można wybrać opcję, aby utworzyć nową lub użyć istniejącej grupy zasobów.

![][5]

### <a name="choose-subscription"></a>Wybierz subskrypcję

Centrum IoT Azure automatycznie wyświetla listę subskrypcji platformy Azure, z którą połączony jest konto użytkownika. Można wybrać subskrypcji platformy Azure do Centrum IoT do skojarzenia.

### <a name="choose-the-location"></a>Wybierz lokalizację

Opcja lokalizacji zawiera listę regionów, w którym Centrum IoT jest dostępna.

### <a name="create-the-iot-hub"></a>Tworzenie Centrum IoT

Po zakończeniu wszystkich poprzednich kroków, można utworzyć Centrum IoT. Kliknij przycisk **Utwórz** do rozpoczęcia procesu zaplecza możesz utworzyć i wdrożyć Centrum IoT z opcji wybrania.

Może upłynąć kilka minut na utworzenie Centrum IoT, jak czas wdrożenia zaplecza do uruchomienia na serwerach odpowiednią lokalizację.

## <a name="change-the-settings-of-the-iot-hub"></a>Zmień ustawienia Centrum IoT

Po utworzeniu jest ono w bloku Centrum IoT można zmienić ustawienia istniejących Centrum IoT.

![][8]

**Zasady dostępu do udostępnionych**: te zasady definiowania uprawnień do urządzeń i usług do nawiązania połączenia Centrum IoT. Te zasady można uzyskać, klikając **zasady dostępu współużytkowanego** w obszarze **ogólne**. W tym bloku należy modyfikowania istniejących zasad lub Dodaj nowe zasady.

### <a name="create-a-policy"></a>Tworzenie zasad

* Kliknij przycisk **Dodaj** aby otworzyć blok. Można w tym miejscu wprowadź nową nazwę zasady i uprawnienia, które chcesz skojarzyć z tymi zasadami, jak pokazano na poniższej ilustracji:

    Istnieje kilka uprawnienia, które mogą być skojarzone z tych zasad udostępnionych. **Odczytać rejestru** i **zapisu rejestru** zasady prawa odczytu i zapisu w rejestrze tożsamości. Wybranie opcji zapisu automatycznie wybierze opcję odczytu.

    **Usługa połączyć** zasada nie udziela uprawnienia dostępu punktów końcowych usług, takich jak **odbierania urządzenia do chmury**. **Urządzenie połączyć** zasad przyznaje uprawnienia do wysyłania i odbierania wiadomości za pomocą punktów końcowych po stronie urządzenia IoT Hub.

* Kliknij przycisk **Utwórz** można dodać nowo utworzonej zasady do istniejącej listy.

![][10]

## <a name="endpoints"></a>Punkty końcowe

Kliknij przycisk **punkty końcowe** do wyświetlenia na liście punktów końcowych dla Centrum IoT, która jest modyfikowana. Istnieją dwa typy punktów końcowych: punkty końcowe, które są wbudowane w Centrum IoT i punkty końcowe, które dodajesz do Centrum IoT po jego utworzeniu.

![][11]

### <a name="built-in-endpoints"></a>Wbudowane punkty końcowe

Istnieją dwa punkty końcowe wbudowanych: **chmury do urządzenia opinii** i **zdarzenia**.

* **Chmury do urządzenia opinii** ustawienia: to ustawienie nie ma dwa subsettings: **chmury do urządzenia TTL** (time-to-live) i **czas przechowywania** (w godzinach) dla wiadomości. Podczas pierwszego tworzenia Centrum IoT, oba te ustawienia mają wartość domyślną jedną godzinę. Aby zmienić te ustawienia, za pomocą suwaków lub wpisz wartości.
* **Zdarzenia** ustawienia: to ustawienie nie ma kilka subsettings, niektóre z nich są tylko do odczytu. Poniższa lista zawiera opis tych ustawień:

  * **Partycje**: ustawiono wartość domyślną, podczas tworzenia Centrum IoT. Liczba partycji za pomocą tego ustawienia można zmienić.

  * **Nazwa zgodnych z Centrum zdarzeń i punktu końcowego**: Centrum IoT po utworzeniu, Centrum zdarzeń jest utworzone wewnętrznie czy może być wymagany dostęp do określonych warunkach. Nie można przypisać wartości nazwy i punktu końcowego zgodnego Centrum zdarzeń, ale można je skopiować, klikając **kopiowania**.

  * **Czas przechowywania**: Domyślnie ustawiany na jeden dzień, ale można zmienić za pomocą listy rozwijanej. Ta wartość jest w dni w obszarze Ustawienia urządzenia do chmury.

  * **Grupy konsumentów**: grupy konsumentów umożliwić wielu czytelnikom niezależnie odczytywać wiadomości z Centrum IoT. Każdy Centrum IoT tworzona jest domyślna grupa odbiorców. Można jednak dodać lub usunąć grupy konsumentów do Twojej centra IoT za pomocą tego ustawienia.

  > [!NOTE]
  > Domyślna grupa odbiorców nie można edytować ani usunąć.

### <a name="custom-endpoints"></a>Niestandardowe punkty końcowe

Można dodać niestandardowe punkty końcowe Centrum IoT przy użyciu portalu. Z **punkty końcowe** bloku, kliknij przycisk **Dodaj** u góry, aby otworzyć **dodać punkt końcowy** bloku. Wprowadź wymagane informacje, a następnie kliknij przycisk **OK**. Niestandardowe punktu końcowego zostanie wyświetlony w oknie głównym **punkty końcowe** bloku.

![][13]

Więcej informacje niestandardowe punkty końcowe w [odwołanie — punkty końcowe Centrum IoT][lnk-devguide-endpoints].

## <a name="routes"></a>Trasy

Kliknij przycisk **tras** do zarządzania, jak Centrum IoT wysyła wiadomości urządzenia do chmury.

![][14]

Można dodać trasy do Centrum IoT, klikając **Dodaj** w górnej części **tras*** bloku, wprowadzając wymagane informacje, a następnie klikając polecenie **OK**. Dostęp jest następnie wyświetlane w głównym **tras** bloku. Można edytować trasę, klikając go na liście tras. Aby włączyć trasy, kliknij go na liście tras i ustaw **włączone** Przełącz, aby **poza**. Aby zapisać wprowadzone zmiany, kliknij przycisk **OK** w dolnej części bloku.

![][15]

## <a name="pricing-and-scale"></a>Cenach i skali

Cennik istniejących Centrum IoT można zmienić za pomocą **cennik** ustawienia, z następującymi wyjątkami:

* W bieżącej implementacji Centrum IoT z bezpłatnej wersji nie można zmienić warstwy do jednego z płatnych jednostki SKU, albo na odwrót.
* Może istnieć tylko jeden Centrum IoT warstwę bezpłatna subskrypcja platformy Azure.

![][12]

Można przenosić z wyższej do dolnej warstwy tylko wtedy, gdy liczbę komunikatów wysłanych tego dnia przekroczenie limitu przydziału dla warstwy dolnej. Na przykład jeśli liczba komunikatów dziennie przekracza 400 000, następnie warstwy Centrum IoT można zmienić. Jednak w przypadku zmiany do warstwy S1 Centrum IoT jest ograniczony do danego dnia.

## <a name="delete-the-iot-hub"></a>Usuń Centrum IoT

Możesz przejść do Centrum IoT, które chcesz usunąć, klikając **Przeglądaj**, a następnie wybierając odpowiedni koncentratora do usunięcia. Aby usunąć Centrum IoT kliknij **usunąć** znajdujący się poniżej nazwę Centrum IoT.

## <a name="next-steps"></a>Następne kroki

Skorzystaj z poniższych linków, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

* [Zbiorcze zarządzania urządzeniami IoT][lnk-bulk]
* [Metryki Centrum IoT][lnk-metrics]
* [Operacje monitorowania][lnk-monitor]

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]
* [Wdrażanie urządzenia brzegowe AI krawędzi IoT Azure][lnk-iotedge]
* [Zabezpieczanie rozwiązania IoT od podstaw w górę][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
