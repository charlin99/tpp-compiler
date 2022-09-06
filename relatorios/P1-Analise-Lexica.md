# Relatório da análise léxica da linguagem TPP
**Aluno**: Carlos Miguel dos Santos Filho¹

¹Bacharelado em Ciência da Computação – Universidade Tecnológica Federal do Paraná (UTFPR)

**Abstract.** There are four main steps when it comes to developing a compiler. Lexical analysis, syntactic analysis, semantic analysis and code generation. In this first part of the assignemnt, the development of lexical analysis will be reported using finite automata and regular expressions to associate words into tokens accepted by the desired programming language.

**Resumo.** Existem quatro etapas principais quando tratamos do desenvolvimento de um compilador. A análise léxica, a análise sintática, a análise semântica e a geração de código. Nessa primeira parte do trabalho, será relatado o desenvolvimento da análise léxica com a utilização de autômatos finitos e expressões regulares para associar palavras em tokens aceitos pela linguagem de programação desejada.

## 1. Introdução
A análise léxica é o processo de converter uma sequência de caracteres em uma sequência de tokens léxicos equivalente. Estudaremos nesse trabalho sobre como é feita a sua implementação utilizando Python e a ferramenta PLY para montar um scanner que realizará a análise léxica, com a utilização de autômatos finitos e expressões regulares, assim como entender também um pouco sobre as especificações da  linguagem a ser implementada, o T++.

## 2. Especificação da linguagem
A linguagem T++/TPP precisa que os tipos de suas variáveis sejam especificados explicitamente em sua declaração, e suporta dois tipos básicos de dados, o tipo inteiro e o tipo flutuante.
```
inteiro: n
flutuante: m
```
**Código 1**: Exemplo de tipos suportados.

Também é possível utilizar arranjos uni e bidimensionais, ou seja, vetores e matrizes.
```
inteiro: vetor[5]
inteiro: matriz[3][2]
```
**Código 2**: Exemplo de arranjos.

O tipo de uma função pode ser omitido, e quando é feito assim, irá virar um procedimento, que é quando o seu tipo é um void, não esperando nenhum retorno. É uma linguagem quase fortemente tipada, nem todos os erros são especificados porẽm sempre devem ocorrer avisos.

Possui suporte a:
- Atribuição (:=).
- Operadores aritméticos de adição (+), subtração (-), multiplicação (\*) e divisão (/).
- Operadores lógicos e (&&), ou (||) e operador de negação (!).
- Operadores relacionais menor que (<), menor ou igual (<=), maior que (>), maior ou igual (>=) igualdade (=) e diferente (<>).
- 
## 3. Detalhes da implementação
Foram utilizadas para a implementação da varredura a linguagem Python e a ferramenta PLY, que é uma reimplementação do Lex e do Yacc para a linguagem Python.

Através do PLY definimos uma lista de tokens a serem reconhecidos na análise léxica.
```
tokens = [
    "ID",  # identificador
    # numerais
    "NUM_NOTACAO_CIENTIFICA",  # ponto flutuante em notaçao científica
    "NUM_PONTO_FLUTUANTE",  # ponto flutuate
    "NUM_INTEIRO",  # inteiro
    # operadores binarios
    "MAIS",  # +
    "MENOS",  # -
    "MULTIPLICACAO",  # *
    "DIVISAO",  # /
    "E_LOGICO",  # &&
    "OU_LOGICO",  # ||
    "DIFERENCA",  # <>
    "MENOR_IGUAL",  # <=
    "MAIOR_IGUAL",  # >=
    "MENOR",  # <
    "MAIOR",  # >
    "IGUAL",  # =
    # operadores unarios
    "NEGACAO",  # !
    # simbolos
    "ABRE_PARENTESE",  # (
    "FECHA_PARENTESE",  # )
    "ABRE_COLCHETE",  # [
    "FECHA_COLCHETE",  # ]
    "VIRGULA",  # ,
    "DOIS_PONTOS",  # :
    "ATRIBUICAO",  # :=
    # 'COMENTARIO', # {***}
]
```
**Código 3**: Lista contendo os tokens da linguagem T++.

E também é possível definir as palavras reservadas na linguagem, que também serão transformadas em tokens junto aos anteriores, vistos no código 3.
```
reserved_words = {
    "se": "SE",
    "então": "ENTAO",
    "senão": "SENAO",
    "fim": "FIM",
    "repita": "REPITA",
    "flutuante": "FLUTUANTE",
    "retorna": "RETORNA",
    "até": "ATE",
    "leia": "LEIA",
    "escreva": "ESCREVA",
    "inteiro": "INTEIRO",
}
]
```
**Código 4**: Lista contendo as palavras reservadas da linguagem T++.

