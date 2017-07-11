---
title: "Tworzenie pierwszej aplikacji internetowej w środowisku Java na platformie Azure"
description: "Dowiedz się, jak można uruchamiać aplikacje internetowe w usłudze App Service, wdrażając podstawową aplikację w środowisku Java."
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
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: a805d92fbe1043b9143140bdbfb8626362aa8bb5
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017

---
<a id="create-your-first-java-web-app-in-azure" class="xliff"></a>

# Tworzenie pierwszej aplikacji internetowej w środowisku Java na platformie Azure

Funkcja [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) usługi [Azure App Service](../app-service/app-service-value-prop-what-is.md) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie. W tym przewodniku Szybki start pokazano, jak wdrożyć aplikację internetową w języku Java przy użyciu środowiska [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).

![„Hello Azure!” — przykładowa aplikacja internetowa](./media/app-service-web-get-started-java/browse-web-app-1.png)

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start, zainstaluj:

* Bezpłatne środowisko [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/). W tym przewodniku Szybki start używane jest środowisko Eclipse Neon.
* [Zestaw narzędzi platformy Azure dla środowiska Eclipse](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-dynamic-web-project-in-eclipse" class="xliff"></a>

## Tworzenie dynamicznego projektu internetowego w środowisku Eclipse

W środowisku Eclipse wybierz pozycję **File** > **New** > **Dynamic Web Project** (Plik > Nowy > Dynamiczny projekt internetowy).

W oknie dialogowym **New Dynamic Web Project** (Nowy dynamiczny projekt internetowy) wpisz nazwę projektu **MyFirstJavaOnAzureWebApp**, a następnie wybierz pozycję **Finish** (Zakończ).
   
![Okno dialogowe New Dynamic Web Project (Nowy dynamiczny projekt internetowy)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

<a id="add-a-jsp-page" class="xliff"></a>

### Dodawanie strony JSP

Jeśli obszar Project Explorer (Eksplorator projektów) nie jest wyświetlany, przywróć go.

![Obszar roboczy Java EE dla środowiska Eclipse](./media/app-service-web-get-started-java/pe.png)

W obszarze Project Explorer (Eksplorator projektów) rozwiń projekt **MyFirstJavaOnAzureWebApp**.
Kliknij prawym przyciskiem myszy folder **WebContent**, a następnie kliknij pozycję **New** > **JSP File** (Nowy > Plik JSP).

![Menu dla nowego pliku JSP w obszarze Project Explorer (Eksplorator projektów)](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

W oknie dialogowym **New JSP File** (Nowy plik JSP):

* Jako nazwę pliku podaj wartość **index.jsp**.
* Wybierz pozycję **Finish** (Zakończ).

  ![Okno dialogowe New JSP File (Nowy plik JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

W pliku index.jsp zastąp element `<body></body>` następującym kodem:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Zapisz zmiany.

<a id="publish-the-web-app-to-azure" class="xliff"></a>

## Publikowanie aplikacji internetowej na platformie Azure

W obszarze Project Explorer (Eksplorator projektów) kliknij projekt prawym przyciskiem myszy, a następnie wybierz pozycję **Azure** > **Publish as Azure Web App** (Publikuj jako aplikację internetową platformy Azure).

![Menu kontekstowe Publish as Azure Web App (Publikuj jako aplikację internetową platformy Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

W oknie dialogowym **Azure Sign In** (Logowanie do platformy Azure) zachowaj opcję **Interactive** (Interaktywne), a następnie wybierz pozycję **Sign in** (Zaloguj).

Postępuj zgodnie z instrukcjami dotyczącymi logowania.

<a id="deploy-web-app-dialog-box" class="xliff"></a>

### Okno dialogowe Deploy Web App (Wdrażanie aplikacji internetowej)

Po zalogowaniu się na koncie platformy Azure zostanie wyświetlone okno dialogowe **Deploy Web App** (Wdrażanie aplikacji internetowej).

Wybierz pozycję **Utwórz**.

![Okno dialogowe Deploy Web App (Wdrażanie aplikacji internetowej)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

<a id="create-app-service-dialog-box" class="xliff"></a>

### Okno dialogowe Create App Service (Tworzenie usługi App Service)

Zostanie wyświetlone okno dialogowe **Create App Service** (Tworzenie usługi App Service) z wartościami domyślnymi. Liczba **170602185241** wyświetlana na poniższym obrazie będzie inna w Twoim oknie dialogowym.

![Okno dialogowe Create App Service (Tworzenie usługi App Service)](./media/app-service-web-get-started-java/cas1.png)

W oknie dialogowym **Create App Service** (Tworzenie usługi App Service):

* Zachowaj wygenerowaną nazwę aplikacji internetowej. Ta nazwa musi być unikatowa w obrębie całej platformy Azure. Nazwa jest częścią adresu URL aplikacji internetowej. Przykład: jeśli nazwa aplikacji internetowej to **MyJavaWebApp**, adres URL to *myjavawebapp.azurewebsites.net*.
* Zachowaj domyślny kontener internetowy.
* Wybierz subskrypcję platformy Azure.
* Na karcie **App service plan** (Plan usługi App Service):

  * **Create new** (Utwórz nowy): zachowaj wartość domyślną, czyli nazwę planu usługi App Service.
  * **Location** (Lokalizacja): wybierz pozycję **West Europe** (Europa Zachodnia) lub lokalizację w Twoim pobliżu.
  * **Pricing tier** (Warstwa cenowa): wybierz opcję Free (Bezpłatna). Aby uzyskać informacje o funkcjach, zobacz [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/) (Cennik usługi App Service).

   ![Okno dialogowe Create App Service (Tworzenie usługi App Service)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<a id="resource-group-tab" class="xliff"></a>

### Karta Resource group (Grupa zasobów)

Wybierz kartę **Resource group** (Grupa zasobów). Zachowaj domyślnie wygenerowaną wartość dla grupy zasobów.

![Karta Resource group (Grupa zasobów)](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Wybierz pozycję **Utwórz**.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Za pomocą zestawu narzędzi platformy Azure zostanie utworzona aplikacja internetowa i wyświetlone okno dialogowe postępu.

![Okno dialogowe Create App Service Progress (Postęp tworzenia usługi App Service)](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

<a id="deploy-web-app-dialog-box" class="xliff"></a>

### Okno dialogowe Deploy Web App (Wdrażanie aplikacji internetowej)

W oknie dialogowym **Deploy Web App** (Wdrażanie aplikacji internetowej) wybierz pozycję **Deploy to root** (Wdróż w katalogu głównym). Jeśli istnieje usługa App Service w lokalizacji *wingtiptoys.azurewebsites.net* i nie wybierzesz wdrożenia w katalogu głównym, to aplikacja internetowa o nazwie **MyFirstJavaOnAzureWebApp** zostanie wdrożona w lokalizacji *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Okno dialogowe Deploy Web App (Wdrażanie aplikacji internetowej)](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Okno dialogowe zawiera wybory dokonane dla platformy Azure, zestawu JDK i kontenera internetowego.

Wybierz pozycję **Deploy** (Wdróż), aby opublikować aplikację internetową na platformie Azure.

Po zakończeniu publikowania wybierz link **Published** (Opublikowano) w oknie dialogowym **Azure Activity Log** (Dziennik aktywności platformy Azure).

![Okno dialogowe Azure Activity Log (Dziennik aktywności platformy Azure)](./media/app-service-web-get-started-java/aal.png)

Gratulacje! Aplikacja internetowa została pomyślnie wdrożona na platformie Azure. 

![„Hello Azure!” — przykładowa aplikacja internetowa](./media/app-service-web-get-started-java/browse-web-app-1.png)

<a id="update-the-web-app" class="xliff"></a>

## Aktualizowanie aplikacji internetowej

Zmień przykładowy kod JSP na inny komunikat.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Zapisz zmiany.

W obszarze Project Explorer (Eksplorator projektów) kliknij projekt prawym przyciskiem myszy, a następnie wybierz pozycję **Azure** > **Publish as Azure Web App** (Publikuj jako aplikację internetową platformy Azure).

Zostanie wyświetlone okno dialogowe **Deploy Web App** (Wdrażanie aplikacji internetowej), w którym będzie wyświetlona wcześniej utworzona usługa App Service. 

> [!NOTE]
> Wybieraj pozycję **Deploy to root** (Wdróż w katalogu głównym) za każdym razem, gdy ma miejsce publikowanie.
>

Wybierz aplikację internetową, a następnie wybierz pozycję **Deploy** (Wdróż), co spowoduje opublikowanie zmian.

Gdy zostanie wyświetlony link **Publishing** (Publikowanie), wybierz go, aby przejść do aplikacji internetowej i wyświetlić zmiany.

<a id="manage-the-web-app" class="xliff"></a>

## Zarządzanie aplikacją internetową

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby wyświetlić utworzoną aplikację internetową.

W menu po lewej stronie kliknij pozycję **Grupy zasobów**.

![Nawigacja w portalu do grupy zasobów](media/app-service-web-get-started-java/rg.png)

Wybierz grupę zasobów. Na stronie znajdują się zasoby utworzone w ramach tego przewodnika Szybki Start.

![Grupa zasobów myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Wybierz aplikację internetową (**webapp-170602193915** na powyższym obrazie).

Zostanie wyświetlona strona **Przegląd**. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. Na kartach po lewej stronie strony są pokazane poszczególne konfiguracje, które można otworzyć. 

![Strona usługi App Service w witrynie Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

<a id="next-steps" class="xliff"></a>

## Następne kroki

> [!div class="nextstepaction"]
> [Mapowanie domeny niestandardowej](app-service-web-tutorial-custom-domain.md)

