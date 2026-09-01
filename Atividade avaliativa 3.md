# Experimento Computacional: Ordenação de Pedidos

Este repositório contém o desenvolvimento de um experimento computacional para comparar o desempenho de quatro algoritmos de ordenação (**Bubble Sort**, **Insertion Sort**, **Selection Sort** e **Quick Sort**). O objetivo é avaliar a quantidade de operações (comparações e movimentações) que cada algoritmo realiza à medida que o volume de dados cresce.

---

## 💻 Código do Experimento (Python)

O código abaixo executa a ordenação e faz a contagem exata das operações de cada algoritmo, utilizando uma estrutura simplificada.

```python
import random

def bubble_sort(arr):
    comp = 0
    trocas = 0
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            comp += 1
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                trocas += 1
    return comp, trocas

def insertion_sort(arr):
    comp = 0
    mov = 0
    n = len(arr)
    for i in range(1, n):
        chave = arr[i]
        j = i - 1
        while j >= 0:
            comp += 1
            if arr[j] > chave:
                arr[j + 1] = arr[j]
                mov += 1
                j -= 1
            else:
                break
        arr[j + 1] = chave
    return comp, mov

def selection_sort(arr):
    comp = 0
    trocas = 0
    n = len(arr)
    for i in range(n):
        menor = i
        for j in range(i + 1, n):
            comp += 1
            if arr[j] < arr[menor]:
                menor = j
        if menor != i:
            arr[i], arr[menor] = arr[menor], arr[i]
            trocas += 1
    return comp, trocas

comp_quick = 0
mov_quick = 0

def quick_sort(arr, l, h):
    global comp_quick, mov_quick
    if l < h:
        pivo = arr[h]
        i = l - 1
        for j in range(l, h):
            comp_quick += 1
            if arr[j] < pivo:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
                mov_quick += 1
        arr[i + 1], arr[h] = arr[h], arr[i + 1]
        mov_quick += 1
        p = i + 1
        
        quick_sort(arr, l, p - 1)
        quick_sort(arr, p + 1, h)

# Bloco de testes para gerar e rodar os experimentos
tamanhos = [10, 20, 1000]

for tam in tamanhos:
    # Gerando o vetor original aleatório
    original = [random.randint(1, 10000) for _ in range(tam)]
    
    # Cópias idênticas para cada algoritmo
    v_bubble = original.copy()
    v_insertion = original.copy()
    v_selection = original.copy()
    v_quick = original.copy()
    
    # Executando e guardando resultados
    b_comp, b_trocas = bubble_sort(v_bubble)
    i_comp, i_mov = insertion_sort(v_insertion)
    s_comp, s_trocas = selection_sort(v_selection)
    
    comp_quick = 0
    mov_quick = 0
    quick_sort(v_quick, 0, len(v_quick) - 1)
    
    print(f"\n--- TAMANHO {tam} ---")
    print(f"Bubble Sort    -> Comparações: {b_comp} | Trocas: {b_trocas}")
    print(f"Insertion Sort -> Comparações: {i_comp} | Movimentações: {i_mov}")
    print(f"Selection Sort -> Comparações: {s_comp} | Trocas: {s_trocas}")
    print(f"Quick Sort     -> Comparações: {comp_quick} | Movimentações: {mov_quick}")
```

---

## 📊 Tabela de Resultados

Os valores coletados durante a execução dos testes com dados aleatórios foram organizados na estrutura abaixo:

| Tamanho | Bubble Comp. | Bubble Trocas | Insertion Comp. | Insertion Mov. | Selection Comp. | Selection Trocas | Quick Comp. | Quick Mov. |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **10** | 45 | 19 | 27 | 19 | 45 | 7 | 29 | 19 |
| **20** | 190 | 83 | 101 | 83 | 190 | 17 | 83 | 62 |
| **1.000** | 499.500 | 237.493 | 238.487 | 237.493 | 499.500 | 992 | 10.626 | 5.677 |