Definimos as expressões regulares a serem reconhecidas nos códigos da linguagem.
```
digito = r"([0-9])"
letra = r"([a-zA-ZáÁãÃàÀéÉíÍóÓõÕ])"
sinal = r"([\-\+]?)"

id = (
    r"(" + letra + r"(" + digito + r"+|_|" + letra + r")*)"
)

inteiro = r"\d+"

flutuante = (
    r'\d+[eE][-+]?\d+|(\.\d+|\d+\.\d*)([eE][-+]?\d+)?'
)

notacao_cientifica = (
    r"(" + sinal + r"([1-9])\." + digito + r"+[eE]" + sinal + digito + r"+)"
)
```
**Código 5**: Expressões regulares definidas, com excessão das que definem tokens simples (símbolos, operadores lógicos e relacionais).

A partir das expressões regulares, podemos montar autômatos finitos que representam como funcionam as classes dos tokens.
## 5. Exemplos de saída do sistema de varredura para exemplos de entrada
Para a realização do exemplo, será utilizado o arquivo teste.tpp disponibilizado pelo professor da disciplina, que podersá ser visto logo abaixo no código 3.
```
inteiro: a[10]
flutuante: b

inteiro func1(inteiro:x, flutuante:y)
  inteiro: res
  se (x > y) então
    res := x + y
  senão
    res := x * y
  fim
  retorna(res)
fim

func2(inteiro:z, flutuante:w)
  a := z
  b := w
fim

inteiro principal()
  inteiro: x,y
  flutuante: w
  a := 10 + 2
  leia(x)
  leia(w)
  w := .6 + 1.
  func2(1, 2.5)
  b := func1(x,w)
  escreva(b)
  retorna(0)
fim
```
**Código 5**: Código fonte do arquivo teste.tpp

A partir do código fonte escrito na linguagem T++, será gerado uma lista sequencial dos tokens que representam cada elemento identificado no código, que poderá ser visto logo abaixo no código 4.

```
INTEIRO
DOIS_PONTOS
ID
ABRE_COLCHETE
NUM_INTEIRO
FECHA_COLCHETE
FLUTUANTE
DOIS_PONTOS
ID
INTEIRO
ID
ABRE_PARENTESE
INTEIRO
DOIS_PONTOS
ID
VIRGULA
FLUTUANTE
DOIS_PONTOS
ID
FECHA_PARENTESE
INTEIRO
DOIS_PONTOS
ID
SE
ABRE_PARENTESE
ID
MAIOR
ID
FECHA_PARENTESE
ENTAO
ID
ATRIBUICAO
ID
MAIS
ID
SENAO
ID
ATRIBUICAO
ID
MULTIPLICACAO
ID
FIM
RETORNA
ABRE_PARENTESE
ID
FECHA_PARENTESE
FIM
ID
ABRE_PARENTESE
INTEIRO
DOIS_PONTOS
ID
VIRGULA
FLUTUANTE
DOIS_PONTOS
ID
FECHA_PARENTESE
ID
ATRIBUICAO
ID
ID
ATRIBUICAO
ID
FIM
INTEIRO
ID
ABRE_PARENTESE
FECHA_PARENTESE
INTEIRO
DOIS_PONTOS
ID
VIRGULA
ID
FLUTUANTE
DOIS_PONTOS
ID
ID
ATRIBUICAO
NUM_INTEIRO
MAIS
NUM_INTEIRO
LEIA
ABRE_PARENTESE
ID
FECHA_PARENTESE
LEIA
ABRE_PARENTESE
ID
FECHA_PARENTESE
ID
ATRIBUICAO
NUM_PONTO_FLUTUANTE
MAIS
NUM_PONTO_FLUTUANTE
ID
ABRE_PARENTESE
NUM_INTEIRO
VIRGULA
NUM_PONTO_FLUTUANTE
FECHA_PARENTESE
ID
ATRIBUICAO
ID
ABRE_PARENTESE
ID
VIRGULA
ID
FECHA_PARENTESE
ESCREVA
ABRE_PARENTESE
ID
FECHA_PARENTESE
RETORNA
ABRE_PARENTESE
NUM_INTEIRO
FECHA_PARENTESE
```
**Código 6**: Saída de tokens gerados para o código fonte do código 3.

## 6. Conclusão
O PLY é uma ferramenta que auxilia e facilita muito na construção de um compilador, deixando com que apenas tenhamos que definir as palavras a serem utilizadas na linguagem.

Foi possível realizar a construção da análise léxica sem problemas algum, podendo passar com sucesso em todo e qualquer teste realizado, tornando possível iniciar o desenvolvimento da fase de análise sintática do compilador.

## 7. Referências
**LOUDEN, Kenneth C. Compiladores: princípios e práticas:** Cengage Learning Brasil, 2004. E-book. 9788522128532. Disponível em: https://integrada.minhabiblioteca.com.br/#/books/9788522128532/. Acesso em: 05 set. 2022.

**Wikipedia contributors. (2022, August 16).** Lexical analysis. Wikipedia. Retrieved September 5, 2022, from https://en.wikipedia.org/wiki/Lexical_analysis
