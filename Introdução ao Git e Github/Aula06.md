# Chaves SSH e tokens
### SSH
 As chaves SSH são formas seguras de estabelecer uma conexão entre duas máquinas. Hoje em dia ela é usada, por exemplo, para conectar uma máquina local com o Github web (nuvem). Antigamente, essa comunicação era feita apenas pelo nome e senha do usuário do Github, mas foi mudado para a SSH por questão de segurança.
O processo para comunicar a máquina local com o Github consiste nos seguintes passos
-	Gerar uma chave SSH no computador, no terminal Git Bash:
 - Comando:

      ssh-keygen -t ed25519 -C [enderço email Github]

 -	Isso vai gerar um par de chaves (1 pública e 1 privada) e te informar onde essas chaves estão armazenadas
-	Depois, vamos ao Github web para informar a ele a chave pública (extenção .pub)
  -	Ir na no local do computador onde as chaves estão armazenadas
	- Copiar o conteúdo do arquivo .pub
	- Ir ao Github web > Settings > SSH and GPG keys > New SSH key
	- Colar o conteúdo da chave pública no campo e clicar em “Add SSH key”
- 	Por fim, devemos iniciar o programa SSH agent no computador e informar para ele a nossa chave privada. no terminal Git Bash:
	- Iniciar o SSH agente com o comando:

        eval $(ssh-agent -s)

	- Estando na pasta onde estão as chaves, informar a chave privada (sem .pub), com o comando: ssh-add [chave]

### Token de acesso pessoal
A conexão por SSH é segura, e permite a clonagem de repositórios a partir do endereço SSH do repositório. Porém, também é possível clonar a partir do endereço URL do repositório. Para isso, devemos usar uma forma alternativa de conexão, no lugar do SSH. Essa forma alternativa é o token de acesso pessoal, que permite o acesso ao repositório a partir do protocolo HTTPS. Para gerar um token, devemos seguir os seguintes passos:
-	Gerar o Token no Github:
 -	Entre no Github web > Settings > Developer settings > Personal access tokens > Generate new token
 -	NÃO FECHAR A JANELA LOGO DE CARA. O token gerado só será mostrado uma vez, por isso, é importante copiá-lo em um lugar seguro antes de fechar a janela.
 -	Cada vez que tentarmos clonar um repositório com a URL, ele vai pedir o token de acesso pessoal. Basta digitar o token.
Observação: é possível configurar o git para armazenar o token de acesso pessoal, de modo que não será necessário digitá-lo todas as vezes que for copiar um repositório. Eu fiz esse processo em [outro tutorial, usando a linguagem R](https://happygitwithr.com/https-pat.html#https-vs-ssh). Talvez não seja o ideal para todos os usuários.
