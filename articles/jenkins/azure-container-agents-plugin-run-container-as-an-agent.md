---
title: "Tworzenie projektu na platformie Azure przy użyciu Wpięć i wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak zbudować projekt na platformie Azure z wystąpień kontenera Azure za pomocą wtyczki agenta kontenera platformy Azure dla Wpięć"
services: multiple
documentationcenter: 
author: tomarcher
manager: rloutlaw
editor: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 269e936cb79ba4138285f5dbd326413d70d5924d
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="build-a-project-in-azure-using-jenkins-and-azure-container-instances"></a>Tworzenie projektu na platformie Azure przy użyciu Wpięć i wystąpień kontenera platformy Azure

Wystąpień kontenera Azure umożliwia łatwe do uruchomienia i działa bez konieczności umieszczanie maszyn wirtualnych lub wdrożyć usługę wyższego poziomu usługi. Wystąpień kontenera Azure udostępnia rozliczeń na sekundę na podstawie wydajności, które są potrzebne; dzięki czemu atrakcyjną opcję dla przejściowej obciążeń, takich jak przeprowadzanie kompilacji.

Omawiane kwestie:
> [!div class="checklist"]
> * Instalowanie i konfigurowanie serwera Wpięć na platformie Azure
> * Instalowanie i konfigurowanie agentów kontenera Azure wtyczki dla Wpięć
> * Umożliwia tworzenie wystąpień kontenera Azure [Spring PetClinic przykładowej aplikacji](https://github.com/spring-projects/spring-petclinic)

## <a name="prerequisites"></a>Wymagania wstępne

- **Subskrypcja platformy Azure** — aby zapoznaj się z opcjami zakupu platformy Azure, zobacz [jak kupić usługę Azure](https://azure.microsoft.com/pricing/purchase-options/) lub [bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI w wersji 2.0 lub powłoki chmury Azure** -zainstalować jednego z następujących produktów, w którym można wpisać poleceń Azure:

    - [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) — umożliwia uruchamianie poleceń Azure z polecenia lub okno terminalu.
    - [Powłoka chmury Azure](/azure/cloud-shell/quickstart.md) — oparte na przeglądarce powłoki środowisko. Chmury powłoki zapewnia dostęp do wiersza polecenia w przeglądarce skompilowanej za pomocą zadań zarządzania platformy Azure na uwadze.

## <a name="install-a-jenkins-server-on-azure-using-the-jenkins-marketplace-image"></a>Instalowanie serwera Wpięć na platformie Azure przy użyciu obrazu Wpięć Marketplace

Wpięć obsługuje model, w którym Wpięć delegatów serwera działa na co najmniej jeden Agent umożliwia jednej instalacji Wpięć do obsługi wielu projektów lub w celu zapewnienia różnych środowiskach potrzebne do kompilacji lub testów. Kroki opisane w tej sekcji przeprowadzają użytkownika przez proces instalowania i konfigurowania serwera Wpięć na platformie Azure.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-the-jenkins-server-running-on-azure"></a>Połączyć się z serwerem Wpięć działających na platformie Azure

Po zainstalowaniu Wpięć na platformie Azure, należy nawiązać połączenia z Wpięć. W poniższych krokach objaśniono przy konfigurowaniu połączenia SSH z maszyną wirtualną z Wpięć działających na platformie Azure. 

[!INCLUDE [jenkins-connect-to-jenkins-server-running-on-azure](../../includes/jenkins-connect-to-jenkins-server-running-on-azure.md)]

## <a name="update-jenkins-dns"></a>Aktualizacja Wpięć DNS

Wpięć musi znać własnego adresu URL, podczas tworzenia łącza do samej siebie. Na przykład adres URL musi zostać użyte podczas Wpięć wysyła wiadomości e-mail zawierających linki bezpośrednie do kompilacji wyników. 

Ta sekcja przeprowadzi Cię przez ustawienie Wpięć adres URL.

1. W przeglądarce przejdź do pulpitu nawigacyjnego Wpięć na `http://localhost:8080`.

1. Wybierz **Zarządzanie Wpięć**.

    ![Zarządzanie opcjami Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wybierz **skonfigurować System**.

    ![Zarządzanie Wpięć opcji dodatków na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. W obszarze **Wpięć lokalizacji**, wprowadź adres URL serwera Wpięć.

1. Wybierz pozycję **Zapisz**.

## <a name="update-jenkins-to-allow-java-network-launch-protocol-jnlp"></a>Wpięć aktualizacji, aby umożliwić Java sieci uruchamianie protokołu (JNLP)

Agent Wpięć połączy się z serwerem Wpięć za pomocą języka Java sieci uruchamianie protokołu (JNLP). W tej sekcji opisano sposób określić port dla agentów JNLP do użycia przy komunikacji z serwerem Wpięć.

1. Na pulpicie nawigacyjnym Wpięć wybierz **Zarządzanie Wpięć**.

    ![Zarządzanie opcjami Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wybierz **Konfigurowanie zabezpieczeń globalnych**.

    ![Konfigurowanie zabezpieczeń globalnych na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-global-security.png)

1. W obszarze **agentów**, wybierz pozycję **stałe**, a następnie wprowadź port. Zrzut ekranu pokazujący wartość port 12345, na przykład. Należy określić port, który ma sens dla danego środowiska.

    ![Zaktualizuj ustawienia zabezpieczeń globalnych Wpięć umożliwia JNLP](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-set-jnlp.png)

1. Wybierz pozycję **Zapisz**.

1. Za pomocą powłoki chmury albo 2.0 interfejsu wiersza polecenia Azure, wprowadź następujące polecenie, aby utworzyć regułę ruchu przychodzącego dla swojej grupy zabezpieczeń sieci Wpięć:

    ```azurecli
    az network nsg rule create  \
    --resource-group JenkinsResourceGroup \
    --nsg-name JenkinsNSG  \
    --name allow-https \
    --description "Allow access to port 12345 for HTTPS" \
    --access Allow \
    --protocol Tcp  \
    --direction Inbound \
    --priority 102 \
    --source-address-prefix "*"  \
    --source-port-range "*"  \
    --destination-address-prefix "*" \
    --destination-port-range "12345"
    ```

## <a name="create-and-add-an-azure-service-principal-to-the-jenkins-credentials"></a>Utwórz i Dodaj nazwę główną usługi Azure poświadczenia Wpięć

Należy nazwy głównej usługi Azure, aby wdrożyć na platformie Azure. Poniższe kroki prowadzące przez proces tworzenia nazwy głównej usługi (Jeśli nie masz jeszcze jeden) i aktualizowania Wpięć z Twojej nazwy głównej usługi.

1. Jeśli już masz nazwy głównej usługi (i dowiedzieć się, identyfikator subskrypcji, dzierżawy, appId i hasło), możesz pominąć ten krok. Jeśli musisz utworzyć podmiot zabezpieczeń, zapoznaj się z artykułem [zapisać nazwy głównej usługi platformy Azure z Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Podczas tworzenia podmiot zabezpieczeń, zanotuj wartości Identyfikatora subskrypcji, dzierżawy, appId i hasło.

1. Wybierz **poświadczenia**.

    ![Zarządzanie opcji użycia poświadczeń na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials.png)

1. W obszarze **poświadczenia**, wybierz pozycję **systemu**.

    ![Zarządzanie opcji użycia poświadczeń systemu w pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-system.png)

1. Wybierz **poświadczeń globalnych (bez ograniczeń)**.

    ![Zarządzanie globalnym systemie opcji użycia poświadczeń na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-global.png)

1. Wybierz **dodawania poświadczeń niektórych**.

    ![Dodawanie poświadczeń na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-adding-credentials.png)

1. W **rodzaj** listy rozwijanej wybierz **główną usługi Microsoft Azure** spowodować strona do wyświetlania pól określonych Dodawanie nazwy głównej usługi. Następnie wprowadź wymagane wartości w następujący sposób:

    - **Zakres** — wybierz opcję dla **Global (Wpięć, węzłów, elementy, wszystkie elementy podrzędne, itp.)** .
    - **Identyfikator subskrypcji** — identyfikator subskrypcji platformy Azure, określony podczas uruchamiania `az account set`.
    - **Identyfikator klienta** -użyj `appId` wartość zwracana z `az ad sp create-for-rbac`.
    - **Klucz tajny klienta** -użyj `password` wartość zwracana z `az ad sp create-for-rbac`.
    - **Identyfikator dzierżawy** -użyj `tenant` wartość zwracana z `az ad sp create-for-rbac`.
    - **Środowisko Azure** — wybierz tę opcję `Azure`.
    - **Identyfikator** — wprowadź `myTestSp`. Ta wartość jest używana później w tym samouczku.
    - **Opis elementu** — Opcjonalnie wprowadź opis wartość dla tego podmiotu zabezpieczeń.

    ![Określ nowe właściwości główną usługi na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-principal-properties.png)

1. Po wprowadzeniu informacji Definiowanie podmiot zabezpieczeń, można opcjonalnie wybierz **Sprawdź nazwy głównej usługi** aby upewnić się, że wszystko działa poprawnie. Jeśli nazwy głównej usługi jest poprawnie zdefiniowany, zostanie wyświetlony komunikat z informacją, "pomyślnie zweryfikować nazwy głównej usługi Microsoft Azure." poniżej **opis** pola.

1. Gdy skończysz, wybierz **OK** do dodania do Wpięć podmiot zabezpieczeń. Pulpit nawigacyjny Wpięć Wyświetla nowo dodanego podmiotu zabezpieczeń na **poświadczeń globalnych** strony.

## <a name="create-an-azure-resource-group-for-your-azure-container-instances"></a>Tworzenie grupy zasobów platformy Azure dla swoich wystąpień kontenera platformy Azure

Azure wystąpień kontenera muszą znajdować się w grupie zasobów platformy Azure. Grupy zasobów platformy Azure jest kontenerem, który zawiera powiązane zasoby Azure rozwiązania.

Przy użyciu Azure CLI 2.0 lub powłoki chmury, wprowadź następujące polecenie, aby utworzyć grupę zasobów o nazwie `JenkinsAciResourceGroup` w lokalizacji `eastus`:

```azurecli
az group create --name JenkinsAciResourceGroup --location eastus
```

Po zakończeniu `az group create` polecenie wyświetla wyniki podobne do poniższego przykładu:

```JSON
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/JenkinsAciResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "JenkinsAciResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="install-the-azure-container-agents-plugin-for-jenkins"></a>Zainstaluj wtyczkę Azure kontenera agentów dla Wpięć

Jeśli po zainstalowaniu dodatku agentów kontenera platformy Azure, możesz pominąć tę sekcję.

Po utworzeniu grupy zasobów platformy Azure utworzonych dla agenta Wpięć pokazano, jak instalowanie wtyczki agentów kontenera Azure następujące czynności:

1. Na pulpicie nawigacyjnym Wpięć wybierz **Zarządzanie Wpięć**.

    ![Zarządzanie opcjami Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wybierz **Zarządzanie wtyczkami**.

    ![Zarządzanie Wpięć opcji dodatków na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-plugins.png)

1. Wybierz **dostępne**.

    ![Wyświetl dostępną opcją wtyczek Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-view-available-plugins.png)

1. Wprowadź `Azure Container Agents` do **filtru** pola tekstowego. (Listy filtrów wprowadzania tekstu.)

    ![Filtruj dostępne dodatki Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-filter-available-plugins.png)

1. Zaznacz pole wyboru obok pozycji **agentów kontenera Azure** wtyczki i jedną z opcji instalacji. Do celów tego pokazu, I wybrana **zainstalować bez ponownego uruchomienia** opcji.

    ![Instalowanie wtyczki agentów kontenera platformy Azure z Wpięć pulpitu nawigacyjnego](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin.png)

1.  Po wybraniu opcji do zainstalowania żądanego wtyczki pulpitu nawigacyjnego Wpięć wyświetla stronę opisujące stan w przypadku instalowania.

    ![Instalowanie wtyczki agentów kontenera platformy Azure z poziomu pulpitu nawigacyjnego Wpięć stanu instalacji](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin-confirmation.png)

    Aby przywrócić strony głównej pulpitu nawigacyjnego Wpięć, wybierz **wróć do strony górnej**.

## <a name="configure-the-azure-container-agents-plugin"></a>Skonfiguruj wtyczkę agentów kontenera platformy Azure

Po zainstalowaniu dodatku agentów kontenera platformy Azure, ta sekcja przeprowadzi Cię przez konfigurowanie wtyczki w pulpicie nawigacyjnym Wpięć.

1. Na pulpicie nawigacyjnym Wpięć wybierz **Zarządzanie Wpięć**.

    ![Zarządzanie opcjami Wpięć na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Wybierz **skonfigurować System**.

    ![Zarządzanie Wpięć opcji dodatków na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Zlokalizuj **chmury** sekcji u dołu strony i z **Dodaj nowe chmury** listy rozwijanej wybierz **wystąpienia kontenera Azure**.

    ![Dodaj nowego dostawcę chmury z Wpięć pulpitu nawigacyjnego](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-cloud-provider.png)

1. W **wystąpienia kontenera Azure** sekcji, określ następujące wartości:

    - **Nazwa chmury** — opcjonalnie zgodnie z wartością domyślną jest nazwa wygenerowana automatycznie. Określ nazwę dla tego wystąpienia. 
    - **Poświadczenia Azure** — wybierz strzałkę listy rozwijanej, a następnie wybierz `myTestSp` wpis, który identyfikuje główną usługi Azure utworzony wcześniej.
    - **Grupa zasobów** — wybierz strzałkę listy rozwijanej, a następnie wybierz `JenkinsAciResourceGroup` wpisu, który identyfikuje grupę zasobów Azure kontenera wystąpienia utworzone wcześniej.

    ![Definiowanie właściwości wystąpienia kontenera platformy Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-properties.png)

1. Wybierz **dodać szablon kontenera** strzałkę listy rozwijanej, a następnie wybierz **Aci kontenera szablonu**.

1. W **Aci kontenera szablonu** sekcji, określ następujące wartości:

    - **Nazwa** — wprowadź `ACI-container`.
    - **Etykiety** — wprowadź `ACI-container`.
    - **Obraz docker** — wprowadź`cloudbees/jnlp-slave-with-java-build-tools`

    ![Definiowanie właściwości obrazu wystąpienie kontenera platformy Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-image-properties.png)

1. Wybierz **zaawansowane**.

1. Wybierz **strategii przechowywania** strzałkę listy rozwijanej i wybierz **strategii przechowywania bezczynności kontenera**. Po zaznaczeniu tej opcji, Wpięć przechowuje agenta, dopóki nie nowe zadanie jest wykonywane na agencie i upływie określonego czasu bezczynności.

    ![Definiowanie strategii przechowywania wystąpień kontenera platformy Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-retention-strategy.png)

1. Wybierz pozycję **Zapisz**.

## <a name="create-the-spring-petclinic-application-job-in-jenkins"></a>Utwórz zadanie Spring PetClinic aplikacji w Wpięć

Poniższe kroki przedstawiono tworzenie zadania Wpięć — jako projekt stylu — do skompilowania aplikacji PetClinic sprężyny.

1. Na pulpicie nawigacyjnym Wpięć wybierz **nowy element**.

    ![Opcję menu Nowy element na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-item.png)

1. Wprowadź `myPetClinicProject` dla nazwy elementu, a następnie wybierz **stylu projektu**.

    ![Nowy projekt stylu na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project.png)

1. Kliknij przycisk **OK**.

1. Wybierz **ogólne** , a następnie określ następujące wartości:

    - **Ograniczenia, gdzie można uruchomić projektu** — wybierz tę opcję.
    - **Etykieta wyrażenie** — wprowadź `ACI-container`. Po wyjściu z pola, zostanie wyświetlony komunikat z potwierdzeniem, że etykiety jest obsługiwana przez konfigurację chmury utworzony w poprzednim kroku.

    ![Ogólne ustawienia dla nowego projektu stylu na pulpicie nawigacyjnym Wpięć](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project-general.png)

1. Wybierz **zarządzania kodem źródłowym** , a następnie określ następujące wartości:

    - **Źródło zarządzania kodem** — wybierz tę opcję **Git**.
    - **Adres URL repozytorium** — wprowadź następujący adres URL repozytorium GitHub aplikacji przykładowej PetClinic Spring: `https://github.com/spring-projects/spring-petclinic.git`.

1. Wybierz **kompilacji** , a następnie wykonaj następujące zadania:

    - Wybierz **kroku kompilacji Dodaj** strzałkę listy rozwijanej i wybierz **wywołania najwyższego poziomu celów Maven**.

    - Aby uzyskać **cele**, wprowadź `package`.

1. Wybierz **zapisać** Aby zapisać nową definicję projektu.

## <a name="build-the-spring-petclinic-application-job-in-jenkins"></a>Tworzenie zadania Spring PetClinic aplikacji w Wpięć

Nadszedł czas, aby skompilować projekt! W tej sekcji wyjaśniono, jak zbudować projekt z Wpięć pulpitu nawigacyjnego.

1. Na pulpicie nawigacyjnym Wpięć wybierz `myPetClinicProject`.

    ![Wybierz projekt do kompilacji z Wpięć pulpitu nawigacyjnego.](./media/azure-container-agents-plugin-run-container-as-an-agent/select-project-to-build.png)

1. Wybierz **kompilacji teraz**. 

    ![Skompiluj projekt z Wpięć pulpitu nawigacyjnego.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-project.png)

1. Po uruchomieniu kompilacji w Wpięć przebywa w kolejce kompilacji. W przypadku agenta kontenera Azure kompilacji nie można uruchomić, dopóki Agent kontenera Azure zostanie uruchomiona i przełączony w tryb online. Do tego czasu zostanie wyświetlony komunikat wskazujący nazwa agenta i fakt, że kompilacji jest oczekująca. (Ten proces trwa około pięciu minut, ale jest wymagane tylko po raz pierwszy użyj agenta dla kompilacji. Kolejne kompilacje są znacznie szybsze, ponieważ agent jest w trybie online w tym momencie).

    ![Kompilacja jest w kolejce, dopóki agent nie zostanie utworzona i przełączony w tryb online.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-pending.png)

    Po uruchomieniu kompilacji, pasek postępu bieguna barber wskazuje, czy kompilacja jest uruchomiona:

    ![Kompilacja jest uruchomiona, gdy zostanie wyświetlony pasek postępu bieguna barber.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-running.png)

1. Gdy zniknie pasek postępu bieguna barber, wybierz strzałkę obok numer kompilacji. Wybierz z menu kontekstowego **dane wyjściowe konsoli**.

    ![Kliknij element menu konsoli dziennika, aby wyświetlić informacje o kompilacji.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-menu.png)

1. Informacje dziennika konsoli emitowanych przez proces kompilacji. Aby wyświetlić wszystkie informacje o kompilacji (w tym informacje o kompilacji wykonywane zdalnie w agencie Azure utworzony), wybierz **zapełnienie dziennika**.

    ![Kliknij łącze pełny dziennik, aby wyświetlić bardziej szczegółowe informacje o kompilacji.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log.png)

    Pełny dziennik wyświetla pełniejsze informacje o kompilacji:

    ![Pełny dziennik wyświetla pełniejsze informacje o kompilacji.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-full.png)
    
1. Aby wyświetlić dyspozycji kompilacji, przewiń w dół dziennika.

    ![Wyświetla dyspozycji kompilacji w dolnej części dziennika kompilacji.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-disposition.png)

## <a name="clean-up-azure-resources"></a>Oczyszczanie zasobów platformy Azure

W tym samouczku utworzony zasoby zawarte w dwóch grupach zasobów platformy Azure: 
    - `JenkinsResourceGroup`-Zawiera zasoby platformy Azure dla serwera Wpięć.
    - `JenkinsAciResourceGroup`-Zawiera zasoby platformy Azure dla agenta Wpięć.
    
Jeśli nie trzeba używać dowolnych zasobów w grupie zasobów platformy Azure, można usunąć grupy zasobów przy użyciu `az group delete` polecenia w następujący sposób (zastępowanie &lt;grupa zasobów > Symbol zastępczy nazwą grupy zasobów, aby Usuń):

```azurecli
az group delete -n <resourceGroup>
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Odwiedź Wpięć w Centrum Azure, aby zobaczyć najnowsze artykuły i przykłady](https://docs.microsoft.com/en-us/azure/jenkins/)