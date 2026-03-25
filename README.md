# Projekt Klasyfikacji Obrazów CIFAR-10

Ten projekt demonstruje budowę, trening i ewaluację konwolucyjnej sieci neuronowej (CNN) do klasyfikacji obrazów z zestawu danych CIFAR-10.

## 1. Opis Projektu

Celem tego projektu jest klasyfikacja 10 różnych kategorii obiektów na podstawie małych obrazów kolorowych (32x32 piksele) ze zbioru danych CIFAR-10. Wykorzystano do tego bibliotekę Keras (TensorFlow) do zbudowania i wytrenowania modelu CNN.

Projekt obejmuje następujące kroki:

- Załadowanie i wstępne przetwarzanie danych CIFAR-10.
- Definicja i kompilacja modelu CNN.
- Trening modelu z wykorzystaniem technik zapobiegających przetrenowaniu (Early Stopping, ReduceLROnPlateau).
- Ewaluacja modelu na zbiorze testowym.
- Wizualizacja przebiegu treningu (loss i accuracy).
- Analiza błędnych klasyfikacji i macierzy pomyłek.
- Zapis wytrenowanego modelu.

## 2. Zestaw Danych CIFAR-10

CIFAR-10 to popularny zestaw danych w dziedzinie widzenia komputerowego, składający się z 60 000 obrazów kolorowych 32x32 piksele w 10 klasach, po 6000 obrazów na klasę. Istnieje 50 000 obrazów treningowych i 10 000 obrazów testowych. Klasy to:

- `airplane`
- `automobile`
- `bird`
- `cat`
- `deer`
- `dog`
- `frog`
- `horse`
- `ship`
- `truck`

## 3. Architektura Modelu

Zastosowana architektura CNN jest sekwencyjnym modelem Keras i składa się z następujących warstw:

- Warstwa wejściowa: `Input(shape=(32, 32, 3))`
- Warstwy konwolucyjne (`Conv2D`) z aktywacją `relu`, przeplatane warstwami `MaxPooling2D` i `Dropout` w celu redukcji wymiarowości i zapobiegania przetrenowaniu.
    - `Conv2D(32, (3,3), activation='relu')`
    - `MaxPooling2D((2,2))`
    - `Dropout(0.1)`
    - `Conv2D(64, (3,3), activation='relu')`
    - `MaxPooling2D((2,2))`
    - `Dropout(0.2)`
    - `Conv2D(128, (3,3), activation='relu')`
    - `MaxPooling2D((2,2))`
    - `Dropout(0.3)`
- Warstwa spłaszczająca (`Flatten`).
- Warstwy gęsto połączone (`Dense`) z aktywacją `relu` i `softmax` na wyjściu:
    - `Dense(128, activation='relu')`
    - `Dropout(0.4)`
    - `Dense(num_classes, activation='softmax')`

## 4. Trening i Ewaluacja

Model został skompilowany z optymalizatorem `Adam`, funkcją straty `categorical_crossentropy` i metryką `accuracy`.

Podczas treningu zastosowano następujące techniki:

- **Early Stopping**: Monitoruje `val_accuracy` i zatrzymuje trening, jeśli dokładność walidacji nie poprawia się przez 5 epok.
- **ReduceLROnPlateau**: Zmniejsza współczynnik uczenia o połowę, jeśli `val_accuracy` nie poprawia się przez 3 epoki, z minimalnym współczynnikiem uczenia `1e-4`.

Trening odbywał się przez maksymalnie 100 epok, z `batch_size=32` i `validation_data` ustawionym na zbiór testowy.

## 5. Wyniki

Po zakończeniu treningu, model jest ewaluowany na zbiorze testowym. Wyniki ewaluacji obejmują:

- Dokładność na zbiorze testowym.
- Stratę na zbiorze testowym.
- Wykresy przedstawiające przebieg funkcji straty i dokładności w czasie treningu (na zbiorach treningowym i walidacyjnym).
- Macierz pomyłek, wizualizująca, jak dobrze model klasyfikuje każdą kategorię i gdzie popełnia błędy.
- Przykłady błędnie sklasyfikowanych obrazów, pokazujące oryginalny obraz, jego prawdziwą etykietę i predykcję modelu.

Wszystkie wykresy i wytrenowany model są zapisywane w folderze `colab_cifar10_exports` na Google Drive.

## 6. Struktura Projektu (Pliki)

Projekt generuje i zapisuje następujące pliki na Google Drive (w ścieżce zdefiniowanej przez `base_export_dir`):

- `training_loss_accuracy_YYYYMMDD_HHMM.png`: Wykresy straty i dokładności podczas treningu.
- `confusion_matrix_YYYYMMDD_HHMM.png`: Macierz pomyłek dla zbioru testowego.
- `my_model_YYYYMMDD_HHMM.keras`: Zapisany wytrenowany model Keras.
