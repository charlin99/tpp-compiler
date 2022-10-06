# Relatório da análise sintática da linguagem TPP
**Aluno**: Carlos Miguel dos Santos Filho¹

¹Bacharelado em Ciência da Computação – Universidade Tecnológica Federal do Paraná (UTFPR)

**Abstract.** In this second part of the assignment, the parsing of the TPP language was realized. Occurring after the lexical analysis, the parsing consists of taking the previously generated tokens and establishing the grammar to be used in the language.

**Resumo.** Nessa segunda parte do trabalho, foi realizada a análise sintática da linguagem TPP. Ocorrendo após a análise léxica, a análise sintática consiste em pegar os tokens previamente gerados e estabelecer a gramática a ser seguida na linguagem.

## 1. Introdução
Após a realização da análise léxica, prosseguimos para a análise sintática da linguagem. Essa etapa consiste em verificar a estrutura gramatical do código, para ver se está de acordo com os tokens gerados na análise léxica e respeita a gramática estabelecida.

A análise sintática converte o código no arquivo texto em uma estrutura de dados que se assemelha a uma árvore com várias ramificações que representam as diferentes regras gramaticais, e o código fonte fica pendurado nas folhas da árvore. Conforme você sobe na árvore, mais generalizadas vão ficando as regras, a ponto de você chegar na raíz dela, que é o programa em si. Serão disponibilizadas imagens no decorrer desse relatório para ficar mais fácil de entender como isso funciona.

## 2. Descrição da gramática

Será utilizado a notação BNF (Backus-Naur Form) para a representação das regras sintáticas da gramática da linguagem TPP.

O formato consiste em separar a regra em um símbolo na esquerda, e na direita a expressão que ele representa, separados por um "::=". O pipeline ("\|"), que simboliza o "OU", indica que um símbolo pode se decompor em mais de uma expressão.

Abaixo estão alguns exemplos representados por diagramas:

