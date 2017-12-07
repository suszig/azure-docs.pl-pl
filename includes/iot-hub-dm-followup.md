## <a name="customize-and-extend-the-device-management-actions"></a>Dostosowywanie i rozszerzanie urządzenia akcje zarządzania

Z rozwiązania IoT można rozwinąć zdefiniowanym zestawem wzorców zarządzanie urządzeniami lub włączyć wzorce niestandardowych za pomocą urządzenia dwie i podstawowych metody chmury do urządzenia. Inne akcje zarządzania urządzeniami przykładami Resetowanie do ustawień fabrycznych, aktualizacji oprogramowania układowego, aktualizacji oprogramowania, zarządzania energią, Zarządzanie sieciami i łączności i szyfrowania danych.

## <a name="device-maintenance-windows"></a>Okna obsługi urządzeń

Zwykle możesz skonfigurować urządzenia do wykonywania akcji w określonym czasie zminimalizować przestoje i przestoje. Okna obsługi urządzenia są często używane wzorzec do czasu, gdy urządzenia należy zaktualizować jej konfiguracji. Rozwiązań zaplecza służy odpowiednie właściwości dwie urządzenia do definiowania i aktywować zasad na urządzeniu, który umożliwia okna obsługi. Gdy urządzenie otrzyma zasady okna konserwacji, służy zgłoszone właściwość dwie urządzenia do raportowania stanu zasady. Urządzenie dwie kwerendy następnie można użyć aplikacji zaplecza do potwierdzają zgodności urządzeń i każdej zasady.

## <a name="next-steps"></a>Następne kroki

W tym samouczku metoda bezpośrednia jest użyty do wyzwolenia zdalnego ponownego uruchomienia na urządzeniu. Używane właściwości zgłoszony do zgłaszania podczas ostatniego rozruchu z urządzenia, a zapytanie dwie urządzenia, aby dowiedzieć się, czas ostatniego ponownego uruchomienia urządzenia z chmury.

Aby kontynuować, wprowadzenie do korzystania z Centrum IoT i urządzenia zarządzania wzorców, takich jak zdalnego za pośrednictwem aktualizacji oprogramowania układowego udziału użytkownika, zobacz:

[Samouczek: Sposób wykonywania aktualizacji oprogramowania układowego][lnk-fwupdate]

Aby dowiedzieć się, jak rozszerzyć IoT, Twoje rozwiązanie i harmonogram metoda wywołuje na wielu urządzeniach, zobacz [emisji zadania i harmonogramu] [ lnk-tutorial-jobs] samouczka.

Aby kontynuować, wprowadzenie do korzystania z Centrum IoT, zobacz [wprowadzenie krawędzi IoT][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md