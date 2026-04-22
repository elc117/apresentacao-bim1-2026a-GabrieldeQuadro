#  Parte Teórica — Listas e Logic Puzzles

## 3 pontos que compreendi melhor

### 1. Uso de is

Is é usado como "=" em outras linguagens, como apresentado.

em Uso X = 1+1

false

o uso correto seria X is 1+1

---

### 2. Uso de `member/2`

Permite afirmar que algo existe na lista:

```prolog
member(casa(bob,vermelha,_), Casas)
```

Muito útil para traduzir regras do problema.

---

### 3. Uso de `nextto/3` para vizinhança

Definição auxiliar:

```prolog
ao_lado(X, Y, List) :- nextto(X, Y, List).
ao_lado(X, Y, List) :- nextto(Y, X, List).
```

Facilita representar "ao lado de".

---

## 3 pontos mais difíceis

### 1. Ordem implícita da lista

A posição (esquerda/direita) não é explícita, exige interpretação mental.

---

### 2. Múltiplas restrições juntas

Difícil entender como o Prolog combina tudo via backtracking.

---

### 3. Garantia de unicidade

Sem `permutation/2`, nem sempre fica claro se há repetição de elementos.

---

## Processo para sanar dúvidas

### Uso de `permutation/2`

Pesquisei e entendi que ajuda a gerar todas as combinações possíveis.

---

### Uso de `trace`

Permite visualizar execução passo a passo no Prolog.

---

### Testes isolados com `nextto`

Ajudou a entender melhor o conceito de adjacência.

---

## Fontes

* Documentação SWI-Prolog
* Testes práticos no interpretador
* Materiais de apoio da disciplina

---

# Parte Prática — Consultas em Prolog

## Predicado: `drama_artist/1`

###  Definição

```prolog
drama_artist(A) :-
    (actor(M, A) ; actress(M, A)),
    genre(M, drama).
```

### Teste

```prolog
?- drama_artist(A).
```

### Observação

Pode gerar repetições → usar:

```prolog
?- setof(A, drama_artist(A), Lista).
```
---

## Predicado: `movieAgeByName/2`

### Definição

```prolog
movieAgeByName(M, Age) :-
    movie(_, M, Year),
    get_time(T),
    stamp_date_time(T, DateTime, local),
    date_time_value(year, DateTime, CurrentYear),
    Age is CurrentYear - Year.
```

### Teste

```prolog
?- movieAgeByName(parasite, Age).
```

### Alternativa simples

```prolog
movieAgeByName(M, Age) :-
    movie(_, M, Year),
    Age is 2026 - Year.
```

---


## Predicado: `recommend/2`

### Definição

```prolog
recommend(U, M) :-
    likes(U, MovieLiked),
    genre(MovieLiked, G),
    movie(ID, M, _),
    genre(ID, G),
    ID \= MovieLiked.
```

### Teste

```prolog
?- recommend(arthur, M).
```

### Evitar repetição

```prolog
?- setof(M, recommend(arthur, M), Lista).
```

---

## Predicado: `count_users/1`

### Definição

```prolog
count_users(C) :-
    findall(U, likes(U,_), L),
    list_to_set(L, Unique),
    length(Unique, C).
```

### Teste

```prolog
?- count_users(C).
```

---

# Problemas encontrados

## Duplicação de resultados

Solução:

* `setof/3`
* `list_to_set/2`

---

## Confusão entre ID e nome do filme

Exemplo:

```prolog
likes(arthur, 2).
movie(2, tusk, 2014).
```

Precisa sempre fazer a correspondência entre ID e nome.

---

