---
title: "Przepływność udostępniania dla bazy danych Azure rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak ustawić udostępnionej przepływności dla containsers bazy danych Azure rozwiązania Cosmos, kolekcje, wykresów i tabel."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Ustaw przepustowość dla kontenerów bazy danych Azure rozwiązania Cosmos

Przepływność można ustawić dla kontenerów bazy danych Azure rozwiązania Cosmos w portalu Azure lub za pomocą zestawów SDK klienta. 

W poniższej tabeli wymieniono dostępne dla kontenerów przepływności:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Kontener jednej partycji</strong></p></td>
            <td valign="top"><p><strong>Kontener podzielonym na partycje</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Przepustowość minimalna</p></td>
            <td valign="top"><p>400 jednostek żądań na sekundę</p></td>
            <td valign="top"><p>2500 jednostki żądania na sekundę</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maksymalna przepustowość</p></td>
            <td valign="top"><p>10 000 jednostek żądań na sekundę</p></td>
            <td valign="top"><p>Nieograniczona liczba</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Aby ustawić przepływność przy użyciu portalu Azure

1. Otwórz w nowym oknie, [portalu Azure](https://portal.azure.com).
2. Na pasku po lewej stronie kliknij **bazy danych Azure rozwiązania Cosmos**, lub kliknij przycisk **więcej usług** u dołu, następnie przewiń do **baz danych**, a następnie kliknij przycisk **bazy danych Azure rozwiązania Cosmos**.
3. Wybierz konto DB rozwiązania Cosmos.
4. W nowym oknie kliknij **Eksploratora danych (wersja zapoznawcza)** w menu nawigacji.
5. W nowym oknie, rozwiń węzeł bazy danych i kontener, a następnie kliknij przycisk **& Ustawienia skalowania**.
6. W nowym oknie, wpisz nową wartość przepływności w **przepływności** , a następnie kliknij przycisk **zapisać**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a>Aby ustawić przepływność przy użyciu interfejsu API usługi DocumentDB dla platformy .NET

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Przepływność — często zadawane pytania

**Można ustawić Moje przepływności na mniej niż 400 RU/s?**

400 RU/s jest minimalna przepływność dostępne na kolekcje z jedną partycją DB rozwiązania Cosmos (minimum dla kolekcji partycjonowanych to 2500 RU/s). Żądanie jednostki są ustawiane w 100 RU/s odstępach czasu, ale przepływności nie można ustawić na 100 RU/s lub dowolną wartość mniejszą niż 400 RU/s. Jeśli szukasz ekonomiczne metody umożliwiające opracowanie i przetestowanie rozwiązania Cosmos DB mogą korzystać z BEZPŁATNEJ [Azure rozwiązania Cosmos DB emulatora](local-emulator.md), które można wdrożyć lokalnie bez ponoszenia kosztów. 

**Jak ustawić througput przy użyciu interfejsu API bazy danych MongoDB?**

Brak bez rozszerzenia interfejsu API bazy danych MongoDB, można ustawić przepływności. Zalecane jest używanie interfejsu API usługi DocumentDB, jak pokazano w [można ustawić przepływność przy użyciu interfejsu API usługi DocumentDB dla platformy .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o zasięgu planety będzie DB rozwiązania Cosmos i udostępniania, zobacz [dzielenia na partycje i skalowania rozwiązania Cosmos DB](partition-data.md).
