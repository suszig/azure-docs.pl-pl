Można wykorzystać wiele okazji do monitorowania maszyn wirtualnych przez zbierania, wyświetlanie i analizowanie danych diagnostycznych i danych logowania. Przeprowadzenie proste [monitorowania](../articles/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) maszyny wirtualnej, można użyć ekran Przegląd dla maszyny Wirtualnej w portalu Azure. Można użyć [rozszerzenia](../articles/virtual-machines/windows/extensions-features.md) Aby skonfigurować diagnostykę na maszynach wirtualnych, aby zbierać dodatkowe dane metryki. Można również użyć bardziej zaawansowane opcje monitorowania, takich jak [usługi Application Insights](../articles/application-insights/app-insights-overview.md) i [analizy dzienników](../articles/log-analytics/log-analytics-overview.md).

## <a name="diagnostics-and-metrics"></a>Diagnostyka i metryki 

Można skonfigurować i monitorować zbiorem [dane diagnostyczne](https://docs.microsoft.com/cli/azure/vm/diagnostics) przy użyciu [metryki](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) w portalu Azure, interfejsu wiersza polecenia Azure, programu Azure PowerShell i programowania programowania interfejsów aplikacji (API). Można na przykład:

- **Obserwować podstawowe metryki dla maszyny Wirtualnej.** Na ekranie informacje w portalu Azure podstawowe metryki wyświetlane obejmują użycie procesora CPU, użycia sieci, łączna liczba bajtów na dysku i operacji dysku na sekundę.

- **Włącz zbieranie diagnostyki rozruchu i wyświetl ją za pomocą portalu Azure.** Podczas przełączania własny obraz na platformie Azure albo jeden z obrazów platformy nawet rozruch, może istnieć wiele przyczyn, dlaczego pobiera Maszynę wirtualną do stanu rozruchowego. Można łatwo włączyć diagnostyki rozruchu podczas tworzenia maszyny Wirtualnej, klikając **włączone** dla diagnostyki rozruchu w sekcji monitorowanie ustawień ekranu.

    Jak rozruchu maszyn wirtualnych, agenta diagnostyki rozruchu przechwytuje dane wyjściowe rozruchu i zapisuje go w magazynie Azure. Te dane mogą służyć do rozwiązywania problemów rozruchu maszyny Wirtualnej. Diagnostyki rozruchu nie są automatycznie włączone po utworzeniu maszyny Wirtualnej z narzędzi wiersza polecenia. Przed włączeniem diagnostyki rozruchu, konta magazynu musi być utworzony do przechowywania dzienników rozruchu. Jeśli włączysz diagnostyki rozruchu w portalu Azure, konta magazynu są tworzone automatycznie.

    Jeśli nie włączysz diagnostyki rozruchu podczas tworzenia maszyny Wirtualnej, można zawsze włączyć ją później za pomocą [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [programu Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmbootdiagnostics), lub [szablonu usługi Azure Resource Manager](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Włącz opcję zbierania danych diagnostycznych systemu operacyjnego gościa.** Podczas tworzenia maszyny Wirtualnej, możesz mieć możliwość na ekranie ustawienia, aby włączyć diagnostykę systemu operacyjnego gościa. Po włączeniu zbierania danych diagnostycznych [IaaSDiagnostics rozszerzenia dla systemu Linux](../articles/virtual-machines/linux/diagnostic-extension.md) lub [IaaSDiagnostics rozszerzenia dla systemu Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) zostanie dodany do maszyny Wirtualnej, dzięki czemu można zbierać dodatkowe dane na dysku, procesora CPU i pamięci.

    Za pomocą danych diagnostycznych zebranych, można skonfigurować funkcję skalowania automatycznego dla maszyn wirtualnych. Można także skonfigurować dzienniki, aby przechowywać dane i Konfigurowanie alertów, aby użytkownik wiedział, gdy wydajność nie ma jeszcze prawo.

## <a name="alerts"></a>Alerty

Można utworzyć [alerty](../articles/monitoring-and-diagnostics/monitoring-overview-alerts.md) oparciu metryki wydajności zależnych. Przykładami problemów, które użytkownik może otrzymywać alerty o średnie użycie Procesora przekroczy określony próg lub wolnego miejsca na dysku spada poniżej pewnego. Alerty można skonfigurować w [portalu Azure](../articles/monitoring-and-diagnostics/insights-alerts-portal.md)za pomocą [programu Azure PowerShell](../articles/monitoring-and-diagnostics/insights-alerts-powershell.md), lub [interfejsu wiersza polecenia Azure](../articles/monitoring-and-diagnostics/insights-alerts-command-line-interface.md).

## <a name="azure-service-health"></a>Azure Service Health

[Kondycja usługi Azure](../articles/service-health/service-health-overview.md) zapewnia spersonalizowane wskazówki i pomocy technicznej, gdy dotyczy problemów dotyczących usług platformy Azure i pomaga przygotować się na nadchodzące zaplanowanej konserwacji. Kondycja usługi Azure alerty możesz i zespołów przy użyciu docelowych i elastyczne powiadomienia.

## <a name="azure-resource-health"></a>Azure Resource Health

[Kondycja zasobów Azure](../articles/service-health/resource-health-overview.md) ułatwia diagnozowanie i uzyskać pomoc techniczną, gdy Azure problem ma wpływ na zasoby. Informuje o bieżącej i wcześniejszej kondycji zasobów oraz pomaga uniknąć problemów. Usługa Resource Health oferuje pomoc techniczną w przypadku problemów z usługą platformy Azure.

## <a name="logs"></a>Dzienniki

[Dziennika aktywności platformy Azure](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md) jest Dziennik subskrypcji, która zapewnia wgląd w zdarzenia na poziomie subskrypcji, które wystąpiły na platformie Azure. Dziennik zawiera szereg danych z usługi Azure Resource Manager danych operacyjnych do aktualizacji na zdarzenia kondycji usługi. Możesz kliknąć dziennik aktywności w portalu Azure, aby wyświetlić dziennik dla maszyny Wirtualnej.

Oto niektóre czynności, które można zrobić za pomocą dziennika aktywności:

- Utwórz [alert dotyczący zdarzenia dziennika aktywności](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).
- [Strumienia go do Centrum zdarzeń](../articles/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) dla wprowadzanie przez usługi innej firmy lub rozwiązania analizy niestandardowych, takich jak usługi Power BI.
- Przeanalizuj go za pomocą usługi Power BI [pakiet zawartości usługi Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Zapisz go na konto magazynu](../articles/monitoring-and-diagnostics/monitoring-archive-activity-log.md) inspekcji archiwizacji lub ręcznie. Można określić czas przechowywania (w dniach) przy użyciu profilu dziennika.

Można również dostęp do danych dziennika aktywności, za pomocą [programu Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/monitor), lub [interfejsów API REST Monitor](https://docs.microsoft.com/rest/api/monitor/).

[Azure dzienników diagnostycznych](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) są dzienniki emitowane przez maszyny Wirtualnej, które zawierają rozbudowane, często dane dotyczące jego działania. Dzienniki diagnostyczne różnią się od dziennik aktywności, zapewniając wgląd w działania wykonywane w ramach maszyny Wirtualnej.

Czynności, które można wykonywać za pomocą dzienników diagnostycznych, należą:

- [Zapisz je na konto magazynu](../articles/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) inspekcji inspekcji lub ręcznie. Można określić czas przechowywania (w dniach) przy użyciu ustawień diagnostycznych zasobu.
- [Strumienia je do usługi Event Hubs](../articles/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) dla wprowadzanie przez usługi innej firmy lub rozwiązania analizy niestandardowych, takich jak usługi Power BI.
- Analizuj je za pomocą [analizy dzienników OMS](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie

- [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/) zapewnia możliwości korygowania monitorowania, alertów i alertów w chmurze i lokalnych zasobów. Rozszerzenie można zainstalować na [maszyny Wirtualnej systemu Linux](../articles/virtual-machines/linux/extensions-oms.md) lub [maszyny Wirtualnej systemu Windows](../articles/virtual-machines/windows/extensions-oms.md) instaluje agenta pakietu OMS i rejestruje maszyny Wirtualnej w istniejącym obszarem roboczym pakietu OMS.

- [Zaloguj się Analytics](../articles/log-analytics/log-analytics-overview.md) jest usługą OMS, który monitoruje chmurze i lokalnych środowiskach utrzymywać ich dostępności i wydajności. Zbiera ona dane generowane przez zasoby w środowiskach chmurowych i lokalnych oraz inne narzędzia do monitorowania, aby przeprowadzać analizę na podstawie wielu źródeł.

    Dla systemów Windows i maszyn wirtualnych systemu Linux zbierania dzienników i metryki zaleca przez zainstalowanie agenta analizy dzienników. Najprostszym sposobem zainstalowania agenta analizy dzienników na maszynie Wirtualnej jest za pośrednictwem [rozszerzenia maszyny Wirtualnej analizy dziennika](../articles/log-analytics/log-analytics-azure-vm-extension.md). Przy użyciu rozszerzenia upraszcza proces instalacji i automatycznie konfiguruje agenta do wysyłania danych do obszaru roboczego analizy dzienników, który określisz. Agent jest również uaktualniana automatycznie, sprawdzeniu, czy masz najnowsze funkcje i poprawki.

- [Monitor sieci](../articles/network-watcher/network-watcher-monitoring-overview.md) umożliwia monitorowanie maszyny Wirtualnej i jej skojarzonych zasobów w odniesieniu do sieci, w których są one. Można zainstalować rozszerzenia sieci obserwatorów agenta na [maszyny Wirtualnej systemu Linux](../articles/virtual-machines/linux/extensions-nwa.md) lub [maszyny Wirtualnej systemu Windows](../articles/virtual-machines/windows/extensions-nwa.md).

## <a name="next-steps"></a>Następne kroki
- Wykonać kroki w [umożliwia monitorowanie maszyny wirtualnej systemu Windows przy użyciu programu Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) lub [monitorowanie maszyny wirtualnej systemu Linux z wiersza polecenia platformy Azure](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Dowiedz się więcej o najlepszych rozwiązaniach wokół [monitorowania i diagnostyki](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
