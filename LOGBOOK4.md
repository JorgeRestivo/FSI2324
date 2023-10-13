# Trabalho realizado nas Semanas #4 e #5
## SEED Labs – Environment Variable and Set-UID Program Lab

### 2. LAB TASKS
### 2.1 Task 1: Manipulating Environment Variables
Através do comando "export" podemos criar uma variável de ambiente e imprimi-la através do comando "printenv <nome da variável>".
<img src="imagens/Captura de ecrã 2023-10-13, às 14.15.37.png">

Caso queiramos imprimir o ambiente completo, esta já vai estar presente:
<img src="imagens/Captura de ecrã 2023-10-13, às 14.16.24.png">

Através do comando "unset" podemos eliminar esta variável. Se voltarmos a imprimir o ambiente, então esta variável já não aparece.
<img src="imagens/Captura de ecrã 2023-10-13, às 14.17.22.png">

### 2.2 Task 2: Passing Environment Variables from Parent Process to Child Process
Começamos por compilar o ficheiro "myprintenv.c" e posteriormente correr e imprimir o resultado do ficheiro executável de "myprintenv.c", "a.out", num ficheiro chamado "file1".
<img src="imagens/Captura de ecrã 2023-10-13, às 15.49.39.png">

Novo ficheiro "file1":
<img src="imagens/Captura de ecrã 2023-10-13, às 15.51.05.png">

Após efetuar as alterações pedidas no ficheiro "myprintenv.c":
<img src="imagens/Captura de ecrã 2023-10-13, às 15.17.24.png">

voltamos a compilar este ficheiro e novamente a imprimir o resultado do ficheiro executável "a.out" num ficheiro chamado "file2".
<img src= "imagens/Captura de ecrã 2023-10-13, às 15.54.54.png">

Por fim, corremos o comando "diff file1 file2" com o objetivo de perceber se existiam diferenças entre os dois ficheiros. Não existem.