A notação BNF de um **programa** pode ser representada por: **programa ::= lista_declaracoes**\
\
\
![Diagrama de programa](https://raw.githubusercontent.com/charlin99/tpp-compiler/main/relatorios/img/diagram/programa.png)
##### Figura 1: Diagrama da expressão de programa.
\
\
A notação BNF do símbolo **tipo** pode ser representado pela seguinte expressão: **tipo ::= INTEIRO \| FLUTUANTE**\
\
\
![](https://raw.githubusercontent.com/charlin99/tpp-compiler/main/relatorios/img/diagram/tipo.png)
##### Figura 2: Diagrama da expressão de tipo.
\
\
E a notação BNF do símbolo **ação** pode ser representada pela expressão: **acao ::= expressao \| declaracao_variaveis \| se \| repita \| leia \| escreva \| retorna \| erro**\
\
\
![](https://raw.githubusercontent.com/charlin99/tpp-compiler/main/relatorios/img/diagram/acao.png)
##### Figura 3: Diagrama da expressão de ação.
\
\
Como existem inúmeras regras sintáticas, elas serão condensadas em uma tabela, que pode ser vista abaixo. Ela possui todas as regras sintáticas da linguagem TPP.

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

## 3. Formato utilizado na análise sintática
Foi utilizado o formato LALR(1) na análise sintática da linguagem. LALR significa Look-Ahead LR (LR significa Left-to-Right) e ele é o analisador mais poderoso, que pode lidar com regras gramaticais grandes e em quantidade. Ele possui maior reconhecimento de linguagem que o LR(1), pode gerar ambiguidade pois ele unifica estados semelhantes, porém diminui consideravelmente o número de estados da tabela final.

## 4. Implementação e utilização da ferramenta Yacc
O Yacc (Yet another compiler-compiler) é uma ferramenta utilizada para a geração de análises sintáticas de compiladores. Ele está implementado no PLY, biblioteca do Python, que foi utilizada também na análise léxica e também foi usada para a implementação dessa parte do trabalho.

O Yacc do PLY utiliza os tokens gerados pela análise léxica, gerando os autômatos com pilha no formato LALR(1), onde é realizada a redução de tokens que já estão empilhados. É definida uma função para cada regra gramatical iniciada pelo prefixo 'p_', exemplo: 'p_lista_declaracoes' irá gerar a função da regra gramatical das listas de declarações.

```
def p_lista_declaracoes(p):
    """lista_declaracoes : lista_declaracoes declaracao
                        | declaracao
    """
    pai = MyNode(name='lista_declaracoes', type='LISTA_DECLARACOES')
    p[0] = pai
    p[1].parent = pai

    if len(p) > 2:
        p[2].parent = pai
```
#### Código 1: Função para o reconhecimento do Yacc do PLY.

E cada classe gramatical também possui uma separada para quando acontece algum erro na execução dela, como pode ser visto no código 2 abaixo, que é o tratamento do erro da função vista no código 1, acima.
```
def p_list_declaracoes_error(p):
    """lista_declaracoes : lista_declaracoes error
    """
    
    print("Erro na regra: lista_declaracoes")
```
#### Código 2: Função para o tratamento de erro da função do código 1.

## 5. Implementação da Árvore Sintática
A biblioteca Anytree do Python foi utilizada para a geração dos gráficos das árvores sintáticas, e foi criada uma classe MyNode para a criação dos nós dessa árvore.

Abaixo pode ser visto o código do nó da árvore utilizando o NodeMixin.
```
class MyNode(NodeMixin):  # Add Node feature   

  def __init__(self, name, parent=None, id=None, type=None, label=None, children=None):
    super(MyNode, self).__init__()
    global node_sequence

    if (id):
      self.id = id
    else:
      self.id = str(node_sequence) + ': ' + str(name)
        
    self.label = name
    # self.name = name + '_' + str(node_sequence)
    self.name = name
    node_sequence = node_sequence + 1
    self.type = type
    self.parent = parent
    if children:
      self.children = children

  def nodenamefunc(node):
    return '%s' % (node.name)

  def nodeattrfunc(node):
    return '%s' % (node.name)

  def edgeattrfunc(node, child):
    # return 'label="%s:%s"' % (node.name, child.name)
    return ''

  def edgetypefunc(node, child):
    return '--'
```
#### Código 3: Classe do nó.

## 6. Saídas geradas

Os códigos testados foram disponibilizados pelo professor num diretório chamado "sintatica-testes". Abaixo no código 3, está o arquivo expressoes.tpp que está nesse diretório.

```
inteiro: a[1024]

inteiro func(inteiro: a[])
    retorna(0)
fim

inteiro principal()
    inteiro: i,b

    i:= 1

    a[i]:= b := 0

    b := b + func(a)

    b := +b + i
    
    retorna(0)
fim
```
#### Código 4: Exemplo de código a ser testado.

E ao ser passado como parâmetro na execução do parser.py, gerará os seguintes arquivos:

expressoes.tpp.ast.dot

expressoes.tpp.ast.png

expressoes.tpp.ast2.png

expressoes.tpp.unique.ast.dot

expressoes.tpp.unique.ast.png

Ao abrirmos o último arquivo, expressoes.tpp.unique.ast.png, podemos ver a árvore gerada, com o código fonte disponível na folha dela:
![](https://raw.githubusercontent.com/charlin99/tpp-compiler/main/p2-analise-sintatica/sintatica-testes/expressoes.tpp.unique.ast.png)
#### Figura 1: Árvore sintática gerada do código.

## 7. Conclusão
O PLY nos gerou uma árvore sintática abstrata que será utilizada na análise semântica e geração de código, assim dando sequência à construção desse compilador. Os testes se mostrataram eficientes, acertando onde deveria e acusando os erros certos caso houvesse algum no código.

## 8. Referências
**LOUDEN, Kenneth C. Compiladores: princípios e práticas:** Cengage Learning Brasil, 2004. E-book. ISBN 9788522128532. Disponível em: https://integrada.minhabiblioteca.com.br/#/books/9788522128532/. Acesso em: 03 out. 2022.

**Wikipedia contributors. (2022, September 21). Parsing.** In Wikipedia, The Free Encyclopedia. Retrieved 01:50, October 4, 2022, from https://en.wikipedia.org/w/index.php?title=Parsing&oldid=1111498464

**Wikipedia contributors. (2022, March 19). LALR parser.** In Wikipedia, The Free Encyclopedia. Retrieved 03:00, October 4, 2022, from https://en.wikipedia.org/w/index.php?title=LALR_parser&oldid=1078075749
