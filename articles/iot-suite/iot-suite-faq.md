---
title: Pakiet IoT Azure — często zadawane pytania | Microsoft Docs
description: Często zadawane pytania dotyczące Pakietu IoT
services: ''
suite: iot-suite
documentationcenter: ''
author: aguilaaj
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2016
ms.author: araguila

---
# Często zadawane pytania dotyczące Pakietu IoT
### Jaka jest różnica między usunięciem grupy zasobów w witrynie Azure Portal a usunięciem wstępnie skonfigurowanego rozwiązania w witrynie azureiotsuite.com?
* Usunięcie wstępnie skonfigurowanego rozwiązania w witrynie [azureiotsuite.com][lnk-azureiotsuite] powoduje usunięcie wszystkich zasobów, dla których przeprowadzono aprowizację podczas tworzenia tego rozwiązania. Jeśli do grupy dodano inne zasoby, one również zostaną usunięte. 
* W przypadku usunięcia grupy zasobów w witrynie [Azure Portal][lnk-azure-portal] usuwane są tylko zasoby z tej grupy. Trzeba jeszcze dodatkowo usunąć aplikację usługi Azure Active Directory skojarzoną z wstępnie skonfigurowanym rozwiązaniem w [klasycznym Azure Portal][lnk-classic-portal]

### Ile wystąpień usługi DocumentDB można aprowizować w ramach subskrypcji?
Pięć. Aby zwiększyć ten limit, można utworzyć [żądanie pomocy technicznej dotyczące platformy Azure][link-azuresupportticket], ale domyślnie dla jednej subskrypcji można aprowizować tylko pięć wystąpień usługi DocumentDB. W związku z tym dana subskrypcja pozwala aprowizować maksymalnie pięć wstępnie skonfigurowanych rozwiązań do monitorowania zdalnego.

### Ile bezpłatnych interfejsów API usługi Mapy Bing można aprowizować w ramach subskrypcji?
Dwa. W ramach subskrypcji można utworzyć tylko dwa bezpłatne interfejsy API usługi Mapy Bing. Rozwiązanie do monitorowania zdalnego jest domyślnie aprowizowane przy użyciu bezpłatnego interfejsu API usługi Mapy Bing. W związku z tym niemodyfikowana subskrypcja pozwala aprowizować maksymalnie dwa rozwiązania do monitorowania zdalnego.

### Mam wdrożone rozwiązanie do monitorowania zdalnego z mapą statyczną. Jak dodać interaktywną mapę Bing?
1. Korzystając z witryny [Azure Portal][lnk-azure-portal], pobierz element QueryKey interfejsu API usługi Mapy Bing dla przedsiębiorstw: 
   
   1. W witrynie [Azure Portal][lnk-azure-portal] przejdź do grupy zasobów, w której znajduje się interfejs API usługi Mapy Bing dla przedsiębiorstw.
   2. Kliknij kolejno pozycje Wszystkie ustawienia i Zarządzanie kluczami. 
   3. Zostaną wyświetlone dwa klucze: MasterKey i QueryKey. Skopiuj wartość elementu QueryKey.
      
      > [!NOTE]
      > Nie masz konta interfejsu API usługi Mapy Bing dla przedsiębiorstw? Możesz je utworzyć w witrynie [Azure Portal][lnk-azure-portal]. W tym celu kliknij pozycję + Nowe, wyszukaj interfejs API usługi Mapy Bing dla przedsiębiorstw i postępuj zgodnie z instrukcjami.
      > 
      > 
2. Pobierz najnowszy kod z repozytorium [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Przeprowadź wdrożenie lokalne lub wdrożenie w chmurze, postępując zgodnie ze wskazówkami dotyczącymi wdrażania przy użyciu wiersza polecenia, które są dostępne w folderze /docs/ w repozytorium. 
4. Po wykonaniu wdrożenia lokalnego lub wdrożenia w chmurze poszukaj w folderze głównym pliku *.user.config utworzonego podczas wdrażania. Otwórz ten plik w edytorze tekstu. 
5. Zmień następujący wiersz, dodając wartość skopiowaną z elementu QueryKey: 
   
   `<setting name="MapApiQueryKey" value="" />`

### Czy można utworzyć wstępnie skonfigurowane rozwiązanie w przypadku korzystania z platformy Microsoft Azure dla programu DreamSpark?
Obecnie nie można tworzyć wstępnie skonfigurowanych rozwiązań za pomocą konta [platformy Microsoft Azure dla programu DreamSpark][lnk-dreamspark]. Jednak w ciągu kilku minut można założyć [konto bezpłatnej wersji próbnej platformy Azure][lnk-30daytrial], które umożliwia utworzenie wstępnie skonfigurowanego rozwiązania.

### Jak usunąć dzierżawę usługi AAD?
Zobacz wpis Erica Golpe'a na blogu [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant] (Przewodnik po usuwaniu dzierżawy usługi Azure AD).

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx



<!--HONumber=jun16_HO2-->


