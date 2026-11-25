========================================
FactCheck - Engenharia Reversa
========================================

Visão Geral do Desafio
----------------------

* **Plataforma / Evento:** PicoCTF 2024
* **Nome e Categoria do Desafio:** FactCheck (Engenharia Reversa)
* **Objetivo:** Inspecionar estaticamente um binário compilado para reconstruir uma cadeia de caracteres montada na memória.

Análise
-------

O arquivo fornecido é um executável ELF de 64 bits. Ao executá-lo em um ambiente isolado, o programa encerra imediatamente sem exibir mensagens na saída padrão nem solicitar argumentos.

A busca por strings legíveis diretamente no arquivo revela apenas fragmentos isolados de letras e símbolos. A hipótese é que o programa aloca esses pedaços na pilha ou em objetos de string durante a execução e finaliza sem imprimi-los.

Vulnerabilidade / Conceito Central
----------------------------------

Embutir segredos diretamente em binários compilados configura uso de credenciais ou dados sensíveis codificados rigidamente (CWE-798). Mesmo quando o programa não exibe o conteúdo na tela, as instruções de atribuição e concatenação permanecem presentes na seção de código executável e podem ser lidas com desmontadores. Segredos nunca devem ser armazenados dentro de artefatos distribuídos ao usuário final.

Solução Passo a Passo
---------------------

1. Verificar o formato e a arquitetura do arquivo utilizando utilitários de inspeção estática no terminal.

.. code-block:: bash

   # Inspeção inicial do formato e desmontagem das instruções
   file [binario_alvo]
   objdump -d -M intel [binario_alvo]

2. Abrir o binário em um decompilador para localizar a função ``main`` e observar a declaração de variáveis locais de texto.
3. Acompanhar a sequência cronológica em que caracteres individuais e pequenos blocos são anexados à variável principal.
4. Reunir os fragmentos na ordem exata de execução para formar ``CTF{exemplo_de_flag_aqui}``.