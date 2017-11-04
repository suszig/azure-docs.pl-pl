Teraz, dodano wyzwalacz, czas jego coś zrobić interesujące z danymi, który jest generowany przez wyzwalacz. Wykonaj następujące kroki, aby dodać **SFTP — folder wyodrębniania** akcji. Ta akcja wyodrębnia zawartość pliku po spełnieniu warunków zdefiniowane. 

Aby skonfigurować to akcji, należy podać następujące informacje. Można zauważyć, że są to łatwy w użyciu dane generowane przez wyzwalacz jako dane wejściowe dla niektórych właściwości dla nowego pliku:

| SFTP - extract właściwości folderu | Opis |
| --- | --- |
| Ścieżka pliku archiwum źródła |To jest ścieżka do pliku wyodrębniania. Możesz wybrać jedną z tokenów z wcześniejszych akcji lub przejść serwera SFTP można znaleźć ścieżki pliku. |
| Ścieżka folderu docelowego |Jest to ścieżka, gdzie zostaną umieszczone wyodrębnione pliki. Można wybrać jednego z tokenów z wcześniejszych akcji jako ścieżka docelowa lub serwera SFTP Przeglądaj i wybierz ścieżkę. |
| Czy zastąpić? |Wskazuje, jeśli plik o takiej samej nazwie jak wyodrębnionego pliku znajduje się w ścieżce folderu docelowego, jeśli zastąpić istniejącego pliku, lub nie. |

Rozpocznijmy dodanie akcji, aby wyodrębnić pliki, jeśli wynikiem warunku zdefiniowanego wcześniej jest *True*. 

1. Wybierz **Dodaj akcję**.        
   ![Obraz warunku akcji SFTP 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Wybierz **SFTP — folder wyodrębniania** akcji      
   ![Obraz warunku akcji SFTP 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Wybierz **ścieżki pliku źródłowego archiwum**              
   ![Obraz warunku akcji SFTP 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Wybierz **ścieżka pliku** tokenu. To ustawienie określa, czy ścieżka pliku, który odnaleźć wyzwalacza będzie używany jako archiwum ścieżki pliku źródłowego.           
   ![Obraz warunku akcji SFTP 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Wybierz **ścieżkę folderu docelowego**           
   ![Obraz warunku akcji SFTP 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Wybierz **ścieżka pliku** tokenu. Oznacza to, że ścieżka pliku, który odnaleźć wyzwalacza będzie używany jako ścieżka docelowa dla wyodrębnionych plików.   
7. Wprowadź *\ExtractedFile* w **ścieżkę folderu docelowego** formantu. W tym zaraz po token ścieżka pliku w formancie ścieżka folderu docelowego.         
   ![Obraz warunku akcji SFTP 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Wprowadź *True* w **Zastąp?* sterowania, aby wskazać, że istniejące pliki powinny zostać zastąpione, jeśli mają taką samą nazwę jak wyodrębnić pliki.      
   ![Obraz warunku akcji SFTP 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Zapisz zmiany w przepływie pracy  

