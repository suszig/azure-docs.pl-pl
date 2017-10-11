

| **Identyfikator limit** | **Limit** | **Komentarze** |
| --- | --- | --- |
| Łączna pojemność (w tym chmury) |Maksymalnie 64 TB dla urządzenia wirtualnego |Można przełączyć kompletny zestaw wirtualnego StorSimple do innej tablicy puste. Spróbuj przywrócić z tego samego urządzenia, upewnij się, że masz wystarczającą ilość miejsca na urządzeniu, aby wykonać tę operację. Po przekroczyła 32 TB, nie można przywrócić do tego samego urządzenia. |
| Maksymalna liczba poświadczeń konta magazynu na urządzenie |1 | |
| Maksymalna liczba woluminów/udziałów |16 | |
| Minimalny rozmiar udziału warstwowych |500 GB | |
| Minimalny rozmiar woluminu warstwowego |500 GB | |
| Maksymalny rozmiar udziału warstwowych |20 TB | |
| Maksymalny rozmiar woluminu warstwowego |5 TB | |
| Minimalny rozmiar udziału przypiętych lokalnie |50 GB | |
| Minimalny rozmiar woluminu przypiętego lokalnie |50 GB | |
| Maksymalny rozmiar udziału przypiętych lokalnie |2 TB | |
| Maksymalny rozmiar woluminu przypiętego lokalnie |200 GB | |
| Maksymalna liczba połączeń iSCSI z inicjatorów |512 | |
| Maksymalna liczba rekordów kontroli dostępu na urządzeniu |64 | |
| Maksymalna liczba kopie zapasowe przechowywane przez urządzenie wirtualne w *.backups* folderu dla serwera plików |5 |Obejmuje to najnowsza zaplanowane (generowane przez domyślne zasady tworzenia kopii zapasowej) i ręcznego tworzenia kopii zapasowych. |
| Maksymalna liczba zaplanowane kopie zapasowe przechowywane przez urządzenie |55 |30 codzienne wykonywanie kopii zapasowych<br>12 comiesięczne kopie zapasowe<br>13 corocznych kopii zapasowych |
| Maksymalna liczba ręcznego tworzenia kopii zapasowych, które są zachowane w urządzeniu |45 | |
| Maksymalna liczba plików na udziale serwera plików |1 mln |Podczas przywracania urządzenia, czas przywracania jest proporcjonalna do liczby plików przez wszystkie udziały na urządzeniu. |
| Maksymalna liczba plików dla każdego woluminu na serwerze iSCSI |1 mln | |
| Maksymalna liczba plików na tablicę wirtualnego |4 miliony | |
| Czas odzyskania przywracania |Szybkie przywracanie |Przywracanie jest oparta na mapie interakcji i zależy od rozmiaru woluminu.<br>Operacje tworzenia kopii zapasowej może wystąpić, gdy operacja przywracania jest w toku. |

