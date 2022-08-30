# Relatório da análise léxica da linguagem TPP

**Aluno**: Carlos Miguel dos Santos Filho \
**RA**: 1921843

## Especificações da linguagem
- Tipos básicos de dados suportados: **inteiro** e **flutuante**
- Suporte a arranjos uni e bidimensionais (_arrays_)
  - Exemplos:
    - tipo: identificador[**dim**]
    - tipo: identificador[**dim**][**dim**]
- Variáveis locais e globais devem ter um dos tipos especificados
- Tipos de funções podem ser omitidos (quando omitidos viram um procedimento e um tipo void é devolvido explicitamente
- Linguagem quase fortemente tipificada: nem todos os erros são especificados mas sempre deve ocorrer avisos
- Operadores aritméticos: +, -, * e /
- Operadores lógicos: e (&&), ou (||) e não (!)
- Operador de atribuição (:=)
