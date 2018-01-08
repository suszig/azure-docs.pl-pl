---
title: "Połącz z analizą dzienników programu Configuration Manager | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono kroki, aby nawiązać połączenia analizy dzienników programu Configuration Manager i rozpocząć analizy danych."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: 7acf0cbd4f4cba885e6cc91dfe3cb68306a3649a
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Połącz z analizą dzienników programu Configuration Manager
System Center Configuration Manager można połączyć z analizą dzienników w OMS, na synchronizowanie danych kolekcji urządzeń. Dzięki temu dane z hierarchii programu Configuration Manager dostępne w OMS.

## <a name="prerequisites"></a>Wymagania wstępne

Analiza dzienników obsługuje System Center Configuration Manager bieżącej gałęzi, wersja 1606 i wyższych.  

## <a name="configuration-overview"></a>Przegląd konfiguracji
Poniższe instrukcje stanowią podsumowanie procesu do nawiązania połączenia analizy dzienników programu Configuration Manager.  

1. W portalu Azure Zarejestruj programu Configuration Manager jako aplikację aplikacji sieci Web i/lub interfejs API sieci Web i upewnij się, że użytkownik ma identyfikator klienta i klucz tajny klienta z rejestracji z usługi Azure Active Directory. Zobacz [użycia portalu do tworzenia aplikacji i usług podmiot zabezpieczeń, który ma dostęp do zasobów usługi Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) Aby uzyskać szczegółowe informacje na temat wykonywania tego kroku.
2. W portalu Azure [Określ programu Configuration Manager (aplikacja sieci web w zarejestrowany) z uprawnieniami do OMS](#provide-configuration-manager-with-permissions-to-oms).
3. W programie Configuration Manager [dodać połączenie za pomocą Kreatora dodawania pakietu OMS połączenia](#add-an-oms-connection-to-configuration-manager).
4. W programie Configuration Manager [zaktualizować właściwości połączenia](#update-oms-connection-properties) Jeśli kiedykolwiek klucza tajnego klienta lub hasło wygaśnie lub zostaną utracone.
5. Informacje z portalu OMS [pobrać i zainstalować program Microsoft Monitoring Agent](#download-and-install-the-agent) na komputerze z systemem roli systemu lokacji punktu połączenia programu Configuration Manager service. Agent wysyła dane programu Configuration Manager z usługą OMS.
6. W przypadku analizy dzienników [zaimportować kolekcje z programu Configuration Manager](#import-collections) jako grupy komputerów.
7. W przypadku analizy dzienników wyświetlania danych z programu Configuration Manager jako [grup komputerów](log-analytics-computer-groups.md).

Więcej o łączeniu programu Configuration Manager z usługą OMS na [synchronizowanie danych z programu Configuration Manager do programu Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Określ programu Configuration Manager z uprawnieniami do OMS
Poniższa procedura zawiera portalu Azure z uprawnieniami dostępu OMS. W szczególności należy przyznać *roli współautora* użytkownikom w grupie zasobów, aby umożliwić portalu Azure do łączenia programu Configuration Manager z usługą OMS.

> [!NOTE]
> Należy określić uprawnienia w OMS programu Configuration Manager. W przeciwnym razie zostanie wyświetlony komunikat o błędzie, korzystając z Kreatora konfiguracji w programie Configuration Manager.
>
>

1. Otwórz [portalu Azure](https://portal.azure.com/) i kliknij przycisk **Przeglądaj** > **analizy dzienników (OMS)** otworzyć analizy dzienników (OMS).  
2. Na **analizy dzienników (OMS)**, kliknij przycisk **Dodaj** otworzyć **obszarem roboczym pakietu OMS**.  
   ![OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. Na **obszarem roboczym pakietu OMS**, podaj następujące informacje, a następnie kliknij przycisk **OK**.

   * **Obszar roboczy OMS**
   * **Subskrypcja**
   * **Grupa zasobów**
   * **Lokalizacja**
   * **Warstwa cenowa**  
     ![OMS](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > W powyższym przykładzie tworzy nową grupę zasobów. Grupa zasobów jest używane wyłącznie do zapewnienia programu Configuration Manager z uprawnieniami do obszaru roboczego OMS, w tym przykładzie.
     >
     >
4. Kliknij przycisk **Przeglądaj** > **grup zasobów** otworzyć **grup zasobów**.
5. W **grup zasobów**, kliknij grupę zasobów, której utworzone powyżej, aby otworzyć &lt;Nazwa grupy zasobów&gt; ustawienia.  
   ![Ustawienia grupy zasobów](./media/log-analytics-sccm/sccm-azure03.png)
6. W &lt;Nazwa grupy zasobów&gt; ustawienia, kliknij przycisk kontroli dostępu (IAM), aby otworzyć &lt;Nazwa grupy zasobów&gt; użytkowników.  
   ![zasób grupy użytkowników](./media/log-analytics-sccm/sccm-azure04.png)  
7. W &lt;Nazwa grupy zasobów&gt; , kliknij przycisk **Dodaj** otworzyć **Dodawanie dostępu**.
8. W **Dodawanie dostępu**, kliknij przycisk **wybierz rolę**, a następnie wybierz **współautora** roli.  
   ![Wybierz rolę](./media/log-analytics-sccm/sccm-azure05.png)  
9. Kliknij przycisk **dodawania użytkowników**, wybierz użytkownika programu Configuration Manager, kliknij przycisk **wybierz**, a następnie kliknij przycisk **OK**.  
   ![Dodawanie użytkowników](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Dodaj połączenie OMS do programu Configuration Manager
Aby dodać połączenie OMS, środowiska programu Configuration Manager, należy skonfigurować [punkt połączenia z usługą](https://technet.microsoft.com/library/mt627781.aspx) skonfigurowany dla trybu online.

1. W **administracji** obszaru roboczego programu Configuration Manager, wybierz **łącznik OMS**. Spowoduje to otwarcie **Kreatora dodawania pakietu OMS połączenia**. Wybierz **dalej**.
2. Na **ogólne** ekranu, upewnij się, że wykonano następujące czynności i czy masz szczegółów dla każdego elementu, a następnie wybierz **dalej**.

   1. W portalu Azure jako aplikację aplikacji sieci Web i/lub interfejs API sieci Web został zarejestrowany programu Configuration Manager i czy masz [identyfikator klienta z rejestracji](../active-directory/active-directory-integrating-applications.md).
   2. W portalu Azure zostanie utworzona aplikacja klucz tajny aplikacji zarejestrowanych w usłudze Azure Active Directory.  
   3. W portalu Azure podane aplikacji sieci web w zarejestrowany z uprawnieniami do OMS.  
      ![Połączenie do strony głównej kreatora OMS](./media/log-analytics-sccm/sccm-console-general01.png)
3. Na **usługi Azure Active Directory** ekranu, skonfigurować ustawienia połączenia z usługą OMS zapewniając Twojej **dzierżawy**, **identyfikator klienta**, i **klucz tajny klienta** , a następnie wybierz pozycję **dalej**.  
   ![Połączenie do strony OMS Kreatora usługi Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Jeśli wykonano wszystkie inne procedury pomyślnie, następnie informacje w **konfiguracji połączenia OMS** ekranu będzie automatycznie wyświetlane na tej stronie. Informacje o ustawieniach połączenia ma być wyświetlany dla Twojego **subskrypcji platformy Azure**, **grupy zasobów platformy Azure**, i **obszar roboczy usługi Operations Management Suite**.  
   ![Połączenie do strony OMS kreatora OMS połączenia](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Kreator łączy się z usługą OMS, korzystając z informacji podanych przez Ciebie danych wejściowych. Wybierz kolekcje urządzeń, które mają być synchronizowane z usługą OMS, a następnie kliknij przycisk **Dodaj**.  
   ![Wybierz kolekcje](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Sprawdź ustawienia połączenia w **Podsumowanie** ekranu, a następnie wybierz **dalej**. **Postępu** ekranu przedstawia stan połączenia, a następnie należy **Complete**.

> [!NOTE]
> Należy połączyć OMS do lokacji najwyższego poziomu w hierarchii. Jeśli możesz nawiązać OMS autonomicznej lokacji głównej, a następnie dodać centralną lokację administracyjną do środowiska, należy Usuń i Utwórz ponownie połączenie z usługą OMS w nowej hierarchii.
>
>

Po połączeniu programu Configuration Manager z usługą OMS Dodaj lub usuń kolekcje, a wyświetlić właściwości połączenia OMS.

## <a name="update-oms-connection-properties"></a>Zaktualizuj właściwości połączenia OMS
Jeśli kiedykolwiek klucza tajnego klienta lub hasło wygaśnie lub zostaną utracone, należy ręcznie zaktualizować właściwości połączenia OMS.

1. W programie Configuration Manager przejdź do **usługi w chmurze**, a następnie wybierz pozycję **łącznik OMS** otworzyć **właściwości połączenia OMS** strony.
2. Na tej stronie, kliknij przycisk **usługi Azure Active Directory** kartę, aby wyświetlić Twojej **dzierżawy**, **identyfikator klienta**, **wygaśnięcia klucza tajnego klienta**. **Sprawdź** Twojego **klucza tajnego klienta** Jeśli wygasł.

## <a name="download-and-install-the-agent"></a>Pobierz i zainstaluj agenta
1. W portalu OMS [Pobierz plik Instalatora agenta z usługą OMS](log-analytics-windows-agent.md).
2. Aby zainstalować i skonfigurować agenta na komputerze z uruchomionym roli systemu lokacji punktu połączenia programu Configuration Manager usługi, użyj jednej z następujących metod:
   * [Zainstaluj agenta przy użyciu Instalatora](log-analytics-windows-agent.md)
   * [Zainstaluj agenta przy użyciu wiersza polecenia](log-analytics-windows-agent.md)
   * [Zainstaluj agenta przy użyciu usługi Konfiguracja DSC automatyzacji Azure](log-analytics-windows-agent.md)

## <a name="import-collections"></a>Importuj kolekcje
Po utworzeniu połączenie OMS dodane do programu Configuration Manager i agent zainstalowany na komputerze z uruchomionym połączenia programu Configuration Manager z usługą roli systemu lokacji punktu, następnym krokiem jest, aby zaimportować kolekcje z programu Configuration Manager w OMS jako komputer grupy.

Po włączeniu Import informacje członkostwa kolekcji są pobierane co 3 godziny, aby zapewnić aktualność członkostwa w kolekcji. Można wyłączyć Import w dowolnym momencie.

1. W portalu OMS kliknij **ustawienia**.
2. Kliknij przycisk **grup komputerów** a następnie kliknij pozycję **SCCM** kartę.
3. Wybierz **członkostwa w kolekcji programu Configuration Manager importu** , a następnie kliknij przycisk **zapisać**.  
   ![Grupy komputerów — karta SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Wyświetlanie danych z programu Configuration Manager
Po utworzeniu połączenie OMS dodane do programu Configuration Manager i agent jest zainstalowany na komputerze z uruchomionym roli systemu lokacji punktu połączenia programu Configuration Manager service, dane od agenta są wysyłane do OMS. W OMS, kolekcji programu Configuration Manager są wyświetlane jako [grup komputerów](log-analytics-computer-groups.md). Możesz wyświetlić grupy z **programu Configuration Manager** w obszarze **grup komputerów** w **ustawienia**.

Zaimportowane kolekcje widać wykryto liczbę komputerów z członkostwa w kolekcji. Można również sprawdzić wielu kolekcji, które zostały zaimportowane.

![Grupy komputerów — karta SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Po kliknięciu jednej otworzy wyszukiwania, wyświetlanie wszystkich zaimportowane grupy albo wszystkie komputery, które należą do każdej grupy. Przy użyciu [wyszukiwania dziennika](log-analytics-log-searches.md), możesz uruchomić dokładnych analiz danych programu Configuration Manager.

## <a name="next-steps"></a>Kolejne kroki
* Użyj [wyszukiwania dziennika](log-analytics-log-searches.md) Aby wyświetlić szczegółowe informacje o danych programu Configuration Manager.
