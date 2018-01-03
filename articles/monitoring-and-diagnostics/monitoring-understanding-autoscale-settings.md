---
title: Opis ustawienia skalowania automatycznego | Dokumentacja firmy Microsoft
description: "Podział szczegółowe ustawienia skalowania automatycznego i jak działają."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: cff2be1818417a19f36da08d8c2eaa227bb945ec
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="understand-autoscale-settings"></a>Zrozumienie Ustawienia skalowania automatycznego
Ustawienia skalowania automatycznego umożliwiają upewnij się, że masz jednostkom zasoby z uruchomionym obsłużyć wahania obciążenia aplikacji. Można skonfigurować ustawienia skalowania automatycznego wyzwolenie w oparciu metryki, które wskazują obciążenia lub wydajności, wyzwalacza lub w zaplanowanym czasie. Ten artykuł przedstawia szczegółowe anatomia ustawieniu skalowania automatycznego. Artykuł rozpoczyna się od zrozumienia schematu i właściwości ustawienia, a następnie przeprowadzi Cię przez typy innego profilu, które można skonfigurować, a na koniec omówiono sposób skalowania automatycznego ocenia profil, który można wykonać w dowolnym momencie.

## <a name="autoscale-setting-schema"></a>Schemat Ustawienia skalowania automatycznego
Aby zilustrować schematu Ustawienia skalowania automatycznego, następujące ustawienia skalowania automatycznego jest używany. Należy pamiętać, że to ustawienie skalowania automatycznego zawiera:
- Jeden profil 
- Ma dwie reguły metryki w tym profilu; jeden dla skalowalnego w poziomie i jeden do skalowania.
- Reguła skalowalnego w poziomie jest wyzwalana, gdy zestaw skali maszyny wirtualnej średni procent procesora CPU metryka jest większa niż 85% w ciągu ostatnich 10 minut.
- Reguła w skali jest wyzwalane, gdy średni zestaw skali maszyny wirtualnej jest mniej niż 60% dla ostatniej minucie.

