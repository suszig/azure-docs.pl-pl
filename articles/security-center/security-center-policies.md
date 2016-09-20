<properties
   pageTitle="Ustawianie zasad zabezpieczeń w usłudze Azure Security Center | Microsoft Azure"
   description="Ten dokument zawiera informacje pomocne podczas konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń Azure."
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
   ms.date="08/26/2016"
   ms.author="yurid"/>

# Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure
Ten dokument zawiera informacje i niezbędne instrukcje pomocne podczas konfigurowania zasad zabezpieczeń w usłudze Security Center.

## Czym są zasady zabezpieczeń?
Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, które są zalecane dla zasobów w określonej subskrypcji lub grupie zasobów. W usłudze Security Center można zdefiniować zasady dla grupy zasobów lub subskrypcji Azure zgodnie z potrzebami zabezpieczeń firmy i typem aplikacji oraz poufnością danych w poszczególnych subskrypcjach.

Na przykład zasoby używane do celów deweloperskich lub testowania mogą mieć inne wymagania dotyczące zabezpieczeń niż te, które są używane przez aplikacje produkcyjne. Aplikacje z danymi podlegającymi ochronie (takimi jak dane osobowe) mogą wymagać wyższego poziomu zabezpieczeń. Zasady zabezpieczeń włączone w usłudze Azure Security Center regulują monitorowanie i zalecenia dotyczące zabezpieczeń, aby ułatwić znajdowanie potencjalnych luk i unikanie zagrożeń. Przeczytaj [Przewodnik planowania i użytkowania Centrum zabezpieczeń Azure](security-center-planning-and-operations-guide.md), aby uzyskać więcej informacji na temat sposobu określenia, która opcja jest bardziej odpowiednia dla Ciebie.

## Ustawianie zasad zabezpieczeń dla subskrypcji

Zasady zabezpieczeń można skonfigurować dla każdej subskrypcji lub grupy zasobów. Aby zmodyfikować zasady zabezpieczeń, musisz być właścicielem lub współautorem subskrypcji. Uzyskaj dostęp do usługi Azure Portal i postępuj zgodnie z poniższymi krokami, aby skonfigurować zasady zabezpieczeń w usłudze Security Center:

1. Kliknij kafelek **Zasady** na pulpicie nawigacyjnym usługi Security Center.

2. W bloku **Zasady zabezpieczeń — Zdefiniuj zasady dla subskrypcji lub grupy zasobów**, który zostaje otwarty po prawej stronie, wybierz subskrypcję, dla której chcesz włączyć zasady zabezpieczeń. Jeśli wolisz włączyć zasady zabezpieczeń dla grupy zasobów, a nie dla całej subskrypcji, przewiń w dół do następnej sekcji, w której opisano konfigurowanie zasad zabezpieczeń dla grupy zasobów.

    ![Definiowanie zasad](./media/security-center-policies/security-center-policies-fig1-ga.png)

