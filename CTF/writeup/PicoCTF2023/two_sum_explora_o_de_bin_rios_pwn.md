========================================
two-sum - Exploração de Binários (Pwn)
========================================

Visão Geral do Desafio
----------------------

* **Plataforma / Evento:** PicoCTF 2023
* **Nome e Categoria do Desafio:** two-sum (Exploração de Binários)
* **Objetivo:** Explorar um estouro de inteiro com sinal para satisfazer uma condição aritmética contraditória em C.

Análise
-------

O desafio fornece o código-fonte em C e o endereço de um serviço remoto. O programa lê dois números inteiros da entrada padrão e verifica se ambos são maiores que zero, mas exige simultaneamente que a soma entre eles seja menor que zero.

Na aritmética matemática comum, a soma de dois números positivos nunca é negativa. A hipótese é que a operação ultrapassa o limite de armazenamento do tipo de dado utilizado em C, provocando inversão de sinal.

Vulnerabilidade / Conceito Central
----------------------------------

A falha ocorre devido a um estouro de inteiro (CWE-190) em variáveis do tipo ``int`` de 32 bits com sinal. O valor máximo representável nesse formato é 2.147.483.647; ao somar um valor positivo que ultrapasse esse teto, o bit mais significativo (bit de sinal) é ativado, fazendo o resultado dar a volta para a faixa negativa. A prevenção exige validar se ``a > INT_MAX - b`` antes de realizar a adição.

Solução Passo a Passo
---------------------

1. Examinar a função de verificação no código-fonte fornecido para confirmar os tipos das variáveis e as condições de desvio.

.. code-block:: c

   // Estrutura conceitual da falha aritmética e sua mitigação
   int soma = num1 + num2;
   if (num1 > 0 && num2 > 0 && soma < 0) {
       // Fluxo alternativo alcançado após estouro de inteiro
   }

2. Escolher dois valores positivos ``[valor_positivo_1]`` e ``[valor_positivo_2]`` cuja soma exceda o limite máximo de 32 bits com sinal.
3. Conectar ao serviço do desafio e inserir os dois números separados por quebra de linha.
4. Confirmar que o resultado negativo gerado pelo estouro ativa o bloco protegido e retorna ``CTF{exemplo_de_flag_aqui}``.