> [!NOTE]
> Ustawienie może mieć wiele profilów, przejść do [profile](#autoscale-profiles) sekcji, aby dowiedzieć się więcej.
> Profil może istnieć wiele skalowalnych w poziomie reguły i w skali reguł zdefiniowanych, przejść do [sekcji oceny](#autoscale-evaluation) aby zobaczyć, jak są analizowane

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Sekcja | Nazwa elementu | Opis |
| --- | --- | --- |
| Ustawienie | ID | Identyfikator zasobu w ustawieniu skalowania automatycznego. Ustawienia skalowania automatycznego są zasobów usługi Azure Resource Manager. |
| Ustawienie | name | Nazwa ustawienia skalowania automatycznego. |
| Ustawienie | location | Lokalizacja w ustawieniu skalowania automatycznego. Ta lokalizacja może być inna niż lokalizacja zasobów są skalowane. |
| properties | identyfikatora URI zasobu docelowego | Identyfikator zasobu jest skalowana zasobu. Może mieć tylko jedno ustawienie automatycznego skalowania dla zasobów. |
| properties | Profile | Ustawienia skalowania automatycznego składa się z co najmniej jeden profil. Każdym uruchomieniu aparatu skalowania automatycznego wykonuje jeden profil. |
| Profil | name | Nazwa profilu, można wybrać dowolną nazwę, która pomaga zidentyfikować ten profil. |
| Profil | Capacity.Maximum | Moc maksymalna dozwolona. Gwarantuje, że funkcja automatycznego skalowania, podczas wykonywania tego profilu, nie obsługuje zasobu powyżej ten numer. |
| Profil | Capacity.minimum | Pojemność minimalne dozwolone. Gwarantuje, że funkcja automatycznego skalowania, podczas wykonywania tego profilu, nie obsługuje zasobu pod ten numer. |
| Profil | Capacity.default | Jeśli występuje problem z odczytem pomiar zasobów (w tym przypadku procesora "vmss1"), a obecna pojemność jest poniżej pojemności domyślnej następnie zapewnienie dostępności zasobów, skalowania automatycznego możliwość skalowania w poziomie domyślne. Jeśli pojemność bieżąca już jest wyższa niż wydajność domyślna, automatycznego skalowania zostanie nie skali w. |
| Profil | rules | Skalowania automatycznego jest automatycznie skalowany między przy użyciu reguł w profilu możliwości maksymalne i minimalne. Istnieje wiele reguł w profilu. Podstawowy scenariusz ma dwie reguły: je do określenia, kiedy skalowalnego w poziomie, a drugą do określenia, kiedy w skali. |
| Reguły | metricTrigger | Definiuje metryki warunek reguły. |
| metricTrigger | metricName | Nazwa metryki. |
| metricTrigger |  metricResourceUri | Identyfikator zasobu zasób, który emituje metryki. W większości przypadków jest taka sama jak zasobów są skalowane. W niektórych przypadkach może być inny, na przykład można skalować zestaw skali maszyny wirtualnej na podstawie liczby wiadomości w kolejce magazynu. |
| metricTrigger | Ziarnem czasu | Czas trwania metryki próbkowania. Na przykład ziarnem czasu = "PT1M" oznacza, że powinno być metryki agregowane co minutę metoda agregacji określony w "Statystyka." |
| metricTrigger | Statystyka | Metoda agregacji w okresie ziarnem czasu. Na przykład Statystyka = "Średni" i ziarnem czasu = "PT1M" oznacza, że powinno być metryki agregowane co 1 minutę, wykonując średnią. Ta właściwość decyduje o tym, jak jest próbkowany metryki. |
| metricTrigger | timeWindow | Ilość czasu, aby wyszukać metryki. Na przykład timeWindow = "PT10M" oznacza, że każdym uruchomieniu automatycznego skalowania, wysyła zapytanie metryki dla ostatnich 10 minut. Przedział czasu umożliwia Twoje metryki będą normalizowane i pozwala uniknąć reagowanie na nagłego przejściowej. |
| metricTrigger | timeAggregation | Metoda agregacji używane do agregowania miar próbki. Na przykład TimeAggregation = "Średni" powinien agregacji próbkowanych metryki wykonując średniej. W przypadku powyższych potrwać 10 próbek 1 minutę, a średnie je. |
| Reguły | scaleAction | Akcja wykonywana po wyzwoleniu metricTrigger reguły. |
| scaleAction | kierunek | "Zwiększyć" do skalowania, "Zmniejsz" w skali|
| scaleAction | wartość | Ile, aby zwiększyć lub zmniejszyć pojemność zasobu |
| scaleAction | cooldown | Ilość czasu oczekiwania po zakończeniu operacji skalowania przed skalowanie ponownie. Na przykład jeśli cooldown = "PT10M" po zakończeniu operacji skalowania, skalowania automatycznego nie będzie próbował skalować ponownie dla innego 10 minut. Cooldown jest umożliwienie metryk do utrwalania po Dodawanie lub usuwanie wystąpienia. |

## <a name="autoscale-profiles"></a>Profile skalowania automatycznego

Istnieją trzy typy profilów skalowania automatycznego:

1. **Regularne profilu:** najbardziej typowych profilu. Jeśli nie ma potrzeby skalowania zasobu inaczej oparty na dzień tygodnia lub w określonym dniu, następnie wystarczy skonfigurować profil regularne w Ustawienia skalowania automatycznego. Następnie można skonfigurować ten profil metryki reguły kiedy skalowalnego w poziomie i kiedy w skali. Powinien mieć tylko jeden profil regularne zdefiniowany.

    Profil przykład używane w tym artykule jest przykładem regularne profilu. Czy nie istnieje również możliwość ustawienia profilu skalowanie liczby wystąpień statycznych dla zasobu.

2. **Stała daty profilu:** z profilem regularne zdefiniowane, załóżmy, że masz ważne zdarzenie powtarzający się na 26 grudnia 2017 (PST) i ma minimalne i maksymalne pojemności zasobu różnić się w jednym dniu, ale nadal skalowanie na te same metryki . W takim przypadku należy dodać profil daty stałej do listy profile z ustawieniem. Profil jest skonfigurowany do uruchamiania tylko w dniu zdarzenia. Dla innych dnia regularne profilu jest wykonywana.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
3. **Profil cyklu:** takiego profilu umożliwia upewnij się, że ten profil jest zawsze używana na określony dzień tygodnia. Cyklu profile zawierają tylko czas rozpoczęcia, w związku z tym uruchomienia aż do następnego cyklu profilu lub profilu daty stałej ma ustawione uruchamianie. Ustawienie tylko jeden profil cyklu, skalowania automatycznego wykonuje ten profil, nawet jeśli dostępny jest profil regularne zdefiniowane w tym samym kontekście. Poniższych dwóch przykładach przedstawiono sposób użycia tego profilu:

    **Przykład 1 - vs dzień tygodnia. Weekendy** Załóżmy, że w weekendy ma maksymalną wydajność za 4, ale w dni robocze, ponieważ oczekuje większe obciążenie ma czy maksymalną pojemność się 10. W takim przypadku ustawienia zawiera dwa profile cyklu jego uruchomienie w weekendy, a drugą w dni robocze.
    Ustawienie będzie wyglądać następująco:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    Patrząc na poprzednie ustawienia, można zauważyć, że każdy profil cyklu harmonogramu, ten harmonogram Określa podczas profilu uruchamiania wykonywania. Profil zatrzymuje wykonywanie, gdy nadejdzie czas na wykonanie innego profilu.

    Na przykład w poprzednim ustawienie "weekdayProfile" jest ustawione uruchamianie w poniedziałek o godzinie 0, który oznacza, że ten profil, który rozpoczyna się wykonywanych w poniedziałek w 12.am. Nadal wykonywania do Sobota 12a.m. gdy "weekendProfile" jest zaplanowane do uruchomienia, wykonywania.

    **Przykład 2 - godzinami** wytłumaczone innym przykładem, może być mają metryki próg = "x" w godzinach pracy, 9: 00 do 17: 00, a następnie od 17: 00 Aby 9: 00 następnego dnia ma metryki próg jako "y".
    Ustawienie będzie wyglądać następująco:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Patrząc na poprzednie ustawienia, "businessHoursProfile" rozpoczyna się wykonywanych w poniedziałek w 9: 00 Dzięki temu wykonywanie do momentu 17: 00 ponieważ jest to, kiedy rozpoczyna "nonBusinessHoursProfile" wykonywanie. Wykonuje "nonBusinessHoursProfile" do 9: 00 Wtorek, a następnie przejmuje "businessHoursProfile". To jest powtarzany do piątku. 5, w tym momencie "nonBusinessHoursProfile" wykonuje aż do poniedziałek 9: 00 ponieważ "businessHoursProfile" nie można uruchomić wykonywania do poniedziałek 9: 00
    
> [!Note]
> Automatycznego skalowania środowiska użytkownika w portalu Azure wymusza zakończenia cyklu profilów i rozpoczęciu wykonywania w ustawieniu skalowania automatycznego profil domyślny Between profile cyklu.
    
## <a name="autoscale-evaluation"></a>Ocena skalowania automatycznego
Podane tego skalowania automatycznego ustawień może mieć wiele profilów skalowania automatycznego, a każdy profil może mieć wiele reguł metryki, które ważne jest, aby zrozumieć, jak są analizowane ustawieniu skalowania automatycznego. Zawsze uruchamia zadania automatycznego skalowania zaczyna, wybierając profil, który ma zastosowanie, po wybraniu profilu skalowania automatycznego ocenia min, max wartości i wszystkie metryki reguł w profilu i decyduje o tym, czy akcji skalowania jest konieczne.

### <a name="which-profile-will-autoscale-pick"></a>Profil, który będzie pobranie skalowania automatycznego
- Funkcja automatycznego skalowania najpierw wyszukiwana w każdej stałej Data profil, który jest skonfigurowany do uruchamiania teraz, jeśli istnieje, skalowania automatycznego go uruchomi. W przypadku wielu profilów dacie, które mogą uruchomić skalowania automatycznego wybierze pierwszy.
- Jeśli nie Brak profilów daty stałej, skalowania automatycznego analizuje profile cyklu, jeśli znaleziono, następnie go go uruchomi.
- Jeśli istnieją bez stałych lub cyklu profilów, a następnie skalowania automatycznego wykonania regularne profilu.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Sposób automatycznego skalowania ocenić wiele reguł?

Gdy funkcja automatycznego skalowania określa profil, który powinien wykonać, rozpoczyna się od obliczenia wszystkich reguł skalowania w poziomie w profilu (reguły z kierunku = "Zwiększyć").
- Jeśli jeden lub więcej reguł skalowania w poziomie są wyzwalane, skalowania automatycznego oblicza nowego miejsca ustaleniami scaleAction każdego z tych zasad. Następnie go skalować w poziomie maksymalną tych możliwości, aby zapewnić dostępność usługi.
- Na przykład: Jeśli istnieje zestaw skali maszyny wirtualnej z bieżącego pojemności 10 istnieją dwie reguły skalowania w poziomie; taki, który zwiększa wydajność przy 10%, a drugie zwiększa pojemność 3. Pierwsza reguła spowoduje powstanie nowego miejsca 11, a drugą regułę spowodowałoby pojemności 13. Aby zapewnić dostępność usługi skalowania automatycznego wybiera się, że wybrano akcję, która powoduje maksymalną pojemność, więc drugą regułę.

Jeśli żadne reguły skalowania w poziomie są wyzwalane, skalowania automatycznego ocenia wszystkich w skali reguł (reguły z kierunku = "Zmniejsz"). Skalowania automatycznego ma akcję skalowania tylko, jeśli wszystkie reguły w skali są wyzwalane.
- Funkcja automatycznego skalowania oblicza nowego miejsca ustaleniami scaleAction każdego z tych zasad. Następnie wybiera akcję skalowania, która powoduje maksymalną tych możliwości w celu zapewnienia dostępności usługi.
- Na przykład: Jeśli istnieje zestaw skali maszyny wirtualnej z bieżącego pojemności 10 i dwa w skali reguł; taki, który zmniejsza pojemność o 50%, a drugie zmniejsza pojemność przez 3. Pierwsza reguła spowoduje powstanie nowego pojemność wynoszącą 5, a drugą regułę spowodowałoby pojemności 7. Aby zapewnić dostępność usługi skalowania automatycznego wybiera się, że wybrano akcję, która powoduje maksymalną pojemność, więc drugą regułę.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat automatycznego skalowania można znaleźć w następujących zasobach:

* [Omówienie automatycznego skalowania](monitoring-overview-autoscale.md)
* [Azure metryki wspólnej skalowania automatycznego monitora](insights-autoscale-common-metrics.md)
* [Najlepsze praktyki dotyczące skalowania automatycznego Azure Monitor](insights-autoscale-best-practices.md)
* [Użyj akcji skalowania automatycznego do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach](insights-autoscale-to-webhook-email.md)
* [Interfejs API REST skalowania automatycznego](https://msdn.microsoft.com/library/dn931953.aspx)