3. Zostanie otwarty blok **Zasady zabezpieczeń** dla tej subskrypcji z zestawem opcji (podobny do przedstawionego poniżej):

    ![Włączanie zbierania danych](./media/security-center-policies/security-center-policies-fig2-ga.png)

    Opcje dostępne w tym bloku to:
    - **Zasady zapobiegania**: ta opcja umożliwia konfigurowanie zasad dla subskrypcji lub grupy zasobów.  
    - **Powiadomienie e-mail**: powiadomienie e-mail jest wysyłane po pierwszym wystąpieniu alertu w ciągu dnia i tylko w przypadku alertów o wysokiej ważności. Preferencje poczty e-mail można konfigurować tylko dla zasad subskrypcji. Więcej informacji dotyczących sposobu konfigurowania powiadomień e-mail można znaleźć w temacie [Provide security contact details in Azure Security Center](security-center-provide-security-contact-details.md) (Wprowadzanie danych kontaktowych na potrzeby zabezpieczeń w usłudze Azure Security Center). 
    - **Warstwa cenowa**: użyj tej opcji, aby przeprowadzić uaktualnienie z wyboru warstwy cenowej. Odwiedź [stronę usługi Security Center](https://azure.microsoft.com/pricing/details/security-center/), aby dowiedzieć się więcej o cenach.

    
4.  Upewnij się, że opcja **Zbieraj dane z maszyn wirtualnych** jest włączona (**Wł.**). Ta opcja umożliwia automatyczne zbieranie danych dziennika dla istniejących i nowych zasobów. 

    >[AZURE.NOTE] Zaleca się włączenie funkcji zbierania danych dla każdej subskrypcji, ponieważ pozwala to zagwarantować, że monitorowanie zabezpieczeń jest dostępne dla wszystkich istniejących i nowych maszyn wirtualnych. Włączenie zbierania danych instaluje agenta monitorowania. Jeśli nie chcesz włączyć funkcji zbierania danych z tej lokalizacji, możesz zrobić to później w widokach kondycji i zaleceń. Możesz również włączyć funkcję zbierania danych tylko dla subskrypcji lub dla wybranych maszyn wirtualnych. Więcej informacji na temat obsługiwanych maszyn wirtualnych znajduje się w artykule [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md).

5. Jeśli konto magazynu nie jest jeszcze skonfigurowane, po otwarciu karty **Zasady zabezpieczeń** może zostać wyświetlone ostrzeżenie podobne do pokazanego na ekranie poniżej:

    ![Wybór magazynu](./media/security-center-policies/security-center-policies-fig2.png)

6. Jeśli to ostrzeżenie zostanie wyświetlone, kliknij tę opcję i wybierz region, jak pokazano na poniższym ekranie:

    ![Wybór magazynu](./media/security-center-policies/security-center-policies-fig3-ga.png)

7. Dla każdego regionu, w którym zostały uruchomione maszyny wirtualne, wybierz konto magazynu, w którym będą przechowywane dane z tych maszyn. Dzięki temu można łatwo przechowywać dane w tym samym obszarze geograficznym w celu zachowania prywatności i poufności danych. Po wybraniu regionu, którego będziesz używać, wybierz region, a następnie wybierz konto magazynu.

8. W bloku **Wybieranie kont magazynu** kliknij przycisk **OK**.

    > [AZURE.NOTE] Jeśli wolisz, możesz agregować dane z maszyn wirtualnych w różnych regionach w jednym centralnym koncie magazynu. Więcej informacji znajduje się w temacie [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md).

9. W bloku **Zasady zabezpieczeń** kliknij przycisk **Wł.**, aby włączyć zalecenia dotyczące zabezpieczeń, których chcesz użyć dla tej subskrypcji. Kliknij opcję **Zasady zapobiegania**. Zostanie otwarty blok **Zasady zabezpieczeń**, jak pokazano na poniższym ekranie: 

    ![Wybieranie zasad zabezpieczeń](./media/security-center-policies/security-center-policies-fig4-ga.png)

Użyj poniższej tabeli jako źródła informacji, aby zrozumieć do czego służą poszczególne opcje:

| Zasady | Gdy stan to Wł. |
|----- |-----|
| Aktualizacje systemu | Codziennie pobiera listę dostępnych aktualizacji zabezpieczeń i aktualizacji krytycznych z usługi Windows Update lub WSUS (w zależności od tego, która z nich została skonfigurowana dla danej maszyny wirtualnej) i zaleca zastosowanie brakujących aktualizacji. Sprawdza najnowsze aktualizacje w systemach Linux przy użyciu systemu zarządzania pakietami udostępnionego wraz z dystrybucją, aby ustalić, dla których pakietów są dostępne aktualizacje. Sprawdza również aktualizacje zabezpieczeń i aktualizacje krytyczne z maszyn wirtualnych usługi [Cloud Services](./cloud-services/cloud-services-how-to-configure.md). |
| Luki w zabezpieczeniach systemu operacyjnego | Codziennie analizuje konfiguracje systemu operacyjnego, które mogą sprawić, że maszyna wirtualna będzie bardziej narażona na atak, i zaleca zmiany konfiguracji, aby usunąć te luki w zabezpieczeniach. Więcej informacji na temat określonych monitorowanych konfiguracji znajduje się na [liście zalecanych linii bazowych](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). |
| Ochrona punktów końcowych | Zaleca ochronę punktów końcowych, które mają zostać aprowizowane dla wszystkich maszyn wirtualnych systemu Windows, aby ułatwić identyfikację oraz usunięcie wirusów, programów szpiegujących oraz innego złośliwego oprogramowania.|
| Szyfrowanie dysków | Zaleca włączenie szyfrowania dysków na wszystkich maszynach wirtualnych w celu zwiększenia ochrony magazynowanych danych. 
| Grupy zabezpieczeń sieci | Zaleca, aby [sieciowe grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md) były skonfigurowane do kontroli ruchu przychodzącego i wychodzącego do maszyn wirtualnych z publicznymi punktami końcowymi. Grupy zabezpieczeń sieci skonfigurowane dla podsieci będą dziedziczone przez wszystkie interfejsy sieciowe maszyny wirtualnej, chyba że określono inaczej. Oprócz sprawdzania, czy grupa zabezpieczeń sieci została skonfigurowana, ta opcja ocenia reguły zabezpieczeń ruchu przychodzącego, aby określić, czy istnieją jakieś, które zezwalają na ruch przychodzący. |
| Zapora aplikacji sieci Web | Zaleca się aprowizowanie zapory aplikacji sieci Web na maszynach wirtualnych, gdy: używany jest [Publiczny adres IP na poziomie wystąpienia](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) i skojarzone reguły zabezpieczeń ruchu przychodzącego grupy NSG są skonfigurowane do zezwalania na dostęp do portu 80/443. Zostaje użyty adres IP ze zrównoważonym obciążeniem (VIP), a skojarzone reguły równoważenia obciążenia i reguły NAT ruchu przychodzącego są konfigurowane do zezwalania na dostęp do portu 80/443. Więcej informacji zawiera artykuł [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md) (Obsługa Menedżera zasobów Azure dla Load Balancer). |
| Zapora nowej generacji | Powoduje rozszerzenie ochrony sieci poza sieciowe grupy zabezpieczeń, które są wbudowane w platformę Azure. Centrum zabezpieczeń wykryje wdrożenia, dla których zaleca się zaporę nowej generacji, i pozwoli na aprowizację urządzenia wirtualnego. |
| Inspekcja SQL | Zaleca się, by inspekcja dostępu do baz danych i serwerów SQL Azure była włączona w celu zapewnienia zgodności, zaawansowanego wykrywania i na potrzeby analizy. |
| Niewidoczne szyfrowanie danych SQL | Zaleca się, aby funkcja szyfrowania była włączona dla baz danych SQL na platformie Azure, powiązanych z nimi kopii zapasowych i plików dziennika transakcji. Dzięki temu nawet w przypadku włamania się do danych, nie będzie można ich odczytać. |
    
Po skonfigurowaniu wszystkich opcji kliknij przycisk **OK** w bloku **Zasady zabezpieczeń**, który zawiera zalecenia, a następnie kliknij pozycję **Zapisz** w bloku **Zasady zabezpieczeń**, który zawiera ustawienia początkowe.

## Ustawianie zasad zabezpieczeń dla grup zasobów

Jeśli wolisz skonfigurować zasady zabezpieczeń dla każdej grupy zasobów, procedura wygląda podobnie jak w przypadku konfigurowania zasad zabezpieczeń dla subskrypcji. Główna różnica polega na tym, że trzeba rozwinąć nazwę subskrypcji i wybrać grupę zasobów, dla której chcesz skonfigurować unikatowe zasady zabezpieczeń:

![Wybór grupy zasobów](./media/security-center-policies/security-center-policies-fig5-ga.png)

Po wybraniu grupy zasobów zostanie otwarty blok **Zasady zabezpieczeń**. Domyślnie włączona jest opcja **Dziedziczenie**, co oznacza, że wszystkie zasady zabezpieczeń dla tej grupy zasobów są dziedziczone z poziomu subskrypcji. Tę konfigurację można zmienić, jeśli potrzebujesz niestandardowych zasad zabezpieczeń dla grupy zasobów. Jeśli tak jest, wybierz opcję **Unikatowe** i wprowadź zmiany w obszarze **Zasady zapobiegania**.

![Zasady zabezpieczeń dla grupy zasobów](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [AZURE.NOTE] Jeśli między zasadami na poziomie subskrypcji a zasadami na poziomie grupy zasobów występuje konflikt, pierwszeństwo mają zasady na poziomie grupy zasobów.


## Zobacz też

W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
- [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
- [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure



<!--HONumber=sep16_HO1-->


