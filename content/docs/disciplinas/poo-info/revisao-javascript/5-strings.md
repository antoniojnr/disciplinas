---
title: "5. Strings"
weight: 5
bookHidden: false
---

# 5. Strings

Frequentemente precisamos manipular dados textuais nos sistemas que escrevemos. Afinal, texto está em todo lugar: o usuário digita texto ao inserir informações em um programa, APIs comunicam-se através de formatos textuais e arquivos são texto (em sua maioria).

A facilidade em realizar operações sobre dados textuais se dá graças ao tipo `string`. Os métodos e atributos do tipo `string` permitem realizar operações como calcular o tamanho, localizar a presença de uma palavra ou remover parte de um texto.

---

## O que é uma string?

Strings são sequências de caracteres que representam texto. Os caracteres são codificados em Unicode, o padrão universal para representar texto em todas as linguagens. Assim como em arrays, cada caractere pode ser acessado pelo seu índice, que começa em 0.

Uma característica importante das strings em JavaScript é que elas são **imutáveis**: uma vez criada, uma string não pode ser alterada. Os métodos de string nunca modificam a string original — sempre retornam uma nova. Por isso, é necessário guardar o resultado em uma variável para aproveitá-lo:

```javascript
let nome = "ana lima";
nome.toUpperCase(); // "ANA LIMA" — mas nome continua "ana lima"

let nomeFormatado = nome.toUpperCase(); // agora sim
```

Strings podem ser declaradas com aspas simples, aspas duplas ou _backticks_ (``). As três formas são equivalentes para strings simples — a escolha é geralmente uma questão de convenção:

```javascript
let a = "Olá";
let b = "Olá";
let c = `Olá`;
```

Os _backticks_ têm um comportamento especial e são usados para criar _template literals_, que veremos mais adiante.

---

## Acessando caracteres

Assim como em arrays, é possível acessar um caractere pelo índice usando colchetes:

```javascript
let palavra = "texto";

palavra[0]; // "t"
palavra[4]; // "o"
palavra[9]; // undefined — índice inexistente não gera erro
```

Outra forma é usar o método `charAt`, que se comporta de forma equivalente:

```javascript
palavra.charAt(0); // "t"
```

A diferença é que `charAt` retorna uma string vazia `""` para índices inválidos, enquanto a notação de colchetes retorna `undefined`. Na prática, a notação de colchetes é mais comum por ser mais concisa e consistente com o que já conhecemos de arrays.

---

## Propriedades e métodos essenciais

### `length`

Retorna o número de caracteres da string:

```javascript
let texto = "JavaScript";
texto.length; // 10
```

Espaços e caracteres especiais contam:

```javascript
"olá mundo".length; // 9
"".length; // 0
```

---

### `toUpperCase` e `toLowerCase`

Retornam uma nova string com todos os caracteres convertidos para maiúsculas ou minúsculas, respectivamente:

```javascript
let nome = "Ana Lima";

nome.toUpperCase(); // "ANA LIMA"
nome.toLowerCase(); // "ana lima"
```

São úteis para comparações sem distinção entre maiúsculas e minúsculas:

```javascript
let entrada = "JavaScript";
entrada.toLowerCase() === "javascript"; // true
```

---

### `trim`, `trimStart` e `trimEnd`

Removem espaços em branco das extremidades da string. `trim` remove dos dois lados; `trimStart` remove apenas do início; `trimEnd` remove apenas do fim:

```javascript
let entrada = "   olá mundo   ";

entrada.trim(); // "olá mundo"
entrada.trimStart(); // "olá mundo   "
entrada.trimEnd(); // "   olá mundo"
```

São especialmente úteis para limpar entradas do usuário antes de processá-las.

---

### `includes`, `startsWith` e `endsWith`

Os três verificam a presença de uma substring e retornam `true` ou `false`.

`includes` verifica se a substring aparece em qualquer posição:

```javascript
let frase = "JavaScript é uma linguagem versátil";

frase.includes("linguagem"); // true
frase.includes("Python"); // false
```

`startsWith` verifica se a string começa com a substring:

```javascript
frase.startsWith("JavaScript"); // true
frase.startsWith("linguagem"); // false
```

`endsWith` verifica se a string termina com a substring:

```javascript
frase.endsWith("versátil"); // true
frase.endsWith("Java"); // false
```

---

### `indexOf`

Retorna o índice da primeira ocorrência de uma substring, ou `-1` se não for encontrada:

```javascript
let frase = "um dois um três";

frase.indexOf("um"); // 0  — primeira ocorrência
frase.indexOf("dois"); // 3
frase.indexOf("cinco"); // -1
```

O comportamento é idêntico ao `indexOf` de arrays, mas aplicado a substrings em vez de elementos. Quando apenas a presença importa, `includes` é mais legível; use `indexOf` quando precisar saber a posição.

---

### `slice`

Retorna um trecho da string entre dois índices. O índice de fim não é incluído:

