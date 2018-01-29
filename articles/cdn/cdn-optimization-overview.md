---
title: "Optymalizacja Azure dostarczania zawartości dla danego scenariusza"
description: "Jak zoptymalizować dostarczania zawartości dla konkretnych scenariuszy"
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: fa4cf306eeb1a8372da5b2a86ac73fbba2832666
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optymalizacja Azure dostarczania zawartości dla danego scenariusza

Podczas dostarczania zawartości do dużej, globalnej grupy odbiorców, bardzo ważne jest upewnij się, zoptymalizowane dostarczania zawartości. Azure Content Delivery Network zoptymalizować proces dostarczania na podstawie typu zawartości, do których masz. Zawartość może być witryna sieci Web, strumień na żywo, wideo lub dużego pliku do pobrania. Po utworzeniu punktu końcowego (CDN) sieci dostarczania zawartości, określ scenariusz w **zoptymalizowane pod kątem** opcji. Wybór określa, które optymalizacji jest stosowane do zawartości z punktu końcowego CDN.

Opcje optymalizacji są przeznaczone do zachowaniami najlepszych rozwiązań, aby zwiększyć wydajność dostarczania zawartości i odciążanie lepsze pochodzenia. Wybrane opcje scenariusz wpłynąć na wydajność, modyfikując konfiguracje dla częściowej pamięci podręcznej, obiekt segmentu i zasady ponawiania źródła błędu. 

Ten artykuł zawiera omówienie różnych funkcji optymalizacji i kiedy należy używać. Aby uzyskać więcej informacji o funkcjach i ograniczenia Zobacz odpowiednich artykułów na poszczególnych typach poszczególnych optymalizacji.

> [!NOTE]
> Twoje **zoptymalizowane pod kątem** opcje mogą różnić w zależności od dostawcy, należy wybrać. Dostawcy sieci CDN zastosować rozszerzenia na różne sposoby, w zależności od scenariusza. 

## <a name="provider-options"></a>Opcje dostawcy

**Usługa Azure Content Delivery Network from Akamai** obsługuje następujące funkcje optymalizacji:

