---
title: "Integrowanie z integracją dzienników Azure alerty Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł ułatwia szybkie wprowadzenie do integracji alerty Centrum zabezpieczeń z integracji dzienników Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2018
ms.author: barclayn
ms.openlocfilehash: 229f83d4d74837e506c30a0fd8c704da2b7dc12a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Integrowanie z integracją dzienników Azure alerty Centrum zabezpieczeń Azure
Wiele operacji zabezpieczeń i odpowiedzi na zdarzenia zespołów zależne rozwiązania Security Information and Event Management (SIEM) jako punkt początkowy klasyfikowane i badanie alertów zabezpieczeń. W przypadku integracji dziennika Azure alerty Centrum zabezpieczeń Azure można zintegrować z rozwiązania SIEM.

Integracja dzienników Azure obsługuje obecnie HP ArcSight, Splunk i IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować?
Azure tworzy szczegółowe rejestrowanie dla każdej usługi. Te dzienniki są sklasyfikowane jako:

* **Dzienniki sterowania i zarządzania nimi** , które powodują wgląd w operacji tworzenia Menedżera zasobów Azure, UPDATE i DELETE. Te zdarzenia płaszczyzny sterowania są udostępniane w Dzienniki aktywności platformy Azure
* **Dzienniki płaszczyzna danych** , które powodują wgląd w zdarzenia wywoływane, gdy przy użyciu zasobów platformy Azure. Przykładem jest dziennika zdarzeń systemu Windows, gdzie można uzyskać informacji o zabezpieczeniach zdarzenia z Podglądu zdarzeń bezpiecznego kanału. Zdarzenia płaszczyzna danych (które są generowane przez maszynę wirtualną lub usługę Azure) są udostępniane przez dzienników diagnostycznych platformy Azure.

Integracja dzienników Azure obsługuje obecnie integracji:

* Dzienniki maszyny Wirtualnej platformy Azure
* Dzienniki inspekcji Azure
* Alerty Centrum zabezpieczeń Azure

## <a name="install-azure-log-integration"></a>Zainstaluj integracji dzienników Azure
Pobierz [integracji dzienników Azure](https://www.microsoft.com/download/details.aspx?id=53324).

Usługa integracji dzienników Azure zbiera dane telemetryczne z komputera, na którym jest zainstalowany.  Zbierane dane z telemetrii jest:

* Wyjątków występujących podczas wykonywania integracji dzienników Azure
* Metryki dotyczące liczby zapytań i przetwarzania zdarzeń
* Statystyki, o które Azlog.exe są używane opcje wiersza polecenia

> [!NOTE]
> Zbieranie danych telemetrycznych można wyłączyć, usuwając zaznaczenie pola wyboru tej opcji.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integracja alerty Centrum zabezpieczeń i dzienników inspekcji platformy Azure
1. Otwórz wiersz polecenia i **cd** do **c:\Program Files\Microsoft Azure dziennika integracji**.
2. Uruchom **azlog createazureid** polecenie, aby utworzyć [Azure Active Directory — nazwy głównej usługi](../active-directory/active-directory-application-objects.md) w dzierżawcy usługi Azure Active Directory (AD), obsługujących subskrypcji platformy Azure.

    Zostanie wyświetlony monit o logowania do systemu Azure.

   > [!NOTE]
   > Musisz być właścicielem subskrypcji lub Współadministratorem subskrypcji.
   >
   >

    Uwierzytelnianie na platformie Azure odbywa się za pośrednictwem usługi Azure AD.  Tworzenie nazwy głównej usługi integracji dzienników Azure tworzy tożsamości usługi Azure AD, która uzyskuje dostęp do odczytu z subskrypcji platformy Azure.
3. Uruchom **autoryzować azlog <SubscriptionID>**  polecenie, aby przypisać dostęp czytelnika subskrypcji do nazwy głównej usługi utworzony w kroku 2. Jeśli nie określisz **SubscriptionID**, a następnie nazwy głównej usługi przypisano rolę czytelnika wszystkie subskrypcje, do których masz dostęp.

   > [!NOTE]
   > Zobaczysz ostrzeżenia, jeśli uruchomisz **autoryzować** polecenia natychmiast po **createazureid** polecenia. Brak niektórych opóźnienia między podczas tworzenia konta usługi Azure AD i jeśli konto nie jest dostępny do użycia. Jeśli Poczekaj około 10 sekund po uruchomieniu **createazureid** polecenie do uruchomienia **autoryzować** polecenia, a następnie nie powinna być widoczna te ostrzeżenia dotyczące.
   >
   >
4. Sprawdź następujące foldery, aby potwierdzić, że pliki JSON dziennika inspekcji są dostępne:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Sprawdź następujące foldery, aby upewnić się, że alerty Centrum zabezpieczeń, istnieją w nich:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. Skonfiguruj łącznik usługi przesyłania dalej pliku SIEM do odpowiedniego folderu. Procedura zależy od SIEM używasz.

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o Dzienniki aktywności platformy Azure i definicji właściwości, zobacz:

* [Operacje inspekcji w usłudze Resource Manager](../azure-resource-manager/resource-group-audit.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Uzyskaj najnowsze informacje zabezpieczeń platformy Azure i informacje.
