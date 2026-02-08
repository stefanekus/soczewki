# Symulacja Soczewek Optycznych

Interaktywna aplikacja webowa do symulacji propagacji promieni świetlnych przez układ dwóch cienkich soczewek optycznych. Działa w przeglądarce — zero zależności, jeden plik HTML.

**[Uruchom symulację](https://stefanekus.github.io/soczewki/)**

---

## Spis treści

1. [O co chodzi w tej symulacji?](#o-co-chodzi-w-tej-symulacji)
2. [Podstawy optyki — kluczowe pojęcia](#podstawy-optyki--kluczowe-pojęcia)
3. [Równanie soczewki cienkiej](#równanie-soczewki-cienkiej)
4. [Trzy promienie główne](#trzy-promienie-główne)
5. [Parametry symulacji](#parametry-symulacji)
6. [Co można zasymulować](#co-można-zasymulować)
7. [Presety](#presety)
8. [Aberracja chromatyczna](#aberracja-chromatyczna)
9. [Jak czytać wyniki](#jak-czytać-wyniki)
10. [Szczegóły implementacji](#szczegóły-implementacji)

---

## O co chodzi w tej symulacji?

Symulacja pokazuje, jak promienie światła przechodzą przez układ dwóch soczewek optycznych i tworzą obraz na ekranie. Możesz w czasie rzeczywistym zmieniać parametry układu (pozycje, ogniskowe) i obserwować, jak wpływa to na:

- przebieg promieni,
- pozycję i rozmiar obrazu,
- ostrość obrazu na ekranie,
- powiększenie układu optycznego.

Dzięki temu można intuicyjnie zrozumieć działanie prostych układów optycznych — od lupy i mikroskopu, po teleskop.

---

## Podstawy optyki — kluczowe pojęcia

### Soczewka cienka

Idealizowany model soczewki, w którym zakładamy, że jej grubość jest zaniedbywalnie mała w porównaniu z odległościami w układzie. Cała refrakcja (załamanie światła) zachodzi w jednej płaszczyźnie — **płaszczyźnie soczewki**.

W symulacji soczewki są narysowane jako:
- **Soczewka skupiająca** (f > 0) — kształt dwuwypukły z łukami, strzałki skierowane na zewnątrz
- **Soczewka rozpraszająca** (f < 0) — kształt dwuwklęsły, strzałki skierowane do wewnątrz

### Ogniskowa (f)

**Ogniskowa** to odległość od środka soczewki do jej **ogniska** (punktu skupienia). Określa „siłę" soczewki:

- **f > 0**: soczewka skupiająca (zbierająca) — promienie równoległe po przejściu zbiegają się w ognisku
- **f < 0**: soczewka rozpraszająca — promienie równoległe po przejściu rozbiegają się, jakby wychodziły z wirtualnego ogniska
- Im mniejsza wartość |f|, tym soczewka silniejsza (bardziej zakrzywia promienie)

### Ognisko przednie i tylne

Każda soczewka ma dwa ogniska leżące na osi optycznej:

- **Ognisko przednie (F)** — leży po stronie, z której pada światło. Promień skierowany ku niemu po przejściu przez soczewkę biegnie równolegle do osi.
- **Ognisko tylne (F')** — leży po stronie, na którą wychodzi światło. Promień równoległy do osi po przejściu przez soczewkę trafia właśnie do tego punktu.

W symulacji:
- **F₁, F₁'** — ogniska soczewki L₁ (kolor fioletowy)
- **F₂, F₂'** — ogniska soczewki L₂ (kolor turkusowy)

### Oś optyczna

Linia prosta przechodząca przez środki obu soczewek. W symulacji jest narysowana jako pozioma linia przerywana. Wszystkie elementy układu (źródło, soczewki, ekran) leżą wzdłuż tej osi.

### Obiekt (źródło)

Punkt lub przedmiot, którego obraz chcemy uzyskać. W symulacji mamy **dwa punkty źródłowe** umieszczone symetrycznie nad i pod osią optyczną (na wysokości ±40 jednostek). Dzięki temu widzimy, jak cały układ odwzorowuje rozciągły przedmiot.

### Obraz

Punkt (lub obszar), w którym promienie wychodzące z jednego punktu źródła zbiegają się po przejściu przez układ optyczny.

- **Obraz rzeczywisty** — promienie faktycznie się przecinają; można go uchwycić na ekranie. Powstaje, gdy odległość obrazowa d_i > 0.
- **Obraz pozorny** — promienie rozbiegają się, ale ich przedłużenia (wstecz) przecinają się w jednym punkcie. Nie da się go uchwycić na ekranie, ale oko go widzi (np. w lupie). Powstaje, gdy d_i < 0.

### Powiększenie

Stosunek rozmiaru obrazu do rozmiaru przedmiotu:

```
M = rozmiar_obrazu / rozmiar_przedmiotu
```

- |M| > 1 — obraz powiększony
- |M| < 1 — obraz pomniejszony
- M > 0 — obraz prosty (ta sama orientacja co przedmiot)
- M < 0 — obraz odwrócony (góra-dół)

---

## Równanie soczewki cienkiej

Fundamentalne równanie optyki soczewkowej:

```
1/f = 1/dₒ + 1/dᵢ
```

Gdzie:
- **f** — ogniskowa soczewki
- **dₒ** — odległość przedmiotowa (od przedmiotu do soczewki), zawsze > 0 dla obiektów rzeczywistych
- **dᵢ** — odległość obrazowa (od soczewki do obrazu)
  - dᵢ > 0 → obraz rzeczywisty (po drugiej stronie soczewki)
  - dᵢ < 0 → obraz pozorny (po tej samej stronie co przedmiot)

Przekształcając:

```
dᵢ = f · dₒ / (dₒ − f)
```

**Powiększenie pojedynczej soczewki:**

```
m = −dᵢ / dₒ
```

Znak minus oznacza, że obraz rzeczywisty jest odwrócony.

### Układ dwóch soczewek

W symulacji mamy dwie soczewki (L₁ i L₂). Obliczenia przebiegają kaskadowo:

1. **Obraz przez L₁**: Stosujemy równanie soczewki z dₒ₁ = x (odległość źródła od L₁) i f₁. Obliczamy dᵢ₁.
2. **Obraz L₁ staje się przedmiotem dla L₂**: Odległość przedmiotowa dla L₂ to dₒ₂ = d − dᵢ₁ (odległość między soczewkami minus odległość obrazu od L₁).
3. **Obraz końcowy przez L₂**: Stosujemy równanie soczewki z dₒ₂ i f₂. Obliczamy dᵢ₂.

**Powiększenie całkowite:**

```
M = m₁ · m₂ = (−dᵢ₁/dₒ₁) · (−dᵢ₂/dₒ₂)
```

---

## Trzy promienie główne

Z każdego punktu źródłowego symulacja rysuje **trzy promienie charakterystyczne** — to standardowa metoda graficznego wyznaczania obrazu w optyce:

### Promień 1 — Równoległy do osi

- Wychodzi z punktu źródłowego **równolegle** do osi optycznej
- Po przejściu przez soczewkę biegnie przez **ognisko tylne** (F')
- W symulacji: propagowany dalej przez drugą soczewkę z tą samą zasadą

### Promień 2 — Przez środek soczewki

- Wychodzi z punktu źródłowego i przechodzi przez **środek soczewki** (punkt, w którym oś optyczna przecina płaszczyznę soczewki)
- Nie ulega załamaniu — przechodzi na wylot bez zmiany kierunku
- To dlatego, że w modelu cienkiej soczewki obie powierzchnie w środku są równoległe

### Promień 3 — Przez ognisko przednie

- Wychodzi z punktu źródłowego w kierunku **ogniska przedniego** (F) soczewki
- Po przejściu przez soczewkę biegnie **równolegle** do osi optycznej
- Jest to odwrotność promienia 1

**Punkt przecięcia** tych trzech promieni (po przejściu przez soczewkę) wyznacza pozycję obrazu danego punktu źródłowego.

W symulacji każdy promień jest śledzony przez **obie** soczewki — na pierwszej zachowuje swoją „specjalną" właściwość, a na drugiej podlega standardowej refrakcji paraksjalnej.

---

## Parametry symulacji

Pięć suwaków steruje geometrią układu:

| Parametr | Opis | Zakres | Jednostka |
|----------|------|--------|-----------|
| **x** | Odległość źródła od soczewki L₁ | 50–500 | j.u. (jednostki umowne) |
| **d** | Odległość między soczewkami L₁ i L₂ | 30–500 | j.u. |
| **y** | Odległość ekranu od soczewki L₂ | 50–600 | j.u. |
| **f₁** | Ogniskowa soczewki L₁ | −300 do 300 | j.u. |
| **f₂** | Ogniskowa soczewki L₂ | −300 do 300 | j.u. |

- Wartości dodatnie f → soczewka skupiająca
- Wartości ujemne f → soczewka rozpraszająca
- Wartość f = 0 jest automatycznie pomijana (niefizyczna — nieskończona moc)

---

## Co można zasymulować

### 1. Pojedyncza soczewka skupiająca

Ustaw f₂ na maksymalną wartość (300) — wtedy druga soczewka prawie nie wpływa na promienie. Zmieniaj x, aby zobaczyć:
- **x > f₁**: obraz rzeczywisty, odwrócony. Im bliżej x → f₁, tym obraz dalej i większy.
- **x = f₁**: promienie wychodzą równolegle — obraz w nieskończoności.
- **x < f₁**: obraz pozorny (po tej samej stronie co źródło), prosty, powiększony — działanie lupy.

### 2. Powiększanie i pomniejszanie

Reguluj x i y, obserwując pasek informacyjny:
- Powiększenie |M| > 1 oznacza, że obraz jest większy od przedmiotu
- Powiększenie |M| < 1 oznacza pomniejszenie
- Dopasuj y tak, aby ekran trafił w pozycję obrazu — wtedy promienie zbiegają się w punkt (obraz ostry)

### 3. Układ afokalny (teleskop)

Użyj presetu **Teleskop** lub ustaw ręcznie d = f₁ + f₂. W tym układzie:
- Ognisko tylne L₁ pokrywa się z ogniskiem przednim L₂
- Wiązka równoległa wchodzi → wiązka równoległa wychodzi (ale z inną szerokością)
- Powiększenie kątowe = −f₁/f₂
- Obraz jest „w nieskończoności" — panel informacyjny pokaże to

### 4. Mikroskop

Ustaw:
- x nieco większe od f₁ (np. f₁=100, x=110) — L₁ daje duże powiększenie
- d duże (np. 300)
- f₂ małe — L₂ (okular) dodatkowo powiększa

Obserwuj, jak łączne powiększenie M rośnie do dużych wartości.

### 5. Soczewka rozpraszająca

Ustaw f₁ lub f₂ na wartość ujemną. Obserwuj:
- Promienie po przejściu przez soczewkę rozpraszającą rozbiegają się
- Obraz jest zawsze pozorny (dla pojedynczej soczewki rozpraszającej z obiektem rzeczywistym)
- Kształt soczewki zmienia się na dwuwklęsły

### 6. Układ korekcyjny

Połącz soczewkę skupiającą z rozpraszającą (np. f₁ = 100, f₂ = −80). Takie układy stosuje się w rzeczywistej optyce do korekcji aberracji i uzyskania pożądanych właściwości obrazowania.

### 7. Ostrość obrazu

Przesuwaj suwak y (pozycja ekranu), szukając pozycji, w której:
- Promienie z jednego źródła zbiegają się do jednego punktu na ekranie
- Panel informacyjny pokaże znacznik **✔ ostry** gdy ekran jest blisko płaszczyzny ogniskowej

Gdy ekran nie jest w płaszczyźnie obrazu, promienie tworzą rozproszone punkty — to jest **rozmycie** (defokusacja).

---

## Presety

### Zwyczajny przypadek

```
x = 200, d = 200, f₁ = 120, f₂ = 80, y = 250
```

Typowy układ dwóch soczewek skupiających. Źródło jest dalej niż ogniskowa L₁, więc L₁ tworzy obraz rzeczywisty, który następnie jest dalej przetwarzany przez L₂. Powiększenie jest umiarkowane, obraz odwrócony i rzeczywisty.

### Teleskop

```
x = 500, d = 225, f₁ = 150, f₂ = 75, y = 300
```

Układ afokalny: d = f₁ + f₂ = 150 + 75 = 225. Ognisko tylne L₁ pokrywa się z ogniskiem przednim L₂. Dalekie źródło (x = 500) jest bliskie nieskończoności. Układ działa jak teleskop refraktorowy (luneta Keplera) z powiększeniem kątowym −f₁/f₂ = −2×.

---

## Aberracja chromatyczna

Po włączeniu checkboxa **Aberracja chromatyczna** każdy promień zostaje rozbity na trzy składowe:

| Składowa | Kolor | Współczynnik ogniskowej |
|----------|-------|------------------------|
| Czerwona (R) | Czerwony | f × 1.05 |
| Zielona (G) | Zielony | f × 1.00 |
| Niebieska (B) | Niebieski | f × 0.95 |

### Dlaczego tak się dzieje?

Światło białe składa się z fal o różnych długościach. Współczynnik załamania szkła (lub innego materiału soczewki) zależy od długości fali — jest to zjawisko **dyspersji**:

- **Światło niebieskie** (krótka fala) — załamuje się silniej → efektywna ogniskowa jest **krótsza**
- **Światło czerwone** (długa fala) — załamuje się słabiej → efektywna ogniskowa jest **dłuższa**

W rezultacie:
- Poszczególne kolory ogniskują się w różnych punktach
- Na ekranie widać kolorowe obwódki zamiast ostrych punktów
- Efekt jest szczególnie widoczny daleko od osi optycznej

### Jak to eliminować?

W rzeczywistej optyce stosuje się **achromaty** — zestawy soczewek z różnych szkieł (np. crown + flint), które kompensują dyspersję. W symulacji można to przybliżyć, łącząc soczewkę skupiającą z rozpraszającą o odpowiednio dobranych ogniskowych.

---

## Jak czytać wyniki

### Panel informacyjny

W prawym górnym rogu paska kontrolnego wyświetlane są:

- **Pow:** — powiększenie całkowite M (np. `Pow: -1.50×` oznacza 1.5-krotne powiększenie, odwrócony obraz)
- **Rzeczywisty / Pozorny** — typ obrazu końcowego
- **Prosty / Odwrócony** — orientacja obrazu względem przedmiotu
- **Obraz: ... od L₂** — odległość obrazu od drugiej soczewki (w jednostkach umownych)
- **✔ ostry** — pojawia się, gdy ekran jest ustawiony w płaszczyźnie obrazu (±5 j.u.)
- **(ekran +30)** — o ile jednostek ekran jest za daleko (+) lub za blisko (−) od pozycji ostrego obrazu

### Elementy na canvas

| Element | Opis |
|---------|------|
| Przerywana linia pozioma | Oś optyczna |
| Pomarańczowe punkty z poświatą | Punkty źródłowe (obiekt) |
| Turkusowe kształty z łukami | Soczewki (L₁, L₂) |
| Fioletowe punktki na osi | Ogniska soczewki L₁ (F₁, F₁') |
| Turkusowe punktki na osi | Ogniska soczewki L₂ (F₂, F₂') |
| Szara pionowa linia z kreskowaniem | Ekran |
| Kolorowe linie | Promienie świetlne |
| Złoty pasek na ekranie | Obraz (średnia pozycja, gdzie padają promienie) |
| Wymiary pod osią | Odległości x, d, y |

---

## Szczegóły implementacji

### Śledzenie promieni (ray tracing)

Symulacja wykorzystuje **paraksjalny ray tracing** — przybliżenie, w którym promienie biegną blisko osi optycznej i tworzą małe kąty. Dla cienkiej soczewki transformacja promienia jest opisana wzorem:

```
nachylenie_wyjściowe = nachylenie_wejściowe − h / f
```

Gdzie:
- `nachylenie` = dy/dx (tangens kąta promienia z osią)
- `h` = wysokość, na jakiej promień trafia w soczewkę
- `f` = ogniskowa soczewki

Wysokość promienia na soczewce nie zmienia się (soczewka jest cienka), zmienia się tylko kąt.

### Propagacja między elementami

Między soczewkami (i od źródła do L₁, i od L₂ do ekranu) promień biegnie po linii prostej:

```
h_nowa = h_stara + nachylenie × odległość
```

### Współrzędne

- Początek układu współrzędnych: środek soczewki L₁
- Źródło: x = −P.x
- Soczewka L₁: x = 0
- Soczewka L₂: x = d
- Ekran: x = d + y
- Oś Y: prostopadła do osi optycznej (w górę = dodatnia)

### Viewport

Widok na canvasie dynamicznie dopasowuje się do rozmiarów układu i zasięgu promieni (z ograniczeniem do ±350 j.u. w pionie, żeby uniknąć ekstremalnego oddalenia przy silnej dywergencji).

---

## Technologia

- **HTML5 Canvas** — renderowanie 2D
- **Vanilla JavaScript** — zero bibliotek i frameworków
- **CSS Flexbox** — responsywny layout
- Jeden plik `index.html`, ~440 linii kodu
- Obsługa HiDPI (Retina) przez `devicePixelRatio`