* **Critério de contagem:** Uma comparação é computada quando dois valores são testados nas condicionais. Uma troca ou movimentação é somada quando o dado muda de posição na memória do vetor.

---

## 📝 Análise dos Resultados

* **a) Qual algoritmo realizou o menor número de comparações para 10 elementos?**  
  O **Insertion Sort** realizou o menor número, somando apenas 27 comparações.

* **b) Qual algoritmo realizou menos trocas ou movimentações?**  
  O **Selection Sort** foi o que menos moveu elementos na memória, registrando apenas 7 trocas no tamanho 10 e mantendo esse padrão baixo mesmo para 1.000 elementos (992).

* **c) O comportamento observado para 10 elementos permaneceu semelhante quando o tamanho aumentou para 20?**  
  **Sim.** A proporção de crescimento continuou igual. O Selection seguiu com poucas trocas, o Insertion continuou eficiente em comparações dentro do grupo quadrático e o Quick Sort começou a se destacar.

* **d) O que aconteceu com a quantidade de operações quando o vetor passou para 1.000 elementos?**  
  Houve um **crescimento explosivo e muito alto** nos algoritmos quadráticos (Bubble, Insertion e Selection), alcançando centenas de milhares de operações. Em contraste, o Quick Sort manteve um número muito baixo (10.626 comparações).

* **e) Bubble Sort, Insertion Sort e Selection Sort apresentam complexidade $O(n^2)$ em situações típicas. Eles apresentaram exatamente a mesma quantidade de operações?**  
  **Não.** Mesmo pertencendo à mesma classe teórica, o comportamento prático muda. O Bubble e o Selection fazem o mesmo número de comparações, mas o Selection faz muito menos trocas. Já o Insertion diminui as comparações quase pela metade porque para de procurar assim que acha a posição correta.

* **f) Qual algoritmo apresentou maior crescimento no número de operações?**  
  O **Bubble Sort**, pois necessita fazer o limite máximo de comparações e executa um volume massivo de trocas a cada inversão encontrada.

* **g) Como o comportamento experimental do Quick Sort se diferenciou dos demais algoritmos?**  
  O Quick Sort cresceu de forma **linear-logarítmica**, sendo muito mais lento para acumular operações. Enquanto os outros passaram de 490 mil operações, o Quick resolveu com cerca de 10 mil.

* **h) Os resultados encontrados são coerentes com as complexidades teóricas estudadas?**  
  **Sim.** Os três primeiros algoritmos seguiram a curva de crescimento quadrática ($n^2$), e o Quick Sort provou sua eficiência teórica seguindo a curva $O(n \log n)$.

* **i) Se você fosse responsável pelo sistema da central de distribuição e precisasse ordenar milhares de pedidos, qual dos quatro algoritmos escolheria?**  
  Eu escolheria o **Quick Sort**. Como sistemas reais manipulam grandes volumes de dados a todo momento, usar algoritmos quadráticos geraria lentidão e alto consumo de processamento. O Quick Sort garante respostas rápidas.

---

## ⚡ Desafio Adicional

A organização inicial do vetor modifica o comportamento dos códigos de maneiras distintas:

* **Vetor já ordenado:** O **Insertion Sort** se torna o melhor de todos, fazendo apenas $n-1$ comparações e zero movimentações. O Quick Sort com pivô fixo no final perde sua eficiência e cai para o pior caso ($O(n^2)$).
* **Vetor em ordem inversa:** O Bubble Sort e o Insertion Sort realizam o número máximo de operações possíveis. O Selection Sort mantém o mesmo número de comparações fixas do caso aleatório.

**Conclusão:** A ordenação inicial dos dados **não interfere da mesma maneira** em todos os algoritmos. Alguns se beneficiam muito da pré-ordenação (como o Insertion), enquanto outros mantêm um trabalho rígido (Selection) ou até perdem rendimento (Quick Sort com pivô fixo).
