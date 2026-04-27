---
title: "Lista 07"
weight: 1
bookCollapseSection: true
#bookHidden: true
---

# Lista 07

## Instruções

- Cada questão deve ser implementada no pacote indicado (`lista07.q1`, `lista07.q2`, etc.).
- Todos os exercícios devem ser feitos em um único projeto do IntelliJ.
- Os nomes de classes, métodos e pacotes devem ser **exatamente** os especificados — os testes automatizados dependem disso.
- Todos os exercícios serão corrigidos por execução de testes JUnit 4 fornecidos junto com esta lista.
- Não há casos de teste para esta lista.

## Exercícios

**Questão 01.** O código abaixo faz parte de um sistema de automação fiscal. O método `processar` centraliza o cálculo de imposto e a geração de XML de transmissão para todos os tipos de documento:

```java
public class ProcessadorFiscal {

    public String processar(String tipo, double valor, String identificador) {
        double imposto;
        String xml;

        switch (tipo) {
            case "NFE":
                imposto = valor * 0.18;
                xml = "<nfe>" + identificador + "</nfe>";
                break;
            case "CTE":
                imposto = valor * 0.12;
                xml = "<cte>" + identificador + "</cte>";
                break;
            case "NFSE":
                imposto = valor * 0.05;
                xml = "<nfse>" + identificador + "</nfse>";
                break;
            default:
                throw new IllegalArgumentException("Tipo desconhecido: " + tipo);
        }

        return xml + "|imposto:" + String.format("%.2f", imposto);
    }
}
```

Você recebeu a demanda de adicionar um quarto tipo de documento (`MDF-e`, com alíquota de 8% e XML no formato `<mdfe>...</mdfe>`). Isso exige modificar o método existente, que já está em produção e testado.

Sua tarefa é refatorar o sistema para que a adição de novos tipos de documento não exija alteração em nenhuma classe existente. O método `processar` do `ProcessadorFiscal` deve funcionar com qualquer tipo de documento, presente ou futuro, recebendo-o como parâmetro. Implemente os quatro tipos de documento (NF-e, CT-e, NFS-e e MDF-e).

**Restrições:**

- Crie o código no pacote `lista07.q1`
- O `ProcessadorFiscal` não pode conhecer os tipos concretos de documento.
- Nenhum `switch`, `if-else` por tipo ou `instanceof` deve existir no código cliente.
- Cada tipo de documento deve saber calcular seu próprio imposto e gerar seu próprio XML.

---

**Questão 02.** Uma equipe modelou um serviço de armazenamento de arquivos com a seguinte hierarquia:

```java
public class ArmazenamentoNuvem {
    private Map<String, byte[]> storage = new HashMap<>();

    public void gravar(String caminho, byte[] dados) {
        storage.put(caminho, dados);
    }

    public byte[] ler(String caminho) {
        return storage.get(caminho);
    }
}
```

Para ganhar performance, um desenvolvedor criou uma subclasse que faz cache dos dados localmente:

```java
public class ArmazenamentoComCache extends ArmazenamentoNuvem {
    private Map<String, byte[]> cache = new HashMap<>();

    @Override
    public void gravar(String caminho, byte[] dados) {
        cache.put(caminho, dados);
        // não chama super.gravar() — dados ficam apenas no cache
    }

    @Override
    public byte[] ler(String caminho) {
        return cache.getOrDefault(caminho, null);
    }
}
```

Outro módulo do sistema realiza backup copiando arquivos de um armazenamento de origem para um de destino:

```java
public void realizarBackup(ArmazenamentoNuvem origem,
                           ArmazenamentoNuvem destino,
                           List<String> caminhos) {
    for (String c : caminhos) {
        byte[] dados = origem.ler(c);
        if (dados != null) {
            destino.gravar(c, dados);
        }
    }
}
```

Quando a equipe passou a usar `ArmazenamentoComCache` como destino do backup, os dados pararam de chegar na nuvem. Quando usaram como origem, dados gravados por outros nós do sistema não eram encontrados.

**Tarefa:**

1. Explique, em comentários no código ou em um bloco de texto no início do arquivo, qual princípio de orientação a objetos está sendo violado e por quê. Demonstre o cenário de falha com um trecho de código no `main`.
2. Proponha e implemente uma nova modelagem que resolva o problema. O cache deve continuar existindo como funcionalidade, mas a gravação na nuvem não pode ser comprometida. O método `realizarBackup` deve funcionar corretamente com qualquer combinação de implementações, sem distinção de tipo.

**Restrições:**

