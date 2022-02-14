# Objetos do git
O git controla o versionamento a partir de 3 objetos: blobs, trees e commits.

## Blob (bolha)
Os arquivos controlados pelo git são armazenados em blobs. O blob é uma estrutura que contém não só o conteúdo do arquivo, mas também alguns metadados: tipo (blob), um espaço, o tamanho, um “\0”.
Por isso, quando fazemos a encriptação SHA-1, temos resultados diferentes se fizermos fora do git (função openssl) ou dentro do git (git hash-object).
Assim, o comando que faz a encriptação com o git...

	echo ‘conteudo’ | git hash-object --stdin

... retorna um resultado diferente do que a encriptação sem git.

    echo -e ‘conteudo’ | openssl SHA-1

Porém, se adicionarmos ao conteúdo os elementos do blob (tipo, espaço, tamanho e \0), a encriptação sem git passa a reproduzir a encriptação com git

	echo -e ‘blob 9\0conteudo’ | openssl SHA-1

## Tree (árvore)
As trees armazenam blobs (apontam para o blob) com alguns outros metadados. A estrutura da tree armazena seu tipo (tree), tamanho, um \0, o ponteiro para o blob, o SHA-1 do blob e o nome do arquivo do blob. Uma tree pode armazenar/apontar mais de um blob. As trees/árvores também podem apontar para outras árvores, além de apontar para blobs. Assim, as árvores são usadas para organizar os diretórios do repositório git, assim como acontece no nosso computador. Da mesma forma que um diretório pode conter arquivos e outros diretórios, no git uma tree pode apontar para blobs e para outras trees. O git também gera uma chave SHA-1 para cada tree.
Uma consequência dessa estrutura de trees e blobs é que qualquer mudança em qualquer blob dentro da tree vai reverberar para uma mudança na própria tree. Em outras palavras, mudanças em um arquivo vão mudar o SHA-1 do seu blob, esse novo SHA-1 do blob vai ser registrado na tree e, assim, mudar o conteúdo da tree, o que vai se repercutir em uma mudança no SHA-1 da tree. Da mesma forma, essa mudança no SHA-1 da tree vai se repercutir em todas as trees dos diretórios acima.

## Commit
O commit é a estrutura que organiza o versionamento a partir das blobs e trees. Um commit contém seu tipo (commit), tamanho, o ponteiro para uma árvore, o ponteiro para o último commit anterior, um autor, uma mensagem e uma timestamp. Cada nova versão do repositório é feita a partir de um novo commit. Assim como as outras estruturas, o git também calcula uma chave SHA-1 para cada commit. Esse controle todo dá confiabilidade ao controle de versão. Cada commit registrar exatamente de onde ele veio (commit anterior) permitindo organizar as alterações em sequência, em uma linha do tempo. É impossóvel haver qualquer alteração maliciosa no histórico das versões, sem que fique um rastro. Se alguém alterar a informação de algum commit já registrado, isso mudará o SHA-1 daquele commit e de todos os commits daquele ponto em diante (uma vez que o SHA-1 de cada commit altera o SHA-1 do commit seguinte).

## Sistema distribuído
Essa estrutura também garante que os repositórios possam ser sistemas distribuídos, para trabalho em equipe em múltiplas máquinas. Toda vez que espelhamos um repositório em uma máquina local, ele vem com o registro dos commits anteriores e suas chaves SHA-1 únicas. Caso aconteça alguma coisa com a cópia do repositório em qualquer máquina (inclusive na nuvem), é possível resgatar o histórico a partir das cópias das outras máquinas, que também terão registro dos commits com suas chaves SHA-1 únicas e verificáveis.
