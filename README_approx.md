# Vertex Cover · ApproxVertexCover · Algoritm 2-Aproximativ

> Aplicație web interactivă pentru vizualizarea **pas cu pas** a algoritmului `ApproxVertexCover` — un algoritm 2-aproximativ pentru problema **Acoperirii cu Vârfuri** (Vertex Cover Problem) pe grafuri neponderate.

---

## Despre proiect

Problema **Acoperirii cu Vârfuri** (*Vertex Cover Problem — VCP*) este una dintre problemele clasice NP-hard: dat un graf neorientat G=(V,E), găsește o submulțime S ⊆ V de **cardinal minim** astfel încât fiecare muchie să aibă cel puțin un capăt în S.

Această aplicație vizualizează algoritmul `ApproxVertexCover` prezentat în cadrul cursului **Algoritmi Avansați 2025 (c-4)**, Lect. Dr. Ștefan Popescu, FMI UniBuc.

---

## Algoritmul

### Pseudocod

```
ApproxVertexCover(G = (V, E)):
  E'  ←  E
  S   ←  ∅
  cât timp E' ≠ ∅:
      alege (x, y) ∊ E'
      S  ←  S ∪ {x, y}
      şterge din E' toate muchiile
          incidente lui x şi lui y
  return S
```

### Proprietăți

**Corectitudine:** Algoritmul returnează întotdeauna o acoperire validă.
La fiecare iterație, muchia aleasă `(x,y)` este acoperită prin adăugarea ambelor capete în S. Prin ștergerea tuturor muchiilor incidente, nicio muchie acoperită nu va fi reconsiderată. Bucla se termină când E' = ∅, adică toate muchiile sunt acoperite.

**Garanția de factor 2 (Teorema 2 din curs):**

Fie E* ⊂ E mulțimea muchiilor alese de algoritm (una per iterație). Aceste muchii sunt **nod-disjuncte** prin construcție (odată ce un nod intră în S, toate muchiile incidente lui sunt eliminate). Prin **Lema 1**:

```
OPT ≥ |E*|
```

Deoarece algoritmul adaugă exact 2 noduri per muchie din E*:

```
|S| = 2 · |E*| ≤ 2 · OPT
```

Prin urmare, costul soluției returnate nu depășește de **2 ori** optimul.

### De ce nu este algoritm greedy simplu?

Versiunea naivă care adaugă un singur nod per iterație (`S ← S ∪ {x}`) poate furniza un răspuns **arbitrar de slab** — de exemplu pe un graf stea, dacă se alege mereu câte un frunze în loc de centru, cardinalul soluției poate fi `n-1` față de optimul `1`. Adăugarea **ambelor** capete este cheia garanției de factor 2.

---

## Funcționalități

### Construirea grafului

| Acțiune | Efect |
|---|---|
| `Click` pe canvas | Adaugă un nod nou |
| `Shift + Click` pe două noduri | Conectează nodurile printr-o muchie |
| `Shift + Click` pe o muchie | Șterge muchia |
| `Trage` un nod | Mută nodul pe canvas |

### Controlul algoritmului

| Buton | Efect |
|---|---|
| **Inițializează** | Setează `E' ← E`, `S ← ∅` și pregătește execuția |
| **Pas următor** | Execută o singură iterație a buclei |
| **Rulează tot** | Termină toate iterațiile dintr-o dată |
| **Reset algoritm** | Repornește algoritmul pe același graf |
| **Șterge graful** | Resetează complet aplicația |

### Vizualizare

- **Pseudocodul** se evidențiază în timp real — liniile active sunt colorate în funcție de tipul operației (chihlimbar pentru control, verde pentru modificări ale soluției)
- **Muchiile alese** (E*) sunt afișate în chihlimbar solid
- **Muchiile eliminate** din E' devin gri punctate
- **Nodurile din S** sunt evidențiate în verde cu efect de strălucire
- **Soluția optimă** (calculată prin forță brută pentru n ≤ 20) este marcată cu un inel violet punctat în jurul fiecărui nod
- **Panoul de stare** afișează `E'`, `S`, muchia curent aleasă, jurnalul iterațiilor și, la final, demonstrația completă a garanției `|S| ≤ 2·OPT`

---

## Demonstrație vizuală recomandată

Pentru a observa comportamentul cel mai instructiv al algoritmului:

1. Construiește un **graf stea** (un nod central conectat la mai multe frunze) — algoritmul poate alege frunze înainte de centru, ilustrând de ce adăugarea ambelor capete este necesară
2. Construiește un **ciclu impar** (triunghi, pentagon) — vei observa că soluția aproximativă poate fi mai mare decât optimul
3. Construiește un **graf bipartit complet** K₂,₃ — optim = 2, aproximare = 4 în cazul nefavorabil

---

## Implementare

Aplicația este un **fișier HTML unic**, fără dependențe externe sau framework-uri.

### Structura codului

```
approx-vertex-cover.html
├── <style>           — interfață (CSS variabile, layout flex, animații)
├── <canvas>          — desenarea grafului (noduri, muchii, stări vizuale)
├── PSEUDOCOD[]       — structura statică a pseudocodului cu colorare sintaxă
├── Stare globală G   — { noduri, muchii, sel, muchie_hover }
├── Stare algoritm    — { E_prim, S, matching, eliminate, linii_active, jurnal }
├── Drag & drop       — mousedown / mousemove / mouseup
├── randeazaPseudocod() — evidențierea dinamică a liniilor active
├── randeazaStare()   — afișarea stării curente și a demonstrației
├── initAlg()         — inițializare E', S, pas = 0
├── pasAlg()          — o iterație completă a buclei while
├── ruleazaTot()      — execuție completă până la E' = ∅
└── rezolvaOptim()    — forță brută 2ⁿ pentru soluția exactă (n ≤ 20)
```

### Complexitate

| Operație | Complexitate |
|---|---|
| O iterație `pasAlg()` | O(\|E\|) |
| Execuție completă | O(\|V\| · \|E\|) |
| Soluție optimă (brute force) | O(2ⁿ · \|E\|), activă doar pentru n ≤ 20 |

---

## Utilizare

Nu necesită instalare. Deschide `approx-vertex-cover.html` în orice browser modern.

```bash
git clone https://github.com/utilizator/approx-vertex-cover
cd approx-vertex-cover
open approx-vertex-cover.html
```

---

## Referințe

- Algoritmi Avansați 2025, c-4 — Vertex Cover Problem, Linear Programming — Lect. Dr. Ștefan Popescu, FMI UniBuc
- Cormen, T.H. et al. — *Introduction to Algorithms*, Cap. 35 (Approximation Algorithms)

---

## Licență

MIT