- Crie o código no pacote `lista07.q2`
- A solução não deve usar herança entre `ArmazenamentoComCache` e `ArmazenamentoNuvem` (a relação atual é a causa do problema).
- O método `realizarBackup` não pode usar `instanceof` nem condicional por tipo.

---

**Questão 03.** Um e-commerce precisa de um motor de descontos para o carrinho de compras. A equipe de marketing frequentemente solicita novas regras promocionais, então o sistema precisa ser extensível sem alterar o motor existente.

O carrinho contém itens (cada um com nome, preço unitário e quantidade) e um valor de frete. O motor recebe o carrinho e uma lista de regras, e deve selecionar e retornar a regra que produz o maior desconto entre as aplicáveis. Se nenhuma regra for aplicável, retorna `null`.

Você deve modelar e implementr o sistema completo. Crie as classes para representar o carrinho e seus itens, defina o contrato que toda regra de desconto deve seguir, e implemente o motor e as três regras abaixo:

| Regra                     | Condição de ativação                                                                 | Cálculo do desconto                       |
| ------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------- |
| Desconto por valor mínimo | Subtotal do carrinho estritamente maior que R$ 300                                   | 10% do subtotal                           |
| Desconto segunda unidade  | 2 ou mais itens distintos no carrinho (linhas distintas, independente de quantidade) | 50% do preço unitário do item mais barato |
| Frete grátis              | Soma das quantidades de todos os itens maior que 5                                   | Valor integral do frete                   |

Crie um `main` que monte um carrinho com pelo menos 4 itens, instancie as 3 regras, execute o motor e imprima a descrição da regra escolhida e o valor do desconto.

**Restrições:**

- Crie o código no pacote `lista07.q3`
- O motor de descontos não pode conhecer os tipos concretos de regra — deve tratar todas de forma uniforme.
- Adicionar uma nova regra no futuro não deve exigir alteração em nenhuma classe existente.
- Nenhum `instanceof` ou `switch` por tipo deve existir no motor nem no `main`.

---

**Questão 04.** O código abaixo implementa um sistema de log com dois formatadores. A intenção é que `LogFormatterSeguro`, ao ser usado via uma variável do tipo `LogFormatter`, produza mensagens com o prefixo `[SEC]`, timestamp em UTC e a marcação `[AUDITORIA]`:

```java
public class LogFormatter {
    public String prefixo = "[INFO]";

    public static String timestamp() {
        return LocalDateTime.now()
            .format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
    }

    public String formatar(String mensagem) {
        return prefixo + " " + timestamp() + " " + mensagem;
    }
}

public class LogFormatterSeguro extends LogFormatter {
    public String prefixo = "[SEC]";

    public static String timestamp() {
        return Instant.now().toString();
    }

    @Override
    public String formatar(String mensagem) {
        return prefixo + " " + timestamp() + " [AUDITORIA] " + mensagem;
    }
}
```

Porém, ao executar o código abaixo, o resultado não é o esperado:

```java
LogFormatter log = new LogFormatterSeguro();
System.out.println(log.prefixo);                  // imprime [INFO], não [SEC]
System.out.println(LogFormatter.timestamp());      // imprime timestamp local, não UTC
System.out.println(log.formatar("Acesso negado")); // formatar() funciona, mas depende
                                                   // de como prefixo e timestamp()
                                                   // são resolvidos internamente
```

**Tarefa:**

1. No início do arquivo, explique em comentários por que `log.prefixo` imprime `[INFO]` e por que `LogFormatter.timestamp()` não invoca a versão da subclasse. Identifique os dois mecanismos de resolução envolvidos (um para atributos, outro para métodos estáticos).
2. Reescreva a hierarquia corrigindo os dois problemas, de modo que o polimorfismo funcione corretamente em todos os casos. Adicione uma terceira subclasse `LogFormatterErro` (prefixo `[ERROR]`, timestamp padrão herdado, marcação `[ERRO CRÍTICO]` na mensagem).
3. Crie uma classe `SistemaLog` com um método que receba uma lista de formatadores e uma mensagem, e retorne uma lista com o resultado formatado de cada um — demonstrando que o tipo dinâmico é quem determina o comportamento.
4. No `main`, demonstre o funcionamento criando instâncias dos três formatadores, armazenando-os em uma lista tipada pela superclasse, e imprimindo os resultados via `SistemaLog`.

**Restrições:**

- Nenhum método da hierarquia pode ser `static`.
- Nenhum atributo de instância público deve ser usado para definir o prefixo — use um método sobrescrevível.
- Todos os métodos sobrescritos devem usar `@Override`.
