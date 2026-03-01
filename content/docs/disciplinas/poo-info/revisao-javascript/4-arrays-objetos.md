---
title: "4. Arrays e Objetos"
weight: 4
bookHidden: false
---

# 4. Arrays e Objetos

Arrays e objetos são as principais estruturas de dados em JavaScript — e entender como elas se relacionam ajuda a compreender a linguagem de forma mais profunda.

Um objeto é um conjunto de propriedades: cada propriedade tem um nome (sempre uma string) e um valor associado. Um array, por sua vez, é um objeto especializado cujas propriedades são índices inteiros (0, 1, 2...), com comportamentos extras para lidar com sequências ordenadas de dados.

Na prática, arrays são usados para armazenar e manipular coleções — uma lista de nomes, uma sequência de resultados. Objetos, por outro lado, agrupam propriedades e valores sob um contexto comum — e um de seus usos mais comuns é justamente descrever entidades: um Produto, um Estudante, um Usuário. São, por isso, a base introdutória do estudo de orientação a objetos.

---

## Arrays

### O que é um array?

Um array é uma sequência ordenada de valores. Cada valor é chamado de elemento e ocupa uma posição numérica no array, conhecida como índice. O primeiro elemento sempre está no índice 0.

Arrays em JavaScript não têm tipo fixo: podem conter números, strings, booleanos ou qualquer outro valor — inclusive outros arrays. Essa última possibilidade permite representar estruturas como matrizes (arrays de arrays).

### Criando e acessando elementos

A forma mais direta de criar um array é através de um literal:

```javascript
let dados = [];
let numeros = [1, 2, 3, 4, 5];
let varios = [false, [], 5, "a"];
```

Os valores de um literal não precisam ser constantes — podem ser expressões calculadas no momento da criação:

```javascript
let x = 10;
let calculados = [x * 2, x + 1, Math.sqrt(x)]; // [20, 11, 3.16...]
```

Arrays literais também podem conter objetos ou outros arrays:

```javascript
let matriz = [
  [1, 2],
  [3, 4],
  [5, 6],
];

// Aqui temos um array de objetos, que serão abordados na próxima seção.
let pessoas = [
  { nome: "Ana", idade: 21 },
  { nome: "Bruno", idade: 19 },
];
```

Para acessar um elemento, usa-se o índice entre colchetes. Lembre que o primeiro elemento está no índice 0:

```javascript
let frutas = ["maçã", "banana", "uva"];

frutas[0]; // "maçã"
frutas[2]; // "uva"
frutas[5]; // undefined — índice inexistente não gera erro
```

Para acessar elementos em uma matriz, usamos a mesma notação de colchetes duas vezes — o primeiro índice seleciona o array interno, e o segundo seleciona o elemento dentro dele:

```javascript
let matriz = [
  [1, 2],
  [3, 4],
  [5, 6],
];

matriz[0][1]; // índice 1 do array no índice 0: 2
matriz[2][0]; // índice 0 do array no índice 2: 5
```

Para modificar um elemento, basta atribuir um novo valor à posição:

```javascript
frutas[1] = "manga";
console.log(frutas); // ["maçã", "manga", "uva"]
```

### Propriedades e métodos essenciais

#### `length`

Retorna o número de elementos do array:

```javascript
let numeros = [10, 20, 30];
numeros.length; // 3
```

É útil quando precisamos percorrer o array sem saber quantos elementos ele tem, ou para acessar o último elemento:

```javascript
numeros[numeros.length - 1]; // 30
```

#### `push`, `pop`, `shift` e `unshift`

Esses quatro métodos adicionam ou removem elementos nas extremidades do array. Todos modificam o array original.

`push` adiciona um ou mais elementos ao final; `pop` remove o último e o retorna:

```javascript
let nomes = ["Lara", "Vitória"];

nomes.push("Eliel"); // ["Lara", "Vitória", "Eliel"]
let removido = nomes.pop(); // removido = "Eliel", nomes = ["Lara", "Vitória"]
```

`unshift` adiciona ao início; `shift` remove o primeiro e o retorna:

```javascript
nomes.unshift("Michel"); // ["Michel", "Lara", "Vitória"]
let primeiro = nomes.shift(); // primeiro = "Michel", nomes = ["Lara", "Vitória"]
```

#### `indexOf` e `includes`

`indexOf` retorna o índice da primeira ocorrência de um valor, ou `-1` se não encontrar:

