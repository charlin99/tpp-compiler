# Relatório da análise sintática da linguagem TPP
**Aluno**: Carlos Miguel dos Santos Filho¹

¹Bacharelado em Ciência da Computação – Universidade Tecnológica Federal do Paraná (UTFPR)

**Abstract.**

**Resumo.**

## 1. Introdução
Após a realização da análise léxica, prosseguimos para a análise sintática da linguagem. Essa etapa consiste em verificar a estrutura gramatical do código, para ver se está de acordo com os tokens gerados na análise léxica e respeita a gramática estabelecida.

A análise sintática converte o código no arquivo texto em uma estrutura de dados que se assemelha a uma árvore com várias ramificações que representam as diferentes regras gramaticais, e o código fonte fica pendurado nas folhas da árvore. Conforme você sobe na árvore, mais generalizadas vão ficando as regras, a ponto de você chegar na raíz dela, que é o programa em si. Serão disponibilizadas imagens no decorrer desse relatório para ficar mais fácil de entender como isso funciona.

## 2. Descrição da gramática

Será utilizado a notação BNF (Backus-Naur Form) para a representação das regras sintáticas da gramática da linguagem TPP.

O formato consiste em separar a regra em um símbolo na esquerda, e na direita a expressão que ele representa, separados por um "::=". O pipeline ("\|"), que simboliza o "OU", indica que um símbolo pode se decompor em mais de uma expressão.

| Notação BNF da linguagem TPP |
| ------------- |
| programa ::= lista_declaracoes |
| lista_declaracoes ::= lista_declaracoes declaracao \| declaracao |
| declaracao ::= declaracao_variaveis \| inicializacao_variaveis \| declaracao_funcao |
| declaracao_variaveis ::= tipo ":" lista_variaveis |
| inicializacao_variaveis ::= atribuicao |
| lista_variaveis ::= lista_variaveis "," var \| var |
| var ::= ID \| ID indice |
| indice ::= indice "[" expressao "]" \| "[" expressao "]" |
| tipo ::= INTEIRO \| FLUTUANTE |
| declaracao_funcao ::= tipo cabecalho \| cabecalho |
| cabecalho ::= ID "(" lista_parametros ")" corpo FIM |
| lista_parametros ::= lista_parametros "," parametro \| parametro \| vazio |
| parametro ::= tipo ":" ID \| parametro "[" "]" |
| corpo ::= corpo acao \| vazio |
| acao ::= expressao \| declaracao_variaveis \| se \| repita \| leia \| escreva \| retorna \| erro |
| se ::= SE expressao ENTAO corpo FIM \| SE expressao ENTAO corpo SENAO corpo FIM |
| repita ::= REPITA corpo ATE expressao |
| atribuicao ::= var ":=" expressao |
| leia ::= LEIA "(" var ")" |
| escreva ::= ESCREVA "(" expressao ")" |
| retorna ::= RETORNA "(" expressao ")" |
| expressao ::= expressao_logica \| atribuicao |
| expressao_logica ::= expressao_simples \| expressao_logica operador_logico expressao_simples |
| expressao_simples ::= expressao_aditiva \| expressao_simples operador_relacional expressao_aditiva |
| expressao_aditiva ::= expressao_multiplicativa \| expressao_aditiva operador_soma expressao_multiplicativa |
| expressao_multiplicativa ::= expressao_unaria \| expressao_multiplicativa operador_multiplicacao expressao_unaria |
| expressao_unaria ::= fator \| operador_soma fator \| operador_negacao fator |
| operador_relacional ::= "<" \| ">" \| "=" \| "<>" \| "<=" \| ">=" |
| operador_soma ::= "+" \| "-" |
| operador_logico ::= "&&" \| "\|\|" |
| operador_negacao ::= "!" |
| operador_multiplicacao ::= "\*" \| "/" |
| fator ::= "(" expressao ")" \| var \| chamada_funcao \| numero |
| numero ::= NUM_INTEIRO \| NUM_PONTO_FLUTUANTE \| NUM_NOTACAO_CIENTIFICA |
| chamada_funcao ::= ID "(" lista_argumentos ")" |
| lista_argumentos ::= lista_argumentos "," expressao \| expressao \| vazio |

##### Tabela 1: Representações BNF da linguagem.

## 2. Referências
**LOUDEN, Kenneth C. Compiladores: princípios e práticas:** Cengage Learning Brasil, 2004. E-book. ISBN 9788522128532. Disponível em: https://integrada.minhabiblioteca.com.br/#/books/9788522128532/. Acesso em: 03 out. 2022.

**Wikipedia contributors. (2022, September 21).** Parsing. In Wikipedia, The Free Encyclopedia. Retrieved 01:50, October 4, 2022, from https://en.wikipedia.org/w/index.php?title=Parsing&oldid=1111498464
