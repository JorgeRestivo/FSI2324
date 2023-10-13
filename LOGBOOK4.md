# Trabalho realizado nas Semanas #4 e #5
## SEED Labs – Environment Variable and Set-UID Program Lab

### 2. LAB TASKS
### 2.1 Manipulating Environment Variables
Através do comando "export" podemos criar uma variável de ambiente e imprimi-la através do comando "printenv <nome da variável>".
<img src="imagens/Captura de ecrã 2023-10-13, às 14.15.37.png">

Caso queiramos imprimir o ambiente completo, esta já vai estar presente:
<img src="imagens/Captura de ecrã 2023-10-13, às 14.16.24.png">

Através do comando "unset" podemos eliminar esta variável. Se voltarmos a imprimir o ambiente, então esta variável já não aparece.
<img src="imagens/Captura de ecrã 2023-10-13, às 14.17.22.png">


### 2.2 Passing Environment Variables from Parent Process to Child Process
Começamos por compilar o ficheiro "myprintenv.c" e posteriormente correr e imprimir o resultado do ficheiro executável de "myprintenv.c", "a.out", num ficheiro chamado "file1".
<img src="imagens/Captura de ecrã 2023-10-13, às 15.49.39.png">

Novo ficheiro "file1":
<img src="imagens/Captura de ecrã 2023-10-13, às 15.51.05.png">

Após efetuar as alterações pedidas no ficheiro "myprintenv.c":
<img src="imagens/Captura de ecrã 2023-10-13, às 15.17.24.png">

voltamos a compilar este ficheiro e novamente a imprimir o resultado do ficheiro executável "a.out" num ficheiro chamado "file2".
<img src= "imagens/Captura de ecrã 2023-10-13, às 15.54.54.png">

Por fim, corremos o comando "diff file1 file2" com o objetivo de perceber se existiam diferenças entre os dois ficheiros. Não existem.


#### Observações:
No file1, vemos apenas as variáveis de ambiente impressas pelo processo filho, enquanto no file2 vemos apenas as variáveis de ambiente impressas pelo processo pai.
Quando comparados estes dois ficheiros, tanto o processo filho quanto o processo pai estão a aceder às mesmas variáveis de ambiente. Ambos os processos, quando chamam o printenv(), vão aceder e imprimir o mesmo conjunto de variáveis de ambiente porque o processo filho herda as variáveis de ambiente do seu processo pai.
Isto significa que qualquer variável de ambiente definida no processo pai é acessível ao processo filho sem a necessidade de passagem explícita. Quando o processo filho acede à variável environ, ele vê as variáveis de ambiente do processo pai.


### 2.3 Environment Variables and execve()
Compilar o ficheiro "myenv.c", criámos um novo executável "myenv" e executamo-lo.
<img src="imagens/Captura de ecrã 2023-10-13, às 16.30.49.png">

Quando é executado, ele substitui diretamente o processo atual pelo processo "/urs/bin/env" usando o comando "execve()". Assim o comando "env" vai imprimir as variáveis de ambiente do processo atual.
Decidimos imprimir o resuldo do executável "myenv" num ficheiro chamado "file3". Nada foi impresso neste ficheiro uma ve que o argumento NULL estava a ser passado ao comando execve.
Após fazer as respetivas mudanças no ficheir "myenv.c":
<img src="imagens/Captura de ecrã 2023-10-13, às 20.08.49.png">

Voltámos a criar um novo ficheir "file4" para imprimir o resultado do já compilado "myenv.c".

Desta vez, ao executarmos o comando "diff file3 file4":
<img src ="imagens/Captura de ecrã 2023-10-13, às 16.39.54.png">

Isto acontece pois o "file3" não tem nenhum conteúdo enquanto que o "file4" tem todas as variáveis de ambiente do "environ" array.

#### Observações
As variáveis de ambiente do novo programa são determinadas pelo ambiente do processo de chamada. Ao passar NULL como argumento, ele herda um conjunto nulo de variáveis, enquanto que ao passar "environ" permite herdar o conjunto completo de variáveis de ambiente do processo atual.


### 2.4: Environment Variables and system()
Após criarmos um ficheiro chamado "file4" que contém o código do enunciado,
<img src="imagens/Captura de ecrã 2023-10-13, às 16.59.33.png">
conseguimos ver que quando o executamos, vai imprimir todas as variáveis de ambiente.
No entanto, para conseguirmos tirar uma conclusão, decidimos criar uma nova variável "nova_variável=OLÁ" e ver se esta seria impressa através da função "env" no terminal.
<img src="imagens/Captura de ecrã 2023-10-13, às 17.24.29.png">
Algo que também aconteceu quando executamos o programaga task4:
<img src="imagens/Captura de ecrã 2023-10-13, às 20.29.35.png">

Isto levou-nos a concluir que a função "system" é uma função que permite ao programa executar comandos no terminal, começando por iniciar um novo terminal temporário. Todas das variáveis de ambiente do "current user" e do terminal original vão estar presentes. O comando "env", dá-nos as variáveis de ambiente do "current user" e do terminal atual.


### 2.5: Environment Variable and Set-UID Programs
Após criar o ficheiro "task5.c" com o código indicado, tornei-o num programa Set-UID através dos comandos:
- sudo chown root task5
- sudo chmod 4755 task5
de forma a alterar as permisões e prioriedades dos diretórios.
A partir do comando chown root, mudamos o proprietário do arquivo para root de forma a ter controle total sobre o ficheiro.
Com o comando chmod 4755 alteramos as permissões deste ficheiro, definindo certas permissões características de um programa Set-UID.
Assim, quando "task5" é executado, este será feito com as permissões do proprietário (root), independentemente de quem o executa.
No entanto, após criar as variáveis de ambiente:
<img src = "imagens/Captura de ecrã 2023-10-13, às 18.45.38.png">
estas são apenas definidas no terminal do utilizador uma vez que, quando executamos o programa Set-UID, a variável LD_LIBRARY_PATH não aparece.
<img src = "imagens/Captura de ecrã 2023-10-13, às 18.46.33.png">
<img src = "imagens/Captura de ecrã 2023-10-13, às 18.47.56.png">

Variáveis de ambiente como LD_LIBRARY_PATH podem ser usadas para manipular o comportamento de bibliotecas dinâmicas. Num contexto Set-UID, isso pode ser uma séria vulnerabilidade de segurança se permitido. Portanto, ao definir variáveis de ambiente em programas Set-UID, o sistema frequentemente limpa ou redefine variáveis como LD_LIBRARY_PATH para evitar que um usuário mal-intencionado substitua bibliotecas do sistema por outras versões.

