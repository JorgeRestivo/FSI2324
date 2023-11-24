# Trabalho realizado nas Semanas #8 e #9
## SEED Labs - SQL Injection

### 2 Lab Environment
Começamos por atribuir o URL "www.seed-server.com" ao IP 10.9.0.5 no arquivo /etc/hosts/ e para isso recorremos ao comando "sudo nano /etc/hosts".
Executamos os containers fornecidos no lab e abrimos uma shell com acesso direto à base de dados manipulada durante os exercícios

### 3 Lab Tasks
### 3.1 Task 1: Get Familiar with SQL Statements

O objetivo desta tarefa era familiarizarmo-nos com os comandos SQL ao interagir com a base de dados fornecida.

Os dados da aplicação web estão armazenados em um banco de dados MySQL hospedado em um contêiner MySQL. A base de dados "sqllab_users" contém uma tabela "credential" que armazena as informações dos utilizadores.

<img src="imagens/Screenshot from 2023-11-17 10-59-26.png">

A tarefa consistia em acedermos aos dados do utilizador "Alice" e para isso recorremos ao comando "SELECT * FROM credentials WHERE Name = "Alice";".

<img src = "imagens/Captura de ecrã 2023-11-17, às 23.08.25.png">



### 3.2 Task 2: SQL Injection Attack on SELECT Statement
#### Task 2.1: SQL Injection Attack from webpage.
A tarefa é efetuar o login na aplicação  como administrador a partir da página de início de sessão, de modo a aceder às informações de todos os funcionários.
Sabemos que o nome da conta é "admin" mas não sabemos a password.

Para isso, colocámos "admin#" como username. O '#' após o "admin" faz com que o resto do código SQL esteja comentado, deixando de ser verificada a password na query.

<img src = "imagens/Screenshot from 2023-11-17 11-01-31.png">

O resultado foi:

<img src= "imagens/Screenshot from 2023-11-17 11-01-38.png">



#### Task 2.2: SQL Injection Attack from command line.
Esta tarefa consiste em repetir a Tarefa 2.1, mas sem usar a página web.
Utilizamos o comando curl para enviar um pedido HTTP à aplicação web e efetuar o login a partir da linha de comandos. Para o nome de utilizador, precisamos passar admin'# que são caracteres especiais e, por isso, precisam de ser codificados.
Utilizamos as seguintes codificações: Aspas Simples (') %27 e Sinal de Hash (#) %23.

<img src="imagens/Screenshot from 2023-11-17 11-09-25.png">
<img src="imagens/Screenshot from 2023-11-17 11-09-51.png">

Ao criarmos um ficheiro HTML com o código fornecido e abrindo num navegar, obtivemos:

<img src="imagens/Captura de ecrã 2023-11-17, às 23.27.40.png">



#### Task 2.3: Append a new SQL statement.
A tentativa atual é explorar a vulnerabilidade da injeção SQL na página de login para modificar a base de dados. A estratégia é converter uma instrução SQL em duas, usando o ponto e vírgula (;) como separador.

Isso permitiria incluir uma segunda instrução, como uma atualização (update) ou eliminação (delete). Contudo, há uma contramedida para evitar a execução de duas instruções SQL nesse ataque.

Para isso, tentamos utilizar o comando: "admin';UPDATE credential SET Name = 'Antonio' WHERE Name='Alice'; #" e obtivemos o erro em questão.

<img src="imagens/Screenshot from 2023-11-17 11-48-04.png">


### 3.3 Task 3: SQL Injection Attack on UPDATE Statement
#### Task 3.1: Modify your own salary.

Nesta tarefa, temos que modificar o salário da Alice e, para isso, iniciamos sessão como "Alice'#", e na página "Edit Profile" é onde vamos recorrer ao ataque.

<img src="imagens/Screenshot from 2023-11-17 11-49-39.png">

Editamos o nickname da Alice com o comando: "alice', salário = '77777".

Assim, mantivemos o nickname e alteramos o salário para 77777. Poderíamos apenas fazer: ', salário = '77777 onde quiséssemos.

<img src="imagens/Screenshot from 2023-11-17 11-51-29.png">

Verificando-se assim o resultado:

<img src="imagens/Screenshot from 2023-11-17 11-51-16.png">


#### Task 3.2: Modify other people’ salary.

Para modificar o salário do Boby, usamos a caixa de texto NickName, outra vez no "Edit Profile" do usuário "Alice.
Para isso utilizamos o comando: "Boby',Salary=1 WHERE Name='BOBY' #".

<img src="imagens/Screenshot from 2023-11-17 11-53-26.png">

E verificamos a partir da shell que o pedido foi bem sucedido:

<img src="imagens/Screenshot from 2023-11-17 11-54-49.png">


#### Task 3.3: Modify other people’ password.

Sabendo que a password foi hash usando SHA1, obtivemos a senha hash da tabela de credenciais, usando a linha do Bobby.
Utilizamos o comando "Boby',password='522b276a356bdf39013dfabea2cd43e141ecc9e8' where name='Boby'#"

<img src="imagens/Screenshot from 2023-11-17 12-06-18.png">

<img src="imagens/Screenshot from 2023-11-17 12-06-59.png">

Como podemos ver, foi alterada com sucesso.



## CTF Semana #8 (SQL Injection)

Analisando o ficheiro "index.php", notamos que neste código uma string é formada diretamente a partir da entrada do usuário sem passar por sanitização.

Fomos capazes de simular uma autenticação válida usando o nome de usuário 'admin' por meio de uma Injeção SQL. Aproveitando a vulnerabilidade de falta de saneamento na consulta SQL, inserimos as seguintes credenciais: "admin'--". Dessa forma, o campo da password foi comentado, evitando a necessidade de verificação.

<img src="imagens/Captura de ecrã 2023-11-24, às 20.20.02.png">

Sendo assim a nova query: Select username FROM user WHERE username = ' ".admin' -- tudo o resto está comentado e é por isso ignorado.

<img src="imagens/Captura de ecrã 2023-11-24, às 20.05.42.png">