```javascript
let cores = ["azul", "verde", "azul"];

cores.indexOf("verde"); // 1
cores.indexOf("roxo"); // -1
```

`includes` responde apenas se o valor existe, com `true` ou `false`:

```javascript
cores.includes("azul"); // true
cores.includes("roxo"); // false
```

Use `indexOf` quando precisar saber _onde_ o elemento está; use `includes` quando precisar saber apenas _se_ ele está.

#### `slice` e `splice`

`slice(início, fim)` retorna uma cópia de um trecho do array, sem modificar o original. O índice de fim não é incluído:

```javascript
let letras = ["a", "b", "c", "d", "e"];

letras.slice(1, 4); // ["b", "c", "d"]
letras.slice(2); // ["c", "d", "e"] — até o fim
```

`splice(índice, quantidade, ...itens)` modifica o array original: remove elementos a partir de um índice e, opcionalmente, insere novos no lugar:

```javascript
let letras = ["a", "b", "c", "d"];

letras.splice(1, 2); // remove 2 a partir do índice 1 → letras = ["a", "d"]
letras.splice(1, 0, "x", "y"); // insere sem remover → letras = ["a", "x", "y", "d"]
letras.splice(2, 1, "z"); // remove 1 e insere "z" → letras = ["a", "x", "z", "d"]
```

> `slice` não altera o array original; `splice` altera. Essa distinção importa sempre que o array original precisar ser preservado.

### Iterando sobre arrays

A forma mais direta de percorrer um array é com o `for` tradicional, usando o índice:

```javascript
let numeros = [10, 20, 30];

for (let i = 0; i < numeros.length; i++) {
  console.log(numeros[i]);
}
```

O `for...of` é uma alternativa mais legível quando o índice não é necessário:

```javascript
for (let numero of numeros) {
  console.log(numero);
}
```

Use `for` quando precisar do índice durante a iteração; use `for...of` quando só os valores importam.

### Métodos de ordem superior

Os métodos a seguir recebem uma função como argumento — o que os torna muito expressivos para trabalhar com coleções. Cada um percorre o array e aplica a função a cada elemento, mas com propósitos diferentes.

#### `forEach`

Executa uma função para cada elemento, sem retornar nada. Substitui o `for` quando o objetivo é apenas produzir um efeito colateral, como imprimir valores:

```javascript
let nomes = ["Iago", "Kezya", "Otávio"];

nomes.forEach((nome) => console.log(nome));
```

#### `map`

Transforma cada elemento do array e retorna um **novo array** com os resultados. O array original não é modificado:

```javascript
let numeros = [1, 2, 3, 4];

let dobrados = numeros.map((n) => n * 2);
// dobrados = [2, 4, 6, 8]
```

#### `filter`

Retorna um **novo array** contendo apenas os elementos para os quais a função retornou `true`:

```javascript
let numeros = [1, 2, 3, 4, 5, 6];

let pares = numeros.filter((n) => n % 2 === 0);
// pares = [2, 4, 6]
```

#### `reduce`

Percorre o array acumulando um único resultado. Recebe uma função com dois parâmetros — o acumulador e o elemento atual — e um valor inicial para o acumulador:

```javascript
let numeros = [1, 2, 3, 4];

let soma = numeros.reduce((acumulador, n) => acumulador + n, 0);
// soma = 10
```

A cada iteração, o valor retornado pela função se torna o novo acumulador. É o método mais flexível dos quatro, mas também o que exige mais atenção para ser compreendido.

> `forEach` é para efeitos; `map` é para transformar; `filter` é para selecionar; `reduce` é para acumular. Ter essa distinção clara evita muito código desnecessário.

---

## Objetos

### O que é um objeto?

Um objeto é uma coleção de propriedades, onde cada propriedade associa um nome a um valor. O nome de uma propriedade é sempre uma string — chamada de chave — e o valor pode ser qualquer coisa: número, string, booleano, array, outro objeto ou até uma função.

Quando o valor de uma propriedade é uma função, ela passa a ser chamada de **método**. É exatamente assim que os métodos de arrays funcionam: `push`, `map`, `filter` são funções armazenadas como propriedades do objeto array.

As operações mais comuns sobre objetos são criá-los e definir, consultar, remover, testar e enumerar suas propriedades.

### Criando e acessando propriedades

A forma mais comum de criar um objeto é através de um literal, cuja sintaxe é um conjunto de pares chave-valor entre chaves:

