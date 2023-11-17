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
### Task 2.1: SQL Injection Attack from webpage.
A tarefa é efetuar o login na aplicação  como administrador a partir da página de início de sessão, de modo a aceder às informações de todos os funcionários.
Sabemos que o nome da conta é "admin" mas não sabemos a password.

Para isso, colocámos "admin#" como username. O '#' após o "admin" faz com que o resto do código SQL esteja comentado, deixando de ser verificada a password na query.

<img src = "imagens/Screenshot from 2023-11-17 11-01-31.png">

O resultado foi:

<img src= "imagens/Screenshot from 2023-11-17 11-01-38.png">

### Task 2.2: SQL Injection Attack from command line.
Esta tarefa consiste em repetir a Tarefa 2.1, mas sem usar a página web.
Utilizamos o comando curl para enviar um pedido HTTP à aplicação web e efetuar o login a partir da linha de comandos. Para o nome de utilizador, precisamos passar admin'# que são caracteres especiais e, por isso, precisam de ser codificados.
Utilizamos as seguintes codificações: Aspas Simples (') %27 e Sinal de Hash (#) %23.

<img src="imagens/Screenshot from 2023-11-17 11-09-25.png">


Ao criarmos um ficheiro HTML com o código fornecido e abrindo num navegar, obtivemos:

<img src="imagens/Captura de ecrã 2023-11-17, às 23.27.40.png">