* [Dostarczanie ogólne sieci web](#general-web-delivery) 

* [Ogólne przesyłania strumieniowego multimediów](#general-media-streaming)

* [Przesyłanie strumieniowe multimediów wideo na żądanie](#video-on-demand-media-streaming)

* [Pobieranie dużych plików](#large-file-download)

* [Przyspieszanie witryn dynamicznych](#dynamic-site-acceleration) 

**Usługa Azure Content Delivery Network from Verizon** obsługuje następujące funkcje optymalizacji:

* [Ogólne web dostarczania](#general-web-delivery) (może również służyć do przesyłania strumieniowego multimediów i plików o dużym pobierania zawartości)

* [Przyspieszanie witryn dynamicznych](#dynamic-site-acceleration) 

Zdecydowanie zaleca się przetestowanie zmian wydajności od różnych dostawców, aby wybrać optymalne dostawcy dla firmy.

## <a name="select-and-configure-optimization-types"></a>Wybierz i skonfiguruj typy optymalizacji

Aby utworzyć nowy punkt końcowy, wybierz typ optymalizacji, najlepiej pasującą do scenariusza i typu zawartości, którą chcesz dostarczyć punktu końcowego. **Ogólne web dostarczania** jest ustawieniem domyślnym. Dla istniejących **Azure Content Delivery Network from Akamai** punktów końcowych, możesz zaktualizować opcji optymalizacji w dowolnym momencie. Ta zmiana nie przerywają pracy dostarczania z sieci CDN. 

1. W ramach **Azure Content Delivery Network from Akamai** profilu, wybierz punkt końcowy.

    ![Punkt końcowy zaznaczenia ](./media/cdn-optimization-overview/01_Akamai.png)

2. W obszarze **ustawienia**, wybierz pozycję **optymalizacji**. Wybierz typ z **zoptymalizowane pod kątem** listy rozwijanej.

    ![Wybór optymalizacji i typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optymalizacja dla konkretnych scenariuszy

Aby zoptymalizować punkt końcowy CDN dla jednego z tych scenariuszy. 

### <a name="general-web-delivery"></a>Ogólne dostarczanie w sieci Web

Dostarczanie ogólne sieci web jest najbardziej typowych opcji optymalizacji. Jest on przeznaczony dla optymalizacji zawartości ogólne sieci web, takich jak aplikacje sieci web i stron sieci Web. Tego rodzaju optymalizacji mogą służyć do pliku i pobierane pliki wideo.

Typowy witryny sieci Web zawiera zawartość statycznych i dynamicznych. Zawartość statyczna zawiera obrazy, biblioteki JavaScript i arkusze stylów, które mogą być buforowane i dostarczane do różnych użytkowników. Zawartość dynamiczna jest spersonalizowane dla poszczególnych użytkowników, takie jak wiadomości, które są dostosowane do profilu użytkownika. Zawartość dynamiczna nie jest buforowany, ponieważ jest on unikatowy dla każdego użytkownika, takie jak zawartość koszyka zakupów. Dostarczanie ogólne sieci web można zoptymalizować całej witryny sieci Web. 

> [!NOTE]
> Jeśli używasz **Azure Content Delivery Network from Akamai**, można użyć tego rodzaju optymalizacji, jeśli Twoje średni rozmiar pliku jest mniejsza niż 10 MB. Jeśli Twoje średni rozmiar pliku jest większy niż 10 MB, wybierz **pobierania plików o dużym** z **zoptymalizowane pod kątem** listy rozwijanej.

### <a name="general-media-streaming"></a>Ogólne transmisje strumieniowe multimediów

Jeśli musisz użyć punktu końcowego dla transmisji strumieniowych na żywo i strumieniowego przesyłania wideo na żądanie, zalecamy ogólne multimediów strumieniowych optymalizacji.

Przesyłania strumieniowego multimediów jest czasowego, ponieważ pakiety przychodzące późno na kliencie może spowodować uszkodzenie wrażenia, takie jak częste buforowanie zawartości wideo. Multimediów strumieniowych optymalizacji zmniejsza opóźnienie dostarczania zawartości nośnika i zapewnia płynnego przesyłania strumieniowego dla użytkowników. 

Ten scenariusz jest typowy dla klientów usługi Azure media. Gdy używasz usługi Azure media services, możesz uzyskać jeden przesyłania strumieniowego punktu końcowego, który może służyć do przesyłania strumieniowego na żywo i na żądanie. W tym scenariuszu klienci nie musisz przełączyć się do innego punktu końcowego zmiany z na żywo w celu przesyłania strumieniowego na żądanie. Optymalizacja przesyłania strumieniowego multimediów ogólne obsługuje scenariusza tego typu.

**Azure Content Delivery Network from Verizon** używa typu optymalizacji dostarczania ogólne sieci web do dostarczania transmisji strumieniowej zawartości nośnika.

Aby uzyskać więcej informacji o optymalizacji przesyłania strumieniowego multimediów, zobacz [multimediów strumieniowych optymalizacji](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Przesyłanie strumieniowe multimediów wideo na żądanie

Optymalizacja przesyłania strumieniowego multimediów wideo na żądanie poprawia przesyłania strumieniowego zawartości wideo na żądanie. Jeśli używasz punktu końcowego przesyłania strumieniowego wideo na żądanie, można użyć tej opcji.

**Azure Content Delivery Network from Verizon** używa typu optymalizacji dostarczania ogólne sieci web do dostarczania transmisji strumieniowej zawartości nośnika.

Aby uzyskać więcej informacji o optymalizacji przesyłania strumieniowego multimediów, zobacz [multimediów strumieniowych optymalizacji](cdn-media-streaming-optimization.md).

> [!NOTE]
> Jeśli punkt końcowy służy głównie zawartości wideo na żądanie, użyj tego typu optymalizacji. Główna różnica między tego rodzaju optymalizacji i przesyłania strumieniowego optymalizacji multimediów ogólne jest limit ponownych prób połączenia. Limit czasu jest znacznie krótszy do pracy ze scenariuszami transmisji strumieniowej na żywo.

### <a name="large-file-download"></a>Pobieranie dużych plików

Jeśli używasz **Azure Content Delivery Network from Akamai**, należy użyć pobierania dużych plików w celu dostarczania plików większych niż 1,8 GB. **Usługa Azure Content Delivery Network from Verizon** nie ma ograniczenia rozmiaru pobierania pliku w jego optymalizację dostarczania ogólne sieci web.

Jeśli używasz **Azure Content Delivery Network from Akamai**, pliki do pobrania plików o dużym są zoptymalizowane pod kątem zawartości większych niż 10 MB. Jeśli Twoje średni rozmiar pliku jest mniejsza niż 10 MB, można użyć dostarczania ogólne sieci web. Jeśli Twoje rozmiary plików średni są stale większych niż 10 MB, może być bardziej wydajne, aby utworzyć oddzielne punkt końcowy dla dużych plików. Aktualizacje oprogramowania układowego i oprogramowania są zwykle dużych plików.

**Azure Content Delivery Network from Verizon** używa typu optymalizacji dostarczania ogólne sieci web do dostarczania zawartości pobierania dużych plików.

Aby uzyskać więcej informacji o optymalizacji dużych plików, zobacz [optymalizacji plików o dużym](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Przyspieszanie witryn dynamicznych

 Akceleracja dynamiczne witryny jest dostępna zarówno w **Azure Content Delivery Network from Akamai** i **Azure Content Delivery Network from Verizon** profilów. Tego rodzaju optymalizacji pociąga za sobą dodatkowe opłaty użyty. Aby uzyskać więcej informacji, zobacz [cennik Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

Akceleracja dynamiczne witryny obejmuje różne techniki, które korzystają opóźnienia i wydajności, zawartości dynamicznej. Techniki to m.in. optymalizacji trasy i sieci, optymalizacja protokołu TCP i inne. 

Tego rodzaju optymalizacji można użyć w celu przyspieszenia aplikacji sieci web, która zawiera wiele odpowiedzi, które nie są buforowalnej. Przykłady są wyniki wyszukiwania, transakcje wyewidencjonowania lub danych w czasie rzeczywistym. Można nadal używać podstawowych możliwości buforowania CDN dla danych statycznych. 

Aby uzyskać więcej informacji na temat przyspieszenia dynamiczne witryny, zobacz [przyspieszenie dynamiczne witryny](cdn-dynamic-site-acceleration.md).



