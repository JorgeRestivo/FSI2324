# Trabalho realizado nas Semanas #11, #12e #13
## SEED Labs - Public Key Infrastructures

### 3 Lab Tasks
### 3.1 Task 1: Becoming a Certificate Authority (CA)

Começámos por copiar o ficheiro de certificado (openssl.cnf) para o nosso diretório local uma vez que iríamos fazer alterações neste ficheiro.

<img src = "imagens/copiar_ficheiro.png">

Posteriormente, descomentamos a linha "unique_subject" para permitir a criação de certificados com o mesmo assunto. Isso significa que certificados com o mesmo "assunto" visto que será útil para futuras tarefas.

<img src = "imagens/unique_subject.png">


Começamos por criar um diretório "demoCA" para termos a nossa prórpia autoridade de certificação. Criámos subdiretórios como "crl", "certs" e "newcerts" para usarmos futuramente para o armazenamento de certificados.

<img src = "imagens/dirdemoCA.png">

<img src = "imagens/newsubdirs.png">


Uma autoridade de certificação (CA) emite certificados digitais. Neste caso, está a ser sugerida a geração de um certificado autoassinado para a CA. O termo "autoassinado" significa que a própria CA atesta a autenticidade do seu próprio certificado.

<img src = "imagens/myCA.png">

Para isso foi necessário completarmos com algumas informações:

<img src = "imagens/mydata.png">

Podemos utilizar os seguintes comandos para visualizar o conteúdo decodificado do certificado X509 e da chave RSA (-text para decodificar o conteúdo em texto simples; -noout para não imprimir a versão codificada).

<img src = "imagens/ca.crt_command.png">

Este comando utiliza o OpenSSL para decodificar e exibir o conteúdo do certificado X509 em formato de texto. Sendo esse:

<img src = "imagens/ca.crt_output.png">


Já o comando:

<img src = "imagens/ca.key_command.png">

usa o OpenSSL para decodificar e exibir o conteúdo da chave RSA em formato de texto. Sendo este:

<img src = "imagens/ca.key_output.png">