```javascript
let produto = {
  nome: "Teclado",
  preco: 150,
  emEstoque: true,
};
```

Para acessar uma propriedade, há duas notações. A **notação de ponto** é a mais comum:

```javascript
produto.nome; // "Teclado"
produto.preco; // 150
```

A **notação de colchetes** recebe a chave como string. É menos usual no dia a dia, mas indispensável em alguns casos:

```javascript
produto["nome"]; // "Teclado"
produto["preco"]; // 150
```

A principal vantagem dos colchetes é que a chave pode ser uma variável, o que não é possível com a notação de ponto:

```javascript
let campo = "preco";
produto[campo]; // 150 — equivale a produto["preco"]
```

Acessar uma propriedade inexistente não gera erro — retorna `undefined`:

```javascript
produto.cor; // undefined
```

---

### Métodos

Como qualquer valor, uma função pode ser armazenada como propriedade de um objeto. Quando isso acontece, ela é chamada de método.

```javascript
let produto = {
  nome: "Teclado",
  preco: 150,
  desconto: function (percentual) {
    return produto.preco * (1 - percentual / 100);
  },
};

produto.desconto(10); // 135
```

Dentro de um método, é comum precisar acessar outras propriedades do mesmo objeto. Para isso, usa-se a palavra-chave `this`, que referencia o próprio objeto:

```javascript
let produto = {
  nome: "Teclado",
  preco: 150,
  desconto: function (percentual) {
    return this.preco * (1 - percentual / 100);
  },
};
```

O comportamento de `this` tem nuances que ficarão mais claras no estudo de orientação a objetos. Por ora, vale saber que dentro de um método, `this` é o objeto ao qual o método pertence.

### Adicionando, alterando e removendo propriedades

Objetos em JavaScript são dinâmicos: propriedades podem ser adicionadas, alteradas ou removidas após a criação.

Para adicionar ou alterar, basta atribuir um valor à chave e, se ela não existir, será criada; se existir, será sobrescrita:

```javascript
let estudante = { nome: "Ana" };

estudante.curso = "Informática"; // adiciona
estudante.nome = "Ana Lima"; // altera

console.log(estudante); // { nome: "Ana Lima", curso: "Informática" }
```

Para remover uma propriedade, usa-se o operador `delete`:

```javascript
delete estudante.curso;
console.log(estudante); // { nome: "Ana Lima" }
```

### Iterando sobre objetos

#### `for...in`

O `for...in` percorre as chaves de um objeto, uma por vez:

```javascript
let produto = { nome: "Teclado", preco: 150, emEstoque: true };

for (let chave in produto) {
  console.log(chave, produto[chave]);
}
// nome Teclado
// preco 150
// emEstoque true
```

Note o uso da notação de colchetes com a variável `chave` — aqui ela é indispensável, pois a chave só é conhecida em tempo de execução.

#### `Object.keys`, `Object.values` e `Object.entries`

Esses três métodos retornam arrays, o que permite combiná-los com `forEach`, `map`, `filter` e os demais métodos de ordem superior já vistos.

`Object.keys` retorna um array com as chaves:

```javascript
Object.keys(produto); // ["nome", "preco", "emEstoque"]
```

`Object.values` retorna um array com os valores:

```javascript
Object.values(produto); // ["Teclado", 150, true]
```

`Object.entries` retorna um array de pares `[chave, valor]`:

```javascript
Object.entries(produto);
// [["nome", "Teclado"], ["preco", 150], ["emEstoque", true]]
```

Um uso comum é iterar com `forEach` sobre as entradas:

```javascript
Object.entries(produto).forEach(([chave, valor]) => {
  console.log(`${chave}: ${valor}`);
});
// nome: Teclado
// preco: 150
// emEstoque: true
```

Use `for...in` para iterações simples sobre as chaves; use `Object.keys`, `Object.values` ou `Object.entries` quando precisar tratar os dados como array — filtrar, transformar ou reduzir.

## Arrays e objetos juntos

Na prática, arrays e objetos raramente aparecem isolados. A combinação dos dois é o que permite representar dados do mundo real com fidelidade.

### Arrays de objetos

Um array de objetos é a estrutura mais comum para representar uma coleção de entidades — uma lista de produtos, um conjunto de estudantes, um catálogo de filmes. Cada elemento do array é um objeto com as mesmas propriedades, mas valores diferentes.

```javascript
let estudantes = [
  { nome: "Ana", nota: 8.5 },
  { nome: "Bruno", nota: 6.0 },
  { nome: "Carlos", nota: 9.2 },
];
```

