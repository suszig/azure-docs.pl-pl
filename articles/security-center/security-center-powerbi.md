<properties
   pageTitle="Uzyskiwanie szczegółowych informacji z Centrum zabezpieczeń Azure za pomocą usługi Power BI| Microsoft Azure"
   description="Pakiet zawartości usługi Power BI Centrum zabezpieczeń Azure ułatwia znajdowanie alertów zabezpieczeń, zaleceń, zaatakowanych zasobów i trendów na podstawie zestawu danych, który został utworzony na potrzeby raportu."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/03/2016"
   ms.author="yurid"/>

# Uzyskiwanie szczegółowych informacji z Centrum zabezpieczeń Azure za pomocą usługi Power BI
[Pulpit nawigacyjny usługi Power BI](http://aka.ms/azure-security-center-power-bi) w Centrum zabezpieczeń Azure umożliwia wizualizację, analizowanie i filtrowanie zaleceń oraz alertów zabezpieczeń z dowolnego miejsca — także z urządzenia przenośnego. Pulpit nawigacyjny usługi Power BI służy do wyświetlania trendów i wzorców ataków. Można wyświetlać alerty zabezpieczeń według zasobu lub adresu IP źródła oraz nierozwiązane zagrożenia bezpieczeństwa według zasobu lub wieku. Można również w różny sposób łączyć zalecenia z Centrum zabezpieczeń i alertów zabezpieczeń z innymi danymi, na przykład korzystając z [Dzienników inspekcji platformy Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) i usługi [Azure SQL Database Auditing](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/), które zawierają pulpity nawigacyjne programu Power BI, lub wyeksportować te dane do programu Excel, co pozwoli na łatwe raportowanie stanu zabezpieczeń zasobów w chmurze.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure.


##Uzyskiwanie dostępu do usługi Power BI za pomocą pulpitu nawigacyjnego Centrum zabezpieczeń Azure
Pulpit nawigacyjny Centrum zabezpieczeń Azure umożliwia również dostęp do raportów usługi Power BI. W tym celu wykonaj poniższe kroki: 

1. Na pulpicie nawigacyjnym **Centrum zabezpieczeń Azure** kliknij przycisk **Eksploruj w usłudze Power BI**.

    ![Łączenie z Centrum zabezpieczeń Azure przy użyciu usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig9-new.png) 

2. Po prawej stronie zostanie otwarty blok **Eksplorowanie w usłudze Power BI** w sposób przedstawiony poniżej:

    ![Łączenie z Centrum zabezpieczeń Azure przy użyciu usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig2-new.png)

3. Jeśli po raz pierwszy tworzysz pulpit nawigacyjny usługi Power BI, możesz wybrać jedną z poniższych opcji bloku Eksplorowanie w usłudze Power BI: 

    - **Pulpit nawigacyjny szczegółowych informacji o zabezpieczeniach**: wybierz tę opcję, jeśli chcesz utworzyć pulpit nawigacyjny, który wyświetla stan zabezpieczeń, wątki i wykrycia zagrożeń. Jest to częściej stosowana opcja dla roli DevOps, która odpowiada za analizowanie stanu ochrony oraz wykrytych alertów w subskrypcjach.
    - **Pulpit nawigacyjny zarządzania zasadami**: wybierz tę opcję, jeśli chcesz uzyskać informacje na temat zasad zarządzania i wymuszania.  Jest to opcja częściej stosowana w centralnym dziale IT, który skupia się na zagadnieniach związanych z zarządzaniem. Ten pulpit nawigacyjny może pomóc w uzyskaniu szczegółowych informacji na temat przestrzegania zasad zabezpieczeń w całej organizacji.
    - Jeśli masz już pulpit nawigacyjny usługi Power BI, kliknij opcję **Przejdź do bieżącego pulpitu nawigacyjnego usługi Power BI**.

4. Na potrzeby tego przykładu kliknij opcję **Pulpit nawigacyjny szczegółowych informacji o zabezpieczeniach**. Zostanie wyświetlone następujące okno:

    ![Pulpit nawigacyjny szczegółowych informacji o zabezpieczeniach Centrum zabezpieczeń Azure](./media/security-center-powerbi/security-center-powerbi-fig3-new.png)

5. Upewnij się, że wybrana **Metoda uwierzytelniania** to **oAuth2** i kliknij przycisk **Zaloguj**.
6. Zostanie otwarte okno **Power BI** zawierające raport o strukturze podobnej do przedstawionej poniżej:
    
    ![Pulpit nawigacyjny szczegółowych informacji o zabezpieczeniach](./media/security-center-powerbi/security-center-powerbi-fig5.png)

> [AZURE.NOTE] Zgodnie z harmonogramem odświeżanie raportu odbywa się codziennie. Jeśli odświeżenie nie powiedzie się, zobacz artykuł [Potencjalne problemy z odświeżaniem w usłudze Power BI w Centrum zabezpieczeń Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), aby uzyskać więcej informacji na temat rozwiązywania problemów.

Tutaj można zobaczyć liczbę alertów zabezpieczeń i zaleceń, a także liczbę maszyn wirtualnych, baz danych Azure SQL i zasobów sieciowych, które są monitorowane przez Centrum zabezpieczeń Azure.

Link do Centrum zabezpieczeń Azure przekierowuje użytkownika do portalu Azure. Wykresy ułatwiają wizualizowanie informacji o zaleceniach dotyczących zabezpieczeń i alertach, takich jak:

- Kondycja zabezpieczeń zasobów
- Całkowita liczba oczekujących zaleceń
- Zalecenia dotyczące maszyny wirtualnej
- Alerty w czasie
- Zaatakowane zasoby
- Zaatakowane adresy IP

Każdy wykres zawiera dodatkowe informacje szczegółowe. Wybierz kafelek, aby zobaczyć więcej informacji. Na przykład kafelek Kondycja zabezpieczeń zasobów przedstawia dodatkowe informacje na temat oczekujących zaleceń według zasobów, jak pokazano poniżej:

![Zalecenia](./media/security-center-powerbi/security-center-powerbi-fig6.png)

Po kliknięciu wybranego wiersza wykresu pozostałe wiersze są wyszarzane, co ułatwia skupienie się na wybranym wierszu. Aby powrócić do pulpitu nawigacyjnego, kliknij przycisk **Centrum zabezpieczeń Azure** w obszarze **Pulpity nawigacyjne** w lewym okienku na tej stronie.

> [AZURE.NOTE] Raporty można edytować, aby dostosować je przez dodanie dodatkowych pól lub zmianę istniejących elementów wizualnych. Przeczytaj temat [Interakcja z raportem w widoku do edycji programu Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/), aby uzyskać więcej informacji.

Kafelki **Alerty w czasie, Zaatakowane zasoby** i **Adresy IP osoby atakującej** będą wyświetlać podobne dane wyjściowe po kliknięciu każdego z nich. Dzieje się tak, ponieważ raport agreguje informacje dotyczące wszystkich trzech zmiennych i określa je jako **Zaatakowane zasoby**, jak przedstawiono poniżej:

![Zaatakowane zasoby](./media/security-center-powerbi/security-center-powerbi-fig7.png)

W tym momencie można zapisać kopię tego raportu, wydrukować ją lub opublikować w sieci Web za pomocą opcji dostępnych w menu **Plik**.

![Menu Plik](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## Eksplorowanie danych w Centrum zabezpieczeń Azure za pomocą usług Power BI

Połącz się z usługami [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) w usłudze Power BI i wykonaj następujące czynności:

1. W oknie **Pakiet zawartości dla usługi Power BI** zostaną wyświetlone dwie opcje w sposób przedstawiony poniżej.

    ![Pakiet zawartości usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

2. Na potrzeby tego przykładu kliknij przycisk **Uzyskaj dostęp** w kafelku **Zarządzanie zasadami Centrum zabezpieczeń Azure**.

3. Pamiętaj, aby w oknie **Łączenie z zarządzaniem zasadami Centrum zabezpieczeń Azure** wybrać opcję **oAuth2** na liście rozwijanej **Metoda uwierzytelniania**, tak jak przedstawiono poniżej, a następnie kliknij przycisk **Zaloguj**.

    ![Okno Zarządzanie zasadami](./media/security-center-powerbi/security-center-powerbi-fig4-new.png)

4. Nastąpi przekierowanie do strony uwierzytelniania, gdzie należy wpisać poświadczenia używane do nawiązywania połączeń z Centrum zabezpieczeń Azure. Po zakończeniu uwierzytelniania usługa Power BI rozpocznie importowanie danych używanych do tworzenia raportów. W tym czasie w prawym górnym rogu przeglądarki może zostać wyświetlony następujący komunikat:

    ![Łączenie z Centrum zabezpieczeń Azure przy użyciu usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Jeśli pulpit nawigacyjny jest tworzony po raz pierwszy, może to trwać dłużej niż zwykle — szczególnie w przypadku scenariuszy z wieloma subskrypcjami. 

5. Po zakończeniu procesu pulpit nawigacyjny usługi Power BI w Centrum zabezpieczeń Azure zostanie załadowany z raportem **Zarządzanie zasadami**.


## Następne kroki
W tym dokumencie omówiono korzystanie z usługi Power BI w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Przewodnik planowania i obsługi Centrum zabezpieczeń Azure](security-center-planning-and-operations-guide.md)— informacje o sposobie planowania wdrożenia Centrum zabezpieczeń Azure.
- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje na temat konfigurowania zabezpieczeń w Centrum zabezpieczeń Azure.
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=Jun16_HO2-->


