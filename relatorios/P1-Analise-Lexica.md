# Relatório da análise léxica da linguagem TPP
**Aluno**: Carlos Miguel dos Santos Filho¹

¹Bacharelado em Ciência da Computação – Universidade Tecnológica Federal do Paraná (UTFPR)

**Abstract.**

**Resumo.**

## Especificação da linguagem
A linguagem T++/TPP precisa que os tipos de suas variáveis sejam especificados explicitamente em sua declaração, e suporta dois tipos básicos de dados, o tipo inteiro e o tipo flutuante.
```
inteiro: n
flutuante: m
```
Também é possível utilizar arranjos uni e bidimensionais, ou seja, vetores e matrizes.
```
inteiro: vetor[5]
inteiro: matriz[3][2]
```
O tipo de uma função pode ser omitido, e quando é feito assim, irá virar um procedimento, que é quando o seu tipo é um void, não esperando nenhum retorno. É uma linguagem quase fortemente tipada, nem todos os erros são especificados porẽm sempre devem ocorrer avisos.

Possui suporte a:
- Atribuição (:=).
- Operadores aritméticos de adição (+), subtração (-), multiplicação (\*) e divisão (/).
- Operadores lógicos e (&&), ou (||) e operador de negação (!).
- Operadores relacionais menor que (<), menor ou igual (<=), maior que (>), maior ou igual (>=) igualdade (=) e diferente (<>).
## Especificação dos autômatos para a formação de cada classe de token da linguagem
## Detalhes da implementação da varredura na LP e ferramentas
Foram utilizadas para a implementação da varredura a linguagem Python e a ferramenta PLY, que é uma reimplementação do Lex e do Yacc para a linguagem Python.
## Exemplos de saída do sistema de varredura (lista de tokens) para exemplos de entrada (código fonte)
