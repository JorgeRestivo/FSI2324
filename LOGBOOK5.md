# Trabalho realizado nas Semanas #4 #5 e #6
## SEED Labs - Buffer-Overflow Attack Lab (Set-UID Version)

### 2 Environment Setup
### 2.1 Turning Off Countermeasures

Começamos por desativar a funcionalidade de randomização de espaço de endereço (de pilha e do heap) de forma a facilitar a tentativa de adivinhar os endereços exatos e posteriormente alteramos a shell vinculada a /bin/sh, de modo que a shell executada permita ataques a programas Set-UID.

<img src="imagens/Screenshot from 2023-10-20 11-14-04.png">


### 3 Task 1: Getting Familiar with Shellcode

Após compilar e executar o código, obtivemos acesso ao shell de root tanto nas versões de 32 bits (arquivo a32.out) quanto nas de 64 bits (arquivo a64.out). O código copia um shellcode para a pilha (no array char code[500]) e, ao forçar sua interpretação como um inteiro e chamá-lo com a função func(), conseguimos acessar o shell de root.

<img src ="imagens/Screenshot from 2023-10-20 11-14-56.png">

### 4 Task 2: Understanding the Vulnerable Program

O programa "stack.c" tem uma vulnerabilidade de buffer overflow. Primeiro, lê uma entrada de um ficheiro e, em seguida, passa esta entrada para outro buffer na função bof(). A entrada original pode ter um comprimento máximo de 517 bytes, no entanto, o buffer em bof() tem apenas 100 bytes de comprimento. Como a função strcpy() não verifica os limites, ocorre um buffer overflow. É importante notar que o programa recebe a sua entrada de um ficheiro chamado badfile. O nosso objetivo é criar o conteúdo para o badfile de forma a que, quando o programa vulnerável copiar o conteúdo para o seu buffer, seja iniciado um shell de root.

<img src="imagens/Screenshot from 2023-10-20 11-25-57.png">

Para compilar, desativamos o StackGuard e as proteções de pilha não executável. Após a compilação, é necessário transformar o programa num programa Set-UID de propriedade do root.


### 5 Task 3: Launching Attack on 32-bit Program (Level 1)

Nos passos dados, identificamos e exploramos uma vulnerabilidade de buffer overflow em um programa. Utilizamos o GDB para estabelecer um ponto de interrupção na função bof() e observamos o comportamento dos registos durante a execução do programa.

<img src="imagens/Screenshot from 2023-10-20 11-31-20.png">

Obtivemos informações cruciais, incluindo os valores dos registradores EBP e o endereço do buffer, essenciais para criar um payload de exploração bem-sucedido. Preparamos um arquivo chamado badfile para fornecer entrada ao programa durante a execução. 

<img src="imagens/Screenshot from 2023-10-20 11-31-54.png">
<img src="imagens/Screenshot from 2023-10-20 11-32-15.png">
<img src="imagens/Screenshot from 2023-10-20 11-32-31.png">

Com as informações recolhidas, agora somos capazes de criar o ficheiro "badfile" com a ajuda do "exploit.py". No entanto, primeiro precisamos alterar algumas informações nele:

<img src="imagens/Captura de ecrã 2023-10-20, às 20.58.29.png">
<img src="imagens/Screenshot from 2023-10-20 12-56-50.png">
<img src="imagens/Screenshot from 2023-10-20 12-58-17.png">