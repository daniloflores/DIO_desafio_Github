# Funcionamento do git

Para entender o que acontece por trás do controle de versionamento do git, vamos entender primeiramente o conceito de encriptação SHA-1.

**SHA-1** (secure hash algorithm) é um algoritmo de encriptação que resume o conteúdo de um arquivo em uma chave, um código de caracteres de 40 dígitos. Foi desenvolvido pela Agência de Segurança dos EUA (NSA). Para mais detalhes, ver [página da Wikipedia](https://pt.wikipedia.org/wiki/SHA-1).

O código/chave gerado para cada arquivo é único e serve como identificação do arquivo e da sua versão. Ou seja, caso haja qualquer alteração mínima do arquivo, a chave gerada será outra. E se a alteração for desfeita, a chave voltará a ser a mesma da versão original.
No terminal do gitbash, podemos reproduzir esse processo de encriptação usando o comando:

    openssl sha1 [nome do arquivo]

Exercício: aplicar esse comando a um arquivo de texto, mudar uma vírgula e repetir o comando, depois desfazer a alteração e repetir o comando novamente. Você deverá notar que a chave SHA-1 será modificada com a primeira alteração, mas voltará ao seu valor original após desfazer a alteração.
