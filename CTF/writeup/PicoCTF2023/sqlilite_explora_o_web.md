========================================
SQLiLite - Exploração Web
========================================

Visão Geral do Desafio
----------------------

* **Plataforma / Evento:** PicoCTF 2023
* **Nome e Categoria do Desafio:** SQLiLite (Exploração Web)
* **Objetivo:** Compreender como a concatenação direta de entradas de usuário altera a lógica de consultas em bancos de dados relacionais.

Análise
-------

O desafio disponibiliza um endpoint web com uma tela de autenticação conectada a um banco de dados SQLite. Ao testar um par comum de usuário e senha, a aplicação responde com uma mensagem de falha e exibe a consulta SQL montada no servidor.

Observa-se que os valores digitados nos campos aparecem literalmente dentro das aspas simples da cláusula ``WHERE``. A hipótese inicial é que o sistema constrói comandos SQL por concatenação simples de strings, permitindo a alteração da estrutura lógica da consulta.

Vulnerabilidade / Conceito Central
----------------------------------

Trata-se de uma vulnerabilidade de injeção de SQL (CWE-89), causada pela ausência de separação entre código executável e dados fornecidos pelo usuário. Quando delimitadores de texto não são escapados, a entrada pode encerrar prematuramente o campo de dados e introduzir operadores booleanos que tornam a condição de busca sempre verdadeira. A mitigação padrão consiste em substituir a concatenação por consultas parametrizadas (prepared statements).

Solução Passo a Passo
---------------------

1. Enviar uma requisição de teste com valores alfanuméricos simples para observar o formato exato da consulta exibida na resposta.

.. code-block:: sql

   -- Estrutura conceitual exibida pela aplicação
   SELECT * FROM users WHERE name = '[entrada_usuario]' AND password = '[entrada_senha]';

2. Analisar como as aspas simples delimitam o parâmetro de nome de usuário na instrução acima.
3. Fornecer uma entrada estruturada ``[entrada_booleana_conceitual]`` que feche o delimitador de texto e adicione uma condição lógica verdadeira, anulando a verificação da senha.
4. Confirmar a autenticação bem-sucedida na interface e inspecionar o código-fonte da página restrita para ler ``CTF{exemplo_de_flag_aqui}``.