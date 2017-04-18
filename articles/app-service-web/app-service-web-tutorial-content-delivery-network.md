---
title: "Łączenie aplikacji sieci Web z usługą Content Delivery Network | Microsoft Docs"
description: "Połącz aplikację sieci Web z usługą Content Delivery Network w celu dostarczania plików statycznych z węzłów krawędzi."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: a12eab1f7bc4177f659771d58a58f749507c994c
ms.lasthandoff: 04/12/2017

---
# <a name="connect-a-web-app-to-a-content-delivery-network"></a>Łączenie aplikacji sieci Web z usługą Content Delivery Network

W tym samouczku utworzysz profil i punkt końcowy usługi Azure CDN w celu obsługi plików statycznych z aplikacji sieci Web za pośrednictwem lokalizacji punktów obecności usługi Azure CDN.

> [!TIP]
> Przejrzyj aktualną listę [lokalizacji punktów obecności usługi Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="step-1---login-to-azure-portal"></a>Krok 1 — Logowanie do witryny Azure Portal

Najpierw otwórz ulubioną przeglądarkę i przejdź do witryny [Azure Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Krok 2 — Tworzenie profilu CDN

Kliknij przycisk `+ New` znajdujący się w obszarze nawigacji po lewej stronie, a następnie kliknij pozycję **Sieć Web i mobilność**. W kategorii Sieć Web i mobilność wybierz pozycję **CDN**.

Określ wartości pozycji **Nazwa**, **Lokalizacja**, **Grupa zasobów**, **Warstwa cenowa**, a następnie kliknij przycisk **Utwórz**.

Otwórz centrum grup zasobów w obszarze nawigacji po lewej stronie i wybierz pozycję **myResourceGroup**. Z listy zasobów wybierz pozycję **myCDNProfile**.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Krok 3 — Tworzenie punktu końcowego CDN

Kliknij pozycję `+ Endpoint` wśród poleceń obok pola wyszukiwania, co spowoduje uruchomienie bloku tworzenia punktu końcowego.

Określ wartości pozycji **Nazwa**, **Typ źródła**, **Nazwa hosta źródła**, a następnie kliknij przycisk **Dodaj**.

Punkt końcowy zostanie utworzony. Po utworzeniu punktu końcowego usługi Content Delivery Network stan zostanie zaktualizowany na **uruchomiono**.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---leveraging-cdn"></a>Krok 4 — Korzystanie z usługi CDN

Teraz, gdy punkt końcowy jest uruchomiony, sprawdźmy, czy zawartość jest dostępna na serwerze punktów obecności, przechodząc do pliku statycznego na serwerze sieci Web, a następnie zmieniając nazwę hosta z `http://<app_name>.azurewebsites.net/path/to-static-file` na `http://<endpoint_name>.azureedge.net/path/to-static-file`.

![app-service-web-url-to-cdn-endpoint-url](media/app-service-web-tutorial-content-delivery-network/app-service-web-url-to-cdn-endpoint-url.png)

## <a name="next-steps"></a>Następne kroki