```javascript
let texto = "JavaScript";

texto.slice(0, 4); // "Java"
texto.slice(4); // "Script" — até o fim
texto.slice(-6); // "Script" — índices negativos contam do fim
```

Assim como em arrays, `slice` não altera a string original.

---

### `replace` e `replaceAll`

`replace` substitui a primeira ocorrência de uma substring por outra:

```javascript
let frase = "um dois um três";

frase.replace("um", "zero"); // "zero dois um três" — só a primeira
```

`replaceAll` substitui todas as ocorrências:

```javascript
frase.replaceAll("um", "zero"); // "zero dois zero três"
```

Lembre que strings são imutáveis — `frase` não é alterada em nenhum dos casos. O resultado precisa ser guardado em uma variável.

---

### `split`

Divide a string em um array de substrings, usando um separador como critério:

```javascript
let frase = "um,dois,três";

frase.split(","); // ["um", "dois", "três"]
```

Se o separador for uma string vazia, cada caractere vira um elemento:

```javascript
"abc".split(""); // ["a", "b", "c"]
```

`split` é o inverso de `join` (método de arrays) e a combinação dos dois é muito usada para transformar e remontar strings:

```javascript
"ana lima"
  .split(" ")
  .map((palavra) => palavra[0].toUpperCase() + palavra.slice(1))
  .join(" ");
// "Ana Lima"
```

---

### `padStart` e `padEnd`

Preenchem a string com um caractere até atingir um comprimento mínimo. `padStart` preenche no início; `padEnd` preenche no fim:

```javascript
"5".padStart(3, "0"); // "005"
"5".padEnd(3, "0"); // "500"
```

São úteis para formatar saídas com alinhamento fixo, como exibir horas (`"9".padStart(2, "0")` → `"09"`) ou gerar códigos com comprimento padronizado.

---

## Template literals

Template literals são uma forma alternativa de declarar strings, usando backticks em vez de aspas. Eles têm duas vantagens principais sobre as strings convencionais.

A primeira é a **interpolação**: é possível inserir expressões JavaScript diretamente no texto usando `${}`, sem precisar concatenar com `+`:

```javascript
let nome = "Ana";
let idade = 21;

// com concatenação
"Olá, " + nome + ". Você tem " + idade + " anos.";

// com template literal
`Olá, ${nome}. Você tem ${idade} anos.`;
```

Qualquer expressão válida pode ir dentro de `${}` — variáveis, operações, chamadas de função:

```javascript
let preco = 150;
`Preço com desconto: R$ ${(preco * 0.9).toFixed(2)}`; // "Preço com desconto: R$ 135.00"
```

A segunda vantagem é o suporte a **múltiplas linhas** sem nenhum caractere especial:

```javascript
let mensagem = `Olá, ${nome}.
Seu pedido foi confirmado.
Obrigado pela preferência.`;
```

Em strings convencionais, quebras de linha exigiriam o uso de `\n`. Com template literals, o texto pode ser escrito diretamente como aparece.

---

## Atividades sugeridas

**(Questão 01)** Dada a string `"JavaScript"`:

a. Acesse o primeiro e o último caractere.\
b. Verifique quantos caracteres ela tem.\
c. Extraia apenas `"Script"` usando `slice`.

---

**(Questão 02)** Escreva uma função `normalizar(texto)` que receba uma string com possíveis espaços nas bordas e letras em caixa mista, e retorne a string sem espaços nas extremidades e com todas as letras em minúsculas.

```javascript
normalizar("  JavaScript  "); // "javascript"
normalizar("  ANA LIMA "); // "ana lima"
```

---

**(Questão 03)** Escreva uma função `validarEmail(email)` que retorne `true` se a string contiver `"@"` e terminar com `".com"`, e `false` caso contrário.

```javascript
validarEmail("ana@email.com"); // true
validarEmail("ana@email.org"); // false
validarEmail("anaemail.com"); // false
```

---

**(Questão 04)** Dado o objeto abaixo, use um template literal para gerar a mensagem indicada:

```javascript
let pedido = {
  cliente: "Bruno",
  produto: "Teclado",
  preco: 150,
  prazo: 3,
};

// Mensagem esperada:
// "Olá, Bruno! Seu pedido de Teclado (R$ 150.00) chegará em 3 dias úteis."
```

---

**(Questão 05)** Escreva uma função `capitalizar(frase)` que receba uma frase em letras minúsculas e retorne a frase com a primeira letra de cada palavra em maiúscula.

```javascript
capitalizar("olá mundo"); // "Olá Mundo"
capitalizar("javascript é legal"); // "Javascript É Legal"
```

---

**(Questão 06)** Responda com suas palavras:

a. O que significa dizer que strings são imutáveis? Como isso afeta a forma de trabalhar com métodos como `replace` e `toUpperCase`?\
b. Qual a diferença entre `includes` e `indexOf`? Em que situação você preferiria um ou outro?\
c. Por que `split` e `join` são considerados complementares?
