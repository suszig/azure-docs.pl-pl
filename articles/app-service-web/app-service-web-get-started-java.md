---
title: "Tworzenie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając prostą aplikację w języku Java."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: cephalin;robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 2673a9c0d91510756a97b2dba3801d2925905c9a
ms.lasthandoff: 04/21/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć pierwszą aplikację sieci Web w języku Java w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md). Po zakończeniu pracy z tym samouczkiem będziesz mieć prostą aplikację Java działającą i uruchomioną w chmurze.

![Przechodzenie do aplikacji sieci Web](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym samouczku pokazano, jak utworzyć i wdrożyć aplikację sieci Web w języku Java przy użyciu środowiska Eclipse IDE for Java EE Developers. Jeśli nie masz zainstalowanego środowiska Eclipse, możesz pobrać je bezpłatnie z witryny http://www.eclipse.org/.

Aby uprościć proces publikowania aplikacji sieci Web w języku Java na platformie Azure, w krokach niniejszego samouczka będzie używany [Zestaw narzędzi platformy Azure dla środowiska Eclipse](/azure/azure-toolkit-for-eclipse). Aby uzyskać instrukcje dotyczące sposobu instalowania tego zestawu narzędzi, zobacz [Instalowanie zestawu narzędzi platformy Azure dla programu Eclipse](/azure/azure-toolkit-for-eclipse-installation).

> [!NOTE]
>
> W celu wykonania kroków tego samouczka można także skorzystać z rozwiązania [IntelliJ IDEA](https://www.jetbrains.com/idea/) firmy JetBrains. Kilka kroków może być nieco innych dla tego środowiska programistycznego, choć dostępny jest również [Zestaw narzędzi platformy Azure dla środowiska IntelliJ](/azure/azure-toolkit-for-intellij), przy użyciu którego można uprościć proces publikowania dla tego środowiska IDE.
>

Do wykonania czynności w tym samouczku potrzebna będzie także subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub założyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Tworzenie dynamicznego projektu sieci Web w środowisku Eclipse

W środowisku IDE programu Eclipse kliknij menu **File** (Plik), polecenie **New** (Nowy), a następnie pozycję **Dynamic Web Project** (Dynamiczny projekt sieci Web).

![Nowy dynamiczny projekt sieci Web](./media/app-service-web-get-started-java/file-new-dynamic-web-project-menu.png)

Gdy pojawi się okno dialogowe Dynamic Web Project (Dynamiczny projekt sieci Web), podaj nazwę aplikacji **MyFirstJavaOnAzureWebApp**, a następnie kliknij przycisk **Finish** (Zakończ).
   
![Okno dialogowe Dynamic Web Project (Dynamiczny projekt sieci Web)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

> [!NOTE]
>
> Jeśli masz zainstalowane lokalne środowisko uruchomieniowe, takie jak [Apache Tomcat](https://tomcat.apache.org/), możesz je określić w polu **Target runtime** (Docelowe środowisko uruchomieniowe).
>

Po utworzeniu dynamicznego projektu sieci Web dodaj nową stronę JSP: rozwiń projekt w obszarze Project Explorer (Eksplorator projektów), kliknij prawym przyciskiem myszy folder **WebContent**, kliknij polecenie **New** (Nowy), a następnie kliknij pozycję **JSP File** (Plik JSP).

![Menu New JSP File (Menu Nowy — Plik JSP)](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Gdy pojawi się okno dialogowe New JSP File (Nowy plik JSP), podaj nazwę pliku **index.jsp**, pozostaw folder **MyFirstJavaOnAzureWebApp/WebContent** jako folder nadrzędny, a następnie kliknij przycisk **Next** (Dalej).

![Okno dialogowe New JSP File (Nowy plik JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Na drugiej stronie okna dialogowego New JSP File (Nowy plik JSP) podaj nazwę pliku **index.jsp**, pozostaw folder **MyFirstJavaOnAzureWebApp/WebContent** jako folder nadrzędny, a następnie kliknij przycisk **Next** (Dalej).

![Okno dialogowe New JSP File (Nowy plik JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-2.png)

Po otwarciu nowej strony w środowisku Eclipse zastąp istniejącą sekcję `<body></body>` następującym kodem:

```jsp
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
```

Zapisz zmiany na stronie.

![Edytowanie kodu JSP](./media/app-service-web-get-started-java/creating-index-jsp-page.png)

## <a name="publish-your-web-app-to-azure"></a>Publikowanie aplikacji sieci Web na platformie Azure

W celu wdrożenia aplikacji sieci Web na platformie Azure skorzystasz z kilku funkcji oferowanych przez zestaw narzędzi platformy Azure dla środowiska Eclipse.

Aby rozpocząć proces publikowania, użyj jednej z następujących metod:

* W środowisku Eclipse kliknij prawym przyciskiem myszy projekt w obszarze **Project Explorer** (Eksplorator projektów), kliknij polecenie **Azure**, a następnie kliknij pozycję **Publish as Azure Web App** (Publikuj jako aplikację sieci Web platformy Azure).

   ![Menu kontekstowe Publish as Azure Web App (Publikuj jako aplikację sieci Web platformy Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

* Kliknij ikonę **Publish** (Publikuj) na pasku narzędzi środowiska Eclipse, a następnie kliknij przycisk **Publish as Azure Web App** (Publikuj jako aplikację sieci Web platformy Azure).

   ![Menu rozwijane Publish as Azure Web App (Publikuj jako aplikację sieci Web platformy Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-drop-down-menu.png)

Jeśli jeszcze nie zostało wykonane logowanie na koncie platformy Azure, zostanie wyświetlony monit o zalogowanie się. Aby to zrobić, wykonaj następujące kroki:

1. Istnieją dwie różne opcje logowania się na konto platformy Azure. W tym samouczku wybierz opcję **Interactive** (Interaktywne).

   ![Okno dialogowe logowania do platformy Azure](./media/app-service-web-get-started-java/azure-signin-dialog-box.png)

1. Wprowadź swoje poświadczenia platformy Azure, a następnie kliknij przycisk **Sign in** (Zaloguj się).

   ![Okno dialogowe logowania do platformy Azure](./media/app-service-web-get-started-java/azure-login-dialog-box.png)

1. Wybierz subskrypcje platformy Azure, a następnie kliknij pozycję **Select** (Wybierz).

   ![Okno dialogowe logowania do platformy Azure](./media/app-service-web-get-started-java/select-azure-subscriptions-dialog-box.png)

> [!NOTE]
>
> Szczegółowe instrukcje dotyczące metod logowania **Interactive** (Interaktywne) i **Automated** (Automatyczne) są dostępne w artykule [Azure Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Instrukcje logowania na platformie Azure dla zestawu narzędzi platformy Azure dla środowiska Eclipse).
>

Po zalogowaniu się na koncie platformy Azure zostanie wyświetlone okno dialogowe **Deploy Web App** (Wdrażanie aplikacji sieci Web). Jeśli po raz pierwszy publikujesz aplikację sieci Web w usłudze Azure, na liście nie powinno być żadnych usług App Service. W takim przypadku, lub jeśli chcesz utworzyć nową usługę App Service, następnym krokiem będzie utworzenie nowej usługi App Service. Aby to zrobić, kliknij przycisk **Create** (Utwórz).

![Okno dialogowe Deploy Web App (Wdrażanie aplikacji sieci Web)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

Po wyświetleniu okna dialogowego **Create App Service** (Tworzenie usługi App Service) należy podać następujące dane początkowe:

* Unikatową nazwę aplikacji sieci Web, która stanie się adresem DNS aplikacji sieci Web; na przykład dla nazwy **MyJavaWebApp** adresem będzie *myjavawebapp.azurewebsites.net*.

* Kontener sieci Web, którego będzie używać aplikacja, na przykład **Newest Tomcat 8.5**.

* Swoją subskrypcję platformy Azure.

   ![Okno dialogowe Create App Service (Tworzenie usługi App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

Jeśli nie ma żadnych istniejących planów usługi App Service lub jeśli chcesz utworzyć nowy plan usługi, musisz podać następujące informacje:

* Unikatową nazwę nowego planu usługi (ta nazwa będzie wyświetlana podczas publikowania aplikacji sieci Web w przyszłości przy użyciu zestawu narzędzi platformy Azure i będzie widoczna w witrynie [Azure Portal](https://portal.azure.com) podczas zarządzania kontem).

* Lokalizację geograficzną, w której zostanie utworzony plan usługi.

* Warstwę cenową dla planu usługi.

   ![Tworzenie planu usługi App Service](./media/app-service-web-get-started-java/create-app-service-plan.png)

Następnie kliknij kartę **Resource group** (Grupa zasobów). Jeśli nie masz żadnych istniejących grup zasobów lub jeśli chcesz utworzyć nową, podaj unikatową nazwę dla nowej grupy zasobów. W przeciwnym razie wybierz istniejącą grupę zasobów z menu rozwijanego.

![Tworzenie planu usługi App Service](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

Na koniec kliknij kartę **JDK**. Na tej karcie znajduje się kilka opcji, które umożliwiają deweloperom określenie zestawów JDK innych firm lub niestandardowych, ale na potrzeby tego samouczka wybierz opcję **Default** (Domyślne), a następnie kliknij przycisk **Create** (Utwórz).

![Tworzenie planu usługi App Service](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)

Zestaw narzędzi platformy Azure rozpocznie tworzenie nowej usługi App Service i wyświetli okno dialogowe postępu podczas przetwarzania.

![Pasek postępu tworzenia usługi App Service](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

Po utworzeniu nowej usługi App Service ostatnią opcją do wybrania jest to, czy aplikacja sieci Web ma być wdrożona w katalogu głównym nowej witryny sieci Web. Jeśli na przykład masz usługę App Service w lokalizacji *wingtiptoys.azurewebsites.net* i nie wybierzesz wdrożenia w katalogu głównym, wówczas aplikacja sieci Web o nazwie **MyFirstJavaOnAzureWebApp** zostanie wdrożona w lokalizacji *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Wdrażanie aplikacji sieci Web w katalogu głównym](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Po zakończeniu wszystkich wcześniejszych kroków kliknij przycisk **Deploy** (Wdróż), aby opublikować aplikację sieci Web na platformie Azure.

![Wdrażanie aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

Gratulacje! Aplikacja sieci Web została pomyślnie wdrożona na platformie Azure. Teraz można wyświetlić podgląd aplikacji sieci Web w witrynie sieci Web platformy Azure:

![Przechodzenie do aplikacji sieci Web](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="updating-your-web-app"></a>Aktualizowanie aplikacji sieci Web

Po pomyślnym opublikowaniu aplikacji sieci Web na platformie Azure aktualizowanie aplikacji sieci Web jest znacznie prostszym procesem. Następujące kroki przeprowadzą Cię przez proces publikowania zmian w aplikacji sieci Web.

Najpierw należy zmienić przykładowy kod JSP z wcześniejszego, tak aby tytuł został zastąpiony dzisiejszą datą:

```jsp
<%@ page
    language="java"
    contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"
    import="java.text.SimpleDateFormat"
    import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<% SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd"); %>
<title><% out.println(date.format(new Date())); %></title>
</head>
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
</html>
```

![Aktualizowanie kodu JSP](./media/app-service-web-get-started-java/updating-index-jsp-page.png)

Po zapisaniu zmian strony kliknij prawym przyciskiem myszy projekt w obszarze **Project Explorer** (Eksplorator projektów), kliknij pozycję **Azure**, a następnie kliknij pozycję **Publish as Azure Web App** (Publikuj jako aplikację sieci Web platformy Azure).

![Publikowanie zaktualizowanej aplikacji sieci Web](./media/app-service-web-get-started-java/publish-updated-web-app-context-menu.png)

Gdy zostanie wyświetlone okno dialogowe **Deploy Web App** (Wdrażanie aplikacji sieci Web), będzie w nim wyświetlana wcześniejsza usługa App Service. Aby zaktualizować aplikację sieci Web, wystarczy wyróżnić usługę App Service, a następnie kliknąć pozycję **Deploy** (Wdróż) w celu opublikowania zmian.

![Wdrażanie aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

> [!NOTE]
>
> Jeśli aplikacja sieci Web jest wdrażana w katalogu głównym usługi App Service, konieczne będzie ponowne zaznaczenie opcji **Deploy to root** (Wdróż w katalogu głównym) każdorazowo przy publikowaniu zmian.
>

Po opublikowaniu zmian można zauważyć, że tytuł strony został zmieniony na bieżącą datę w przeglądarce.

![Przechodzenie do aplikacji sieci Web](./media/app-service-web-get-started-java/browse-web-app-2.png)

## <a name="deleting-your-web-app"></a>Usuwanie aplikacji sieci Web

Aby usunąć aplikację sieci Web, możesz użyć programu **Azure Explorer**, który jest częścią zestawu narzędzi platformy Azure. Jeśli widok **Azure Explorer** nie jest jeszcze wyświetlany w środowisku Eclipse, skorzystaj z następujących kroków, aby go wyświetlić:

1. Kliknij menu **Window** (Okno), kliknij polecenie **Show View** (Pokaż widok), a następnie kliknij pozycję **Other** (Inne).

   ![Menu Show View (Pokaż widok)](./media/app-service-web-get-started-java/show-azure-explorer-view-1.png)

2. Po wyświetleniu okna dialogowego **Show View** (Wyświetlanie widoku) wybierz pozycję **Azure Explorer** i kliknij przycisk **OK**.

   ![Okno dialogowe Show View (Wyświetlanie widoku)](./media/app-service-web-get-started-java/show-azure-explorer-view-2.png)

Aby usunąć aplikację sieci Web z poziomu programu Azure Explorer, rozwiń węzeł **Web Apps**, kliknij prawym przyciskiem myszy aplikację sieci Web i wybierz polecenie **Delete** (Usuń).

![Usuwanie aplikacji sieci Web](./media/app-service-web-get-started-java/delete-web-app-context-menu.png)

Gdy pojawi się monit o usunięcie aplikacji sieci Web, kliknij przycisk **OK**.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o zestawach narzędzi platformy Azure dla środowisk IDE języka Java, skorzystaj z następujących linków:

* [Zestaw narzędzi platformy Azure dla środowiska Eclipse (ten artykuł)](../azure-toolkit-for-eclipse.md)
  * [What's New in the Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-whats-new.md) (Co nowego w zestawie narzędzi platformy Azure dla środowiska Eclipse)
  * [Installing the Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md) (Instalowanie zestawu narzędzi platformy Azure dla środowiska Eclipse)
  * [Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Instrukcje logowania dotyczące zestawu narzędzi platformy Azure dla środowiska Eclipse)
* [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) (Zestaw narzędzi platformy Azure dla środowiska IntelliJ)
  * [What's New in the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-whats-new.md) (Co nowego w zestawie narzędzi platformy Azure dla środowiska IntelliJ)
  * [Installing the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md) (Instalowanie zestawu narzędzi platformy Azure dla środowiska IntelliJ)
  * [Sign In Instructions for the Azure Toolkit for IntelliJ](https://go.microsoft.com/fwlink/?linkid=846179) (Instrukcje logowania dotyczące zestawu narzędzi platformy Azure dla środowiska IntelliJ)

Aby uzyskać więcej informacji o używaniu platformy Azure z językiem Java, zobacz [Centrum deweloperów języka Java dla platformy Azure](https://azure.microsoft.com/develop/java/) i [Java Tools for Visual Studio Team Services](https://java.visualstudio.com/) (Narzędzia języka Java dla usługi Visual Studio Team Services).

