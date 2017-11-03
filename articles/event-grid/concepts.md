---
title: "Azure pojęcia siatki zdarzeń"
description: "Opisuje Azure zdarzeń siatki i jego pojęcia. Definiuje kilka najważniejszych składników zdarzeń siatki."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.openlocfilehash: 5b69478bf00284594b984fde452f6bed4e73859b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="concepts-in-azure-event-grid"></a>Pojęcia dotyczące usługi Azure Event siatki

Główne pojęcia w siatce zdarzeń Azure są:

## <a name="events"></a>Zdarzenia

Zdarzenie jest najmniejsza ilość danych opisujący pełni coś się stało w systemie.  Każde zdarzenie zawiera typowe informacje, takie jak: źródło zdarzenia, czas zdarzenia miały miejsce i unikatowym identyfikatorem.  Każde zdarzenie ma również informacje, na które jest tylko istotne dla konkretnego zdarzenia. Na przykład zdarzenie o nowy plik tworzony w usłudze Azure Storage zawiera szczegółowe informacje o pliku, na przykład wartość lastTimeModified. Lub zdarzenia o ponowny rozruch maszyny wirtualnej zawiera nazwę maszyny wirtualnej i przyczynę ponownego uruchomienia. Każde wydarzenie jest ograniczony do 64 KB danych.

## <a name="event-sourcespublishers"></a>Wydawcy źródła zdarzeń

Źródłem zdarzenia jest, gdy zdarzenie. Na przykład usługi Azure Storage jest źródła zdarzeń dla zdarzenia utworzony obiekt blob. Sieci szkieletowej maszyny Wirtualnej Azure jest źródła zdarzeń dla zdarzenia maszyny wirtualnej. Źródła zdarzeń są zobowiązani do publikowania zdarzeń w siatce zdarzeń.

## <a name="topics"></a>Tematy

Wydawcy kategoryzowanie zdarzeń do tematów. Temat zawiera punkt końcowy, gdzie wydawcy wysyła zdarzenia. Aby odpowiedzieć wybranych typów zdarzeń, subskrybentów zdecyduj, jakie tematy, aby subskrybować. Tematy zawierają również schematu zdarzeń, aby subskrybenci można dowiedzieć się, jak można odpowiednio korzystanie ze zdarzeń.

System tematy dotyczą wbudowanych udostępnionego przez usługi Azure. Tematy niestandardowe są tematy innych firm i aplikacji.

## <a name="event-subscriptions"></a>Subskrypcja zdarzeń

Subskrypcję nakazuje siatki zdarzeń na zdarzenia na temat otrzymywać jest subskrybenta.  Subskrypcja zawiera również informacji na temat sposobu zdarzenia powinna zostać dostarczona do subskrybenta.

## <a name="event-handlers"></a>Uchwyty zdarzeń

Z perspektywy siatki zdarzeń program obsługi zdarzeń jest miejscem wysyłania zdarzenia. Program obsługi ma niektórych dalszych kroków w celu przetworzenia zdarzenia.  Siatka zdarzeń obsługuje wiele typów subskrybenta. W zależności od typu subskrybenta siatki zdarzeń wykonuje różne mechanizmy gwarantujące dostarczania zdarzenia.  Dla programów obsługi zdarzeń elementu webhook HTTP, zdarzenie jest ponawiana dopóki obsługi zwraca kod stanu `200 – OK`. Dla kolejki magazynu Azure zdarzenia są ponawiana dopóki usługa kolejki jest może pomyślnie przetworzyć wypychania wiadomości do kolejki.

## <a name="filters"></a>Filtry

Gdy subskrypcja tematu, można filtrować zdarzenia, które są wysyłane do punktu końcowego. Można filtrować według typu zdarzenia lub wzorca podmiotu. Aby uzyskać więcej informacji, zobacz [schematu subskrypcji zdarzeń siatki](subscription-creation-schema.md).

## <a name="security"></a>Bezpieczeństwo

Zdarzenie bezpieczeństwo subskrybowanie tematów i publikowania tematów. Gdy subskrypcja, musi mieć odpowiednie uprawnienia na zasobów lub tematu. Podczas publikowania, musi mieć tokenu sygnatury dostępu Współdzielonego lub uwierzytelniania opartego na kluczu dla tematu. Aby uzyskać więcej informacji, zobacz [siatki zdarzeń zabezpieczeń i uwierzytelniania](security-authentication.md).

## <a name="failed-delivery"></a>Dostarczenie nie powiodło się

Gdy siatki zdarzenia nie mogą potwierdzić, że zdarzenie zostały odebrane przez punkt końcowy abonenta, redelivers zdarzenia. Aby uzyskać więcej informacji, zobacz [dostarczanie komunikatów zdarzeń siatki i ponów próbę](delivery-and-retry.md).

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md).
* Aby szybko rozpocząć korzystanie z siatki zdarzeń, zobacz [tworzenie i tras niestandardowych zdarzeń siatki zdarzeń Azure](custom-event-quickstart.md).