---
title: "Tworzenie i zarządzanie nimi połączeń hybrydowych | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzenie połączenia hybrydowego, zarządzanie połączenia i zainstaluj Menedżera połączeń hybrydowych. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 7b8b9072d0e2fd054ca07873c0a9ce772dc2941e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Tworzenie połączeń hybrydowych i zarządzanie nimi

> [!IMPORTANT]
> Połączenia hybrydowe BizTalk zostały wycofane i zastąpione połączeniami hybrydowymi usługi App Service. Aby uzyskać więcej informacji, m.in. o tym, jak zarządzać istniejącymi połączeniami hybrydowymi BizTalk, zobacz artykuł [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) (Połączenia hybrydowe usługi Azure App Service).


## <a name="overview-of-the-steps"></a>Omówienie kroków procesu
1. Tworzenie połączenia hybrydowego wprowadzając **nazwy hosta** lub **FQDN** zasobów lokalnych w sieci prywatnej.
2. Łączenie z aplikacjami sieci web platformy Azure lub usługi Azure mobile apps do połączenia hybrydowego.
3. Zainstaluj Menedżera połączeń hybrydowych na zasób lokalną i Połącz z określonego połączenia hybrydowego. Azure portal zapewnia jednym kliknięciem do zainstalowania i połączenia.
4. Zarządzanie połączeń hybrydowych i ich kluczy połączenia.

Ten temat zawiera następujące kroki. 

> [!IMPORTANT]
> Istnieje możliwość ustawić adres IP punktu końcowego połączenia hybrydowego. Jeśli używasz adresu IP, może lub nie może skontaktować się zasób lokalną, w zależności od klienta. Połączenia hybrydowe zależy od klienta podczas wyszukiwania DNS. W większości przypadków **klienta** jest kod aplikacji. Jeśli klient nie przeprowadza wyszukiwania DNS (go nie próbuje rozpoznać adresu IP, tak jakby był on nazwę domeny (x.x.x.x)), a następnie ruchu nie są wysyłane za pośrednictwem połączenia hybrydowego.
> 
> Na przykład (pseudocode), należy zdefiniować **10.4.5.6** jako hosta lokalnego:
> 
> **Działa następujący scenariusz:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Nie działa następujący scenariusz:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Tworzenie połączenia hybrydowego
W portalu Azure za pomocą aplikacji sieci Web można tworzyć połączenie hybrydowe **lub** przy użyciu usługi BizTalk Services. 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

**Aby utworzyć połączeń hybrydowych w usługi BizTalk Services**:

