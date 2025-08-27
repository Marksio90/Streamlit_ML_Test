
# Dokumentacja działania projektu **The Most Important Variables**

Projekt składa się z **4 modułów** oraz skryptu `main.py`, który spina wszystko w jedną sekwencję.

---

## **1. Moduł 1 – 01_target_detection.py**
**Cel:** Identyfikacja zmiennej docelowej (targetu) oraz rodzaju problemu (**regresja** / **klasyfikacja**).

**Kiedy uruchamiany:** Na samym początku, po wczytaniu danych przez `main.py`.

**Co robi:**
- Analizuje wszystkie kolumny w danych wejściowych.
- Jeżeli użytkownik podał nazwę kolumny (np. `--target AveragePrice`), używa jej.
- Jeżeli nie, próbuje zasugerować kolumnę na podstawie heurystyk.

**Efekt końcowy:**
- Nazwa wybranego targetu (`target`),
- Typ problemu (`typ` = `regresja` lub `klasyfikacja`),
- Szczegółowy ranking kolumn (w `meta['ranking']`).

---

## **2. Moduł 2 – 02_model_training.py**
**Cel:** Trening modelu ML dostosowanego do rodzaju problemu.

**Co robi:**
- Dzieli dane na zbiór treningowy i testowy (80/20).
- Przygotowuje cechy (standaryzacja, OneHotEncoder).
- Dobiera model z puli dla regresji lub klasyfikacji.
- Wybiera najlepszy model według metryk walidacyjnych.

---

## **3. Moduł 3 – 03_feature_report.py**
**Cel:** Analiza ważności cech i generowanie rekomendacji.

**Co robi:**
- Oblicza Permutation Importance.
- Generuje wykres ważności cech (PNG).
- Tworzy rekomendacje w formacie Markdown.

---

## **4. Moduł 4 – 04_final_report.py**
**Cel:** Tworzenie pełnego raportu HTML.

**Co robi:**
- Łączy metryki, wykres i rekomendacje w spójny raport.
- Zapisuje raport w formacie `.html`.

---

## **5. Skrypt main.py**
**Cel:** Automatyzacja całego procesu.

**Kroki:**  
1. Wczytanie danych.  
2. Próbkowanie (opcjonalnie).  
3. Uruchomienie Modułu 1 → wybór targetu i typu problemu.  
4. Uruchomienie Modułu 2 → trening modelu.  
5. Uruchomienie Modułu 3 → analiza ważności cech i rekomendacje.  
6. Uruchomienie Modułu 4 → wygenerowanie raportu HTML.

---

## **Parametry w aplikacji Streamlit**

| **Parametr** | **Opis** | **Domyślna wartość** | **Zakres / Uwagi** |
|--------------|-----------|----------------------|--------------------|
| **Wybierz kolumnę (lub zostaw puste dla AUTO)** | Określa kolumnę, która będzie celem predykcji (**target**). Jeśli zostawisz `AUTO`, system sam wybierze najlepszą kolumnę na podstawie heurystyk. | `AUTO` | - Wskaż ręcznie np. `AveragePrice` w zbiorze `avocado.csv`. |
| **Top N cech na wykresie** | Liczba najważniejszych cech do wyświetlenia na wykresie ważności. | `20` | Typowe wartości: 10–30. Przy mniejszej liczbie cech wynikowy wykres będzie bardziej czytelny. |
| **Permutation repeats** | Liczba powtórzeń przy obliczaniu **Permutation Importance**. Więcej powtórzeń = większa dokładność, ale dłuższy czas obliczeń. | `5` | `3` – szybka analiza; `5` – balans szybkości i dokładności; `10+` – dokładniejsze dla małych zbiorów danych. |
| **Test size** | Procent danych przeznaczony do testowania modelu (reszta służy do trenowania). | `0.2` (20%) | `0.1` – więcej danych do trenowania, mniej testowych; `0.3–0.4` – większy zbiór testowy, lepsza ocena jakości modelu. |

---

## **Przykład użycia w terminalu**
```bash
python main.py --data avocado.csv --target AveragePrice --outdir out
```
Dla szybszego testu na próbce danych:
```bash
python main.py --data avocado.csv --target AveragePrice --sample-n 3000 --outdir out
```
