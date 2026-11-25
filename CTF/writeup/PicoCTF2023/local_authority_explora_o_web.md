========================================
Local Authority - Exploração Web
========================================

Visão Geral do Desafio
----------------------

* **Plataforma / Evento:** PicoCTF 2023
* **Nome e Categoria do Desafio:** Local Authority (Exploração Web)
* **Objetivo:** Identificar e contornar uma verificação de autenticação executada exclusivamente no navegador do usuário.

Análise
-------

O ambiente fornece uma página web simples contendo um formulário de login com campos para usuário e senha. Ao submeter dados aleatórios, a página recarrega e exibe uma mensagem de acesso negado.

A inspeção inicial da estrutura da página revela o carregamento de um arquivo JavaScript externo responsável por processar o evento de envio do formulário. Como não há requisições de rede enviando credenciais para validação em uma API externa, formula-se a hipótese de que a lógica de verificação reside integralmente no código do cliente.

Vulnerabilidade / Conceito Central
----------------------------------

A falha decorre da validação de controle de acesso no lado do cliente (CWE-602). Quando uma aplicação delega ao navegador a decisão de autenticar um usuário, todo o código e os parâmetros de comparação ficam expostos para leitura e manipulação. Para corrigir o problema, a validação de credenciais deve ocorrer exclusivamente no servidor, retornando apenas o estado da sessão autorizada.

Solução Passo a Passo
---------------------

1. Abrir as ferramentas de desenvolvedor do navegador na aba de código-fonte ou rede para listar os recursos carregados pela página de login.
2. Localizar o arquivo de script vinculado ao formulário e examinar a função acionada durante a tentativa de autenticação.

.. code-block:: javascript

   // Exemplo conceitual da lógica insegura exposta no cliente
   function validarAcesso(usuario, senha) {
       if (usuario === "[usuario_esperado]" && senha === "[senha_esperada]") {
           return true;
       }
       return false;
   }

3. Identificar os valores literais comparados na estrutura condicional e inseri-los como ``[credenciais_identificadas]`` nos campos do formulário.
4. Submeter o formulário para acionar o redirecionamento autorizado e visualizar a flag no formato ``CTF{exemplo_de_flag_aqui}``.