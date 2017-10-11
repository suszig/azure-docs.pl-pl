<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Aby Podłączanie kabli do urządzenia zasilania
> [!NOTE]
> Zarówno obudowy na urządzeniu StorSimple obejmują PCMs nadmiarowe. Dla każdej obudowy PCMs musi być zainstalowane i połączone z różnych źródeł napędu aby zapewnić wysoką dostępność.
> 
> 

1. Upewnij się, że przełączniki zasilania na wszystkich PCMs znajdują się w pozycji OFF.
2. Podstawowy obudowa Połącz kable do obu PCMs. Kable są oznaczone na czerwono na diagramie okablowania zasilania poniżej.
3. Należy upewnić się czy dwa PCMs na obudowę podstawowego źródła zasilania oddzielne.
4. Dołącz przewodów zasilania do włączania stojak jednostki dystrybucji zasilania pokazane zasilania okablowania diagramu.
5. Powtórz kroki od 2 do 4 dla obudowa EBOD.
6. Włącz obudowa EBOD przestawiając przycisk zasilania w każdej PCM pozycji dalej.
7. Sprawdź, czy obudowa EBOD jest włączona przez sprawdzenie, czy zielony LED tyłu kontrolera EBOD są włączone.
8. Włącz głównej obudowy przestawiając każdy przełącznik PCM pozycji dalej.
9. Sprawdź, czy system na zapewniając kontroler urządzenia, dla którego włączono LED.
10. Upewnij się, że połączenia między kontrolerem EBOD i kontrolerów urządzeń jest aktywny, upewniając się, że cztery LED obok portów SAS na kontrolerze EBOD są zielone.
    
    > [!IMPORTANT]
    > Aby zapewnić wysoką dostępność dla systemu, firma Microsoft zaleca ściśle zgodna zasilania okablowania schemat pokazano na poniższym diagramie.
    > 
    > 
    
    ![Podłączanie kabli do urządzenia 4U zasilania](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Okablowanie zasilania**
    
    | Etykieta | Opis |
    |:--- |:--- |
    | 1 |Obudowa podstawowego |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Kontrolera 0 |
    | 5 |Kontrolera 1 |
    | 6 |EBOD kontrolera 0 |
    | 7 |EBOD kontrolera 1 |
    | 8 |Obudowa EBOD |
    | 9 |PDU |