1. Zaloguj się do [klasycznej witryny Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W lewym okienku nawigacji, wybierz **usługi BizTalk Services** , a następnie wybierz usługę BizTalk. 
   
    Jeśli nie masz istniejącej usługi BizTalk, możesz [Utwórz usługę BizTalk](biztalk-provision-services.md).
3. Wybierz **połączeń hybrydowych** karty:  
   ![Karta połączenia hybrydowego][HybridConnectionTab]
4. Wybierz **Tworzenie połączenia hybrydowego** lub wybierz **dodać** na pasku zadań. Wprowadź następujące informacje:
   
   | Właściwość | Opis |
   | --- | --- |
   | Nazwa |Nazwa połączenia hybrydowego muszą być unikatowe i nie może być taką samą nazwę jak usługi BizTalk. Można wprowadź dowolną nazwę, ale przeznaczone z jej celem. Przykłady:<br/><br/>Lista płac*SQLServer*<br/>SupplyList*SharepointServer*<br/>Klienci*OracleServer* |
   | Nazwa hosta |Wprowadź nazwę hosta w pełni kwalifikowaną nazwę hosta lub adres IPv4 zasobów lokalnych. Przykłady:<br/><br/>Mójserwersql<br/>*Mójserwersql*. *Domeny*. corp.*NazwaFirmy*com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *NazwaFirmy*com<br/>10.100.10.10<br/><br/>Użycie adresu IPv4, należy pamiętać, że kod klienta lub aplikacji nie może rozpoznać adresu IP. Zobacz ważne należy zwrócić uwagę na początku tego tematu. |
   | Port |Wprowadź numer portu zasobów lokalnych. Na przykład jeśli używasz aplikacji sieci Web, wprowadź port 80 lub port 443. Jeśli używasz programu SQL Server, wprowadź port 1433. |
5. Zaznacz pole wyboru, aby ukończyć instalację. 

#### <a name="additional"></a>Informacje dodatkowe
* Można tworzyć wiele połączeń hybrydowych. Zobacz [usługi BizTalk Services: wykres wersje](biztalk-editions-feature-chart.md) liczbę dozwolonych połączeń. 
* Każde połączenie hybrydowe jest tworzony przy użyciu pary ciągów połączeń: aplikacja klucze tego WYSYŁANIA i lokalnymi klucze, które NASŁUCHUJĄ. Każda para ma podstawowego i pomocniczego klucza. 

## <a name="LinkWebSite"></a>Łączenie aplikacji mobilnej lub aplikacji sieci Web usługi aplikacji Azure
Aby połączyć aplikację sieci Web lub aplikacji mobilnej w usłudze Azure App Service istniejące połączenie hybrydowe, wybierz **Użyj istniejącego połączenia hybrydowego** w bloku połączeń hybrydowych było możliwe. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Zainstaluj Menedżera połączeń hybrydowych lokalnej
Po utworzeniu połączenia hybrydowego zasobu lokalnego należy zainstalować Menedżera połączeń hybrydowych. Można go pobrać z aplikacji sieci web platformy Azure lub usługi BizTalk. Usługi BizTalk Services kroki: 

1. Zaloguj się do [klasycznej witryny Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W lewym okienku nawigacji, wybierz **usługi BizTalk Services** , a następnie wybierz usługę BizTalk. 
3. Wybierz **połączeń hybrydowych** karty:  
   ![Karta połączenia hybrydowego][HybridConnectionTab]
4. Na pasku zadań wybierz **instalacji lokalnej**:  
   ![Ustawienia lokalnych][HCOnPremSetup]
5. Wybierz **zainstalować i skonfigurować** do uruchamiania lub pobrać Menedżera połączeń hybrydowych w systemie lokalnym. 
6. Wybierz znacznik wyboru, aby rozpocząć instalację. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Informacje dodatkowe
* Menedżera połączeń hybrydowych można zainstalować w następujących systemach operacyjnych:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + i Windows Management Framework 4.0 + wymagane)
  * Windows Server 2012 (Windows Management Framework 4.0 + wymagane)
  * Windows Server 2012 R2
* Po zainstalowaniu Menedżera połączeń hybrydowych, są następujące operacje: 
  
  * Połączenia hybrydowe hostowanej na platformie Azure jest automatycznie konfigurowany do użyto parametrów połączenia z głównej aplikacji. 
  * Zasobu lokalnego jest automatycznie konfigurowany do Użyj podstawowego parametrów połączenia lokalnego.
* Menedżera połączeń hybrydowych, należy użyć lokalnej prawidłowe parametry połączenia do autoryzacji. Aplikacje sieci Web Azure lub Mobile Apps, musisz użyć ciągu połączenia prawidłową aplikację do autoryzacji.
* Można skalować połączeń hybrydowych było możliwe, instalując inne wystąpienie Menedżera połączeń hybrydowych na innym serwerze. Skonfiguruj odbiornik lokalnych do korzystania z tego samego adresu jako pierwszy odbiornik lokalnymi. W takiej sytuacji ruch jest losowo rozproszone (działanie okrężne) między odbiorników active lokalnymi. 

## <a name="ManageHybridConnection"></a>Zarządzanie połączeń hybrydowych
Aby zarządzać połączeń hybrydowych, można:

* Użyj portalu Azure, przejdź do usługi BizTalk. 
* Użyj [interfejsów API REST](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopiuj/regenerate parametry połączenia hybrydowego
1. Zaloguj się do [klasycznej witryny Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W lewym okienku nawigacji, wybierz **usługi BizTalk Services** , a następnie wybierz usługę BizTalk. 
3. Wybierz **połączeń hybrydowych** karty:  
   ![Karta połączenia hybrydowego][HybridConnectionTab]
4. Wybierz połączenie hybrydowe. Na pasku zadań wybierz **Zarządzanie połączenia**:  
   ![Zarządzanie opcjami][HCManageConnection]
   
    **Zarządzanie połączenia** zawiera parametry połączenia aplikacji i lokalnie. Możesz skopiować parametry połączenia lub ponownie wygenerować klucza dostępu używany w parametrach połączenia. 
   
    **W przypadku wybrania Regenerate**, udostępniony klucz dostępu używany w parametrach połączenia zostanie zmieniona. Wykonaj następujące czynności:
   
   * W klasycznym portalu Azure, wybierz **synchronizacji kluczy** w aplikacji Azure.
   * Uruchom ponownie **lokalnej instalacji**. Podczas ponownego uruchamiania Instalatora lokalnego, zasób lokalną jest automatycznie konfigurowany do Użyj ciągu zaktualizowane połączeniem podstawowym.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Zastosowanie zasad grupy do kontrolowania zasobów lokalnych, używany przez połączenie hybrydowe
1. Pobierz [Szablony administracyjne Menedżera połączeń hybrydowych](http://www.microsoft.com/download/details.aspx?id=42963).
2. Wyodrębnij pliki.
3. Na komputerze, który modyfikuje zasad grupy wykonaj następujące czynności:  
   
   * Kopiuj. ADMX plików do *%WINROOT%\PolicyDefinitions* folderu.
   * Kopiuj. ŚILS plików do *%WINROOT%\PolicyDefinitions\en-us* folderu.

Po skopiowaniu służy Edytor zasad grupy do zmiany zasad.

## <a name="next"></a>Następne kroki
[Omówienie połączeń hybrydowych](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Zobacz też
[Interfejs API REST zarządzania usługi BizTalk Services na platformie Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Editions Chart (Usługa BizTalk Services: zestawienie wersji)](biztalk-editions-feature-chart.md)  
[Utwórz usługę BizTalk przy użyciu klasycznego portalu Azure](biztalk-provision-services.md)  
[BizTalk Services: Dashboard, Monitor and Scale tabs (Usługa BizTalk Services: karty Pulpit nawigacyjny, Monitor i Skalowanie)](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
