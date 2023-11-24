
## CTF Semana #8 (SQL Injection)

Analisando o ficheiro "index.php", notamos que neste código uma string é formada diretamente a partir da entrada do usuário sem passar por sanitização.

Fomos capazes de simular uma autenticação válida usando o nome de usuário 'admin' por meio de uma Injeção SQL. Aproveitando a vulnerabilidade de falta de saneamento na consulta SQL, inserimos as seguintes credenciais: "admin'--". Dessa forma, o campo da password foi comentado, evitando a necessidade de verificação.

<img src="imagens/Captura de ecrã 2023-11-24, às 20.20.02.png">

Sendo assim a nova query: Select username FROM user WHERE username = ' ".admin' -- tudo o resto está comentado e é por isso ignorado.

<img src="imagens/Captura de ecrã 2023-11-24, às 20.05.42.png">