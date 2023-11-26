# Teika Design Workshop

## Introdução

https://docs.google.com/presentation/d/1ijtLO4GDlUrYA_0LqwtGyvnSb82Mw6d3NYq1WYt-MTw/edit?usp=sharing

### Syntax

Abaixo vai componentes comuns de Teika, não é algo fixo e alterações irão definitivamente acontecer.

A combinação desses leva a uma série de features, os mais comuns são polimorfismo paramétrico, type constructors, tipos existenciais, tipos indutivos e tipos lineares.

```rust
// x, y : Variables
// A, B, T : Type
// M, N : Term
Term =
  | Type // type of all types
  | (x : A) -> B // type of a function
  | (x : A) => M // making a function
  | M N // calling a function
  | x @-> T // type of a fixpoint
  | x @=> M // making a fixpoint
  | @M // unrolling a fixpoint
  | x = M; N // let expression
  | { x : A; y : B; } // type of a record
  | { x = M; y = N; } // making a record
  | M.x // accessing a record
  | M : A // type annotation
  | (M) // parens
```

### Type of all types

Em Teika, tudo tem um tipo, sem exceções, então qual o tipo de um tipo? A resposta é `Type` e qual o tipo de `Type`? `Type`.

```rust
// polimorfismo
// note que o tipo do retorno da função depende no primeiro parametro
id
  : (A : Type) -> (x : A) -> A
  = (A : Type) => (x : A) => x;
// exemplos
id_string : (x : String) -> String = id String;
id_bool : (x : Bool) -> Bool = id Bool;
```

### Functions

O clássico de linguagens funcionais, permite falar de um termo de forma incompleta, introduzindo um "buraco", em Teika é o componente fundamental da computação.

Você pode ler o tipo `(x : A) -> B` como "assumindo um x do tipo A, então B" ou usando a interpretação de provas formais, "uma prova x de A, implica em uma prova de B".

```rust
Bool : Type;
true : Bool;
false : Bool;

Bool = (A : Type) -> (x : A) -> (y : A) -> A;
true = (A : Type) => (x : A) => (y : A) => x;
// inferencia
false = A => x => y => y;
```

### Fixpoint

É a forma de introduzir recursão, permite um termo referenciar ele mesmo, geralmente para recursão e introduzir tipos indutivos. Na prática inclui algumas restrições para evitar loops infinitos.

```rust
Bool : Type;
true : Bool;
false : Bool;

// leia como provando que P true e P false é verdade, então P b é verdade
// alternativemente, leia como Bool é true ou false
Bool = b @-> (P : Bool -> Type) -> (x : P true) -> (y : P false) -> P b;
// functiona por que b == true, então P b == P true
true = (P : Bool -> Type) => (x : P true) => (y : P false) => x;
```

### Linearidade

Uma restrição importante de Teika é que variáveis sejam usadas apenas uma vez, porém mantenha em mente que muitas coisas podem ser duplicados na mão. Na prática para esse exercicio, sinta-se a vontade para usar variáveis 1 ou 0 vezes.

```rust
Bool = (A : Type) -> (x : A) -> (y : A) -> A;
true : Bool;
false : Bool;

// se Bool for true, retorna (true, true), se for false, retorna (false, false)
clone = (b : Bool) => b (Bool, Bool) (true, true) (false, false);
```

## Problemas

### Tracking

O compilador se beneficiaria de trackear várias possibilidades,

### Duplicar e ignorar

Algumas coisas podem ser duplicadas de "graça". Algumas coisas podem serem ignoradas de "graça".

### Overloading

Seria dahora alguns nomes fazerem coisas diferentes.

### Mutabilidade

Linearidade permite mutabilidade de forma segura, como fazer isso? E como tornar isso conveniente?

### Borrow

Seria bom se referencias imutáveis pudessem ser copiadas de graça, como lidar com isso?

### Effects

Algebraic effects são uma forma mais ergonomica de trackear monads no sistema de tipos.
