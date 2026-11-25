========================================
rotation - Criptografia
========================================

Visão Geral do Desafio
----------------------

* **Plataforma / Evento:** PicoCTF 2023
* **Nome e Categoria do Desafio:** rotation (Criptografia)
* **Objetivo:** Analisar e reverter uma cifra de substituição monoalfabética baseada em deslocamento fixo de caracteres.

Análise
-------

O desafio fornece apenas um arquivo de texto plano contendo uma sequência cifrada com formato semelhante ao padrão de uma flag, incluindo chaves e sublinhados intactos.

A observação inicial mostra que números e símbolos não sofreram alteração, enquanto as letras mantiveram a distinção entre maiúsculas e minúsculas. Isso indica uma cifra clássica de rotação sobre o alfabeto latino de 26 letras.

Vulnerabilidade / Conceito Central
----------------------------------

Cifras de deslocamento fixo, como a cifra de César, utilizam algoritmos criptográficos fracos (CWE-327) com um espaço de chaves limitado a apenas 25 deslocamentos possíveis. Como a frequência das letras e a estrutura das palavras são preservadas, um analista pode testar todas as rotações possíveis em frações de segundo. Aplicações reais devem empregar algoritmos modernos e autenticados, como AES-GCM, com chaves geradas aleatoriamente.

Solução Passo a Passo
---------------------

1. Ler o conteúdo do arquivo cifrado no terminal e verificar o comprimento do prefixo antes do caractere ``{``.
2. Aplicar uma função de deslocamento reverso modular testando os valores de rotação no intervalo de 1 a 25.

.. code-block:: python

   # Lógica conceitual de reversão de deslocamento alfabético
   def rotacionar_caractere(caractere, deslocamento):
       if "a" <= caractere <= "z":
           base = ord("a")
           return chr((ord(caractere) - base - deslocamento) % 26 + base)
       return caractere

3. Identificar o valor de ``[deslocamento_valido]`` que transforma o prefixo cifrado no identificador esperado da plataforma.
4. Aplicar esse mesmo deslocamento ao restante de ``[texto_cifrado]`` para obter a mensagem legível ``CTF{exemplo_de_flag_aqui}``.