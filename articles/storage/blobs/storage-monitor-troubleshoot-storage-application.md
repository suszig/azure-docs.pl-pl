---
title: "Monitorowanie i rozwiązywanie problemów aplikacji magazynu w chmurze na platformie Azure | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów i monitorowanie aplikacji w chmurze, należy użyć narzędzia diagnostyczne, metryki i alerty."
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: db88c331f79d83e0124519f8b6dbb34514b456dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Monitorowanie i rozwiązywanie problemów aplikacji magazynu w chmurze

Ten samouczek ma cztery i końcowe część serii. Sposób monitorowania i rozwiązywania problemów z aplikacji magazynu w chmurze.

W czterech części serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Włącz rejestrowanie i metryki
> * Włącz alerty błędów autoryzacji
> * Uruchom test ruchu z niepoprawne tokeny sygnatury dostępu Współdzielonego
> * Pobieranie i analizowanie dzienników

[Usługa Azure storage analytics](../common/storage-analytics.md) udostępnia dane logowania i metryki dla konta magazynu. Te dane zapewnia wgląd w kondycję Twojego konta magazynu. Aby można było widoczne na koncie magazynu, należy skonfigurować zbierania danych. Ten proces obejmuje włączenie rejestrowania, Konfigurowanie metryki i włączenie alertów.

Rejestrowanie i metryki z konta magazynu są włączone z **diagnostyki** kartę w portalu Azure. Istnieją dwa typy metryki. **Łączny** metryki zbieranie wartości procentowe wejście/wyjście, dostępności, opóźnienia i Powodzenie. Te metryki są agregowane dla obiektu blob, kolejek, tabel i usług plików. **Dla interfejsu API** zbiera ten sam zestaw metryki dla każdej operacji magazynu w interfejsie API usługi Azure Storage. Rejestrowanie magazynu umożliwia rekordów szczegółów dla żądań pomyślnie i niepomyślnie na koncie magazynu. Dzienniki te umożliwiają szczegóły odczytu, zapisu i usuwania operacji względem programu Azure tabel, kolejek i obiektów blob. Umożliwiają one również zobacz przyczyny nieudanych żądań, takich jak przekroczeń limitu czasu, ograniczania i błędów autoryzacji.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

## <a name="turn-on-logging-and-metrics"></a>Włącz rejestrowanie i metryki

Wybierz z menu po lewej stronie **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz konto magazynu na liście zasobów.

W obszarze **diagnostyki** ustawić **stan** do **na**. Upewnij się, **obiektu Blob agregacji metryki**, **obiektu Blob dla metryki interfejsu API**, i **obiektu Blob dzienniki** są włączone.

Po zakończeniu kliknij przycisk **Zapisz**

![Okienko diagnostyki](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Włącz alerty

Alerty umożliwiają administratorom w wiadomościach e-mail lub wywołać elementu webhook w oparciu metryki naruszenie progu. W tym przykładzie zostanie włączone alertu dotyczącego `SASClientOtherError` metryki.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Przejdź do konta magazynu w portalu Azure

Wybierz z menu po lewej stronie **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz konto magazynu na liście zasobów.

W obszarze **monitorowanie** zaznacz **reguły alertów**.

Wybierz **+ Dodaj alert**w obszarze **dodać regułę alertu**, wprowadź wymagane informacje. Wybierz `SASClientOtherError` z **Metryka** listy rozwijanej.

![Okienko diagnostyki](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Symulacja błędu

Aby symulować prawidłowy alertu, można spróbować żądania nieistniejącego obiektu blob z konta magazynu. Aby to zrobić, Zamień `<incorrect-blob-name>` wartości z wartością, która nie istnieje. Uruchom Poniższy przykładowy kod kilka razy do symulowania nieudane żądania obiektu blob.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

Poniższa ilustracja jest alert przykład opiera się poza symulowane awarii został uruchomiony z poprzedniego przykładu.

 ![Przykład alertu](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Pobierz i Wyświetl dzienniki

Dzienniki magazynu przechowywania danych w zestawie obiektów blob w kontenerze obiektu blob o nazwie **$logs** na koncie magazynu. Ten kontener nie jest wyświetlany w przypadku listy wszystkich kontenerów obiektów blob na koncie, ale można wyświetlić jego zawartość, jeśli bezpośredni dostęp.

W tym scenariuszu, należy użyć [programu Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) wchodzić w interakcje z kontem magazynu platformy Azure.

### <a name="download-microsoft-message-analyzer"></a>Pobierz program Microsoft Message Analyzer

Pobierz [programu Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) i zainstaluj aplikację.

Uruchom aplikację i wybierz **pliku** > **Otwórz** > **z innych źródeł pliku**.

W **selektor plików** okno dialogowe, wybierz opcję **+ Dodaj połączenie Azure**. Wprowadź w Twojej **nazwy konta magazynu** i **klucz konta** i kliknij przycisk **OK**.

![Microsoft komunikatów analizatora — Dodawanie magazynu Azure w oknie dialogowym połączenia](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Po nawiązaniu połączenia rozwiń kontenery w drzewie magazynu Widok, aby wyświetlić obiekty BLOB dziennika. Wybierz najnowszy dziennika i kliknij przycisk **OK**.

![Microsoft komunikatów analizatora — Dodawanie magazynu Azure w oknie dialogowym połączenia](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Na **nowej sesji** okna dialogowego, kliknij przycisk **Start** Aby wyświetlić dziennik.

Po otwarciu dziennika można wyświetlać zdarzenia magazynu. Jak widać na poniższej ilustracji, wystąpił `SASClientOtherError` wyzwalane na koncie magazynu. Aby uzyskać dodatkowe informacje na temat rejestrowania magazynu, odwiedź stronę [analityka magazynu](../common/storage-analytics.md).

![Microsoft Message Analyzer — wyświetlanie zdarzeń](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Eksplorator usługi Storage](https://azure.microsoft.com/features/storage-explorer/) jest innego narzędzia, który może służyć do interakcji z kontami magazynu, w tym **$logs** kontenera i dzienniki, które są zawarte w nim.

## <a name="next-steps"></a>Następne kroki

W części cztery i końcowa część serii przedstawiono sposób monitorowania i rozwiązywania problemów z konta magazynu, np.:

> [!div class="checklist"]
> * Włącz rejestrowanie i metryki
> * Włącz alerty błędów autoryzacji
> * Uruchom test ruchu z niepoprawne tokeny sygnatury dostępu Współdzielonego
> * Pobieranie i analizowanie dzienników

Wykonaj to łącze, aby wyświetlić przykłady wbudowanych magazynu.

> [!div class="nextstepaction"]
> [Przykłady skryptów magazynu Azure](storage-samples-blobs-cli.md)