Para acessar um elemento, usa-se o índice; para acessar uma propriedade, usa-se a notação de ponto:

```javascript
estudantes[0].nome; // "Ana"
estudantes[2].nota; // 9.2
```

É aqui que os métodos de ordem superior mostram todo o seu valor. Com `map`, `filter` e `reduce`, é possível trabalhar com coleções de objetos de forma expressiva:

```javascript
// Extrai apenas os nomes dos objetos
let nomes = estudantes.map((e) => e.nome);
// ["Ana", "Bruno", "Carlos"]

// Filtra apenas quem foi aprovado
let aprovados = estudantes.filter((e) => e.nota >= 7);
// [{ nome: "Ana", nota: 8.5 }, { nome: "Carlos", nota: 9.2 }]

// Calcula a média da turma
let media = estudantes.reduce((acc, e) => acc + e.nota, 0) / estudantes.length;
// 7.9
```

### Objetos com arrays como propriedades

Um objeto também pode ter arrays como valores de propriedades. Isso é útil quando uma entidade possui uma lista de itens relacionados — os pedidos de um cliente, as matérias de um estudante, as tags de um produto.

```javascript
let estudante = {
  nome: "Ana",
  matricula: "2024001",
  materias: ["Algoritmos", "Banco de Dados", "Engenharia de Software"],
};
```

Para acessar e manipular o array, usa-se a notação normal — a propriedade retorna o array, e a partir daí todos os métodos já vistos estão disponíveis:

```javascript
estudante.materias[0]; // "Algoritmos"
estudante.materias.length; // 3
estudante.materias.push("POO"); // adiciona "POO" ao array
```

As duas estruturas também se combinam: um array de objetos onde cada objeto tem arrays como propriedades é perfeitamente válido e muito comum em dados vindos de APIs e bancos de dados.

```javascript
let turma = [
  { nome: "Ana", materias: ["Algoritmos", "BD"] },
  { nome: "Bruno", materias: ["Algoritmos", "POO"] },
];
```

## Atividades sugeridas

**(Questão 01)** Crie um objeto que represente um livro, com as propriedades `titulo`, `autor`, `ano` e `paginas`. Em seguida:

1. Acesse o título usando notação de ponto.
2. Acesse o autor usando notação de colchetes.
3. Adicione uma propriedade `disponivel` com valor `true`.
4. Altere o ano para o ano atual.
5. Remova a propriedade `paginas`.

**(Questão 02)** Crie um objeto `contaBancaria` com as propriedades `titular`, `saldo` e dois métodos: `depositar(valor)` e `sacar(valor)`. O método `sacar` deve verificar se há saldo suficiente antes de realizar a operação e exibir uma mensagem adequada em cada caso.

**(Questão 03)** Dado o objeto abaixo, use `for...in` para imprimir cada propriedade no formato `"chave: valor"`. Em seguida, faça o mesmo usando `Object.entries` e `forEach`.

```javascript
let produto = {
  nome: "Monitor",
  marca: "Dell",
  preco: 1200,
  polegadas: 27,
};
```

**(Questão 04)** Dado o array abaixo:

```javascript
let produtos = [
  { nome: "Teclado", preco: 150, categoria: "Periférico" },
  { nome: "Monitor", preco: 1200, categoria: "Vídeo" },
  { nome: "Mouse", preco: 80, categoria: "Periférico" },
  { nome: "Headset", preco: 300, categoria: "Periférico" },
];
```

a. Crie um array com apenas os nomes dos produtos.\
b. Filtre apenas os produtos da categoria `"Periférico"`.\
c. Calcule o valor total de todos os produtos.\
d. Encontre o produto mais caro.

**(Questão 05)** Crie um objeto `playlist` com as propriedades `nome` e `musicas` (um array de strings). Em seguida:

a. Adicione uma música ao final da playlist.\
b. Remova a primeira música.\
c. Verifique se uma música específica está na playlist.\
d. Exiba todas as músicas numeradas, começando em 1.

**(Questão 06)** Responda com suas palavras:

a. Qual a diferença entre acessar uma propriedade com notação de ponto e com notação de colchetes? Em que situação os colchetes são indispensáveis?\
b. O que é um método? Em que sentido ele é diferente de uma função comum?\
c. Por que arrays de objetos são uma estrutura tão comum na programação? Que tipo de dado eles representam bem?
