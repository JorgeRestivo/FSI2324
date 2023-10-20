
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

Com as informações recolhidas, agora somos capazes de criar o ficheiro "badfile" com o conteúdo que queremos inserir no buffer.

#### Em que consiste este ataque?
O ataque consiste em preencher o ficheiro "badfile" com 517 bytes de instruções NOP (sem operação). Em seguida, calcula-se o tamanho do shellcode (27 bytes) e injetamo-lo no ficheiro, 27 bytes antes do final. Utilizando o debugger gdb, descobre-se o endereço de memória do ponteiro da função bof() através do comando "$ebp". Com essa informação, calcula-se o endereço onde o endereço de retorno é armazenado, subtraindo 4 bytes. Manipula-se esse endereço para apontar para um local no código antes da injeção do shellcode. As instruções NOP permitem que a CPU avance até encontrar e executar o shellcode malicioso.


Recorremos ao ficheiro "exploit.py" mas ainda precisamos de alterar algumas informações:

1º- Na variável shellcode inserimos o shellcode em 32-bits que executa uma shell:
<img src="imagens/Captura de ecrã 2023-10-20, às 20.58.29.png">

2º- O buffer tem um tamanho de 517 bytes e o shellcode tem um tamanho de 27 bytes. Como vamos colocar o nosso shellcode no final do buffer, então 517 - 27 = 490.

3º- Este é o endereço para o qual queremos que o nosso shellcode aponte. Sabemos que o buffer começa em 0xFFFFCA7C (obtivemos esta informação através do debbuger gdb e do comando "&buffer") e tem um tamanho de 517 bytes. Colocamos o nosso shellcode 27 bytes antes do final, ou seja, em 0xFFFFCA7C + 1EA (490 em hexadecimal) e assim obtivemos o endereço: 0xFFFFCC66.

4º- Sabemos que o retorno que queremos modificar está após o ebp, e também sabemos que o ebp tem um tamanho de 4 bytes. Utilizando os 2 endereços obtidos no debug, calculou-se a localização do endereço de retorno relativamente ao inicio do array (offset): 0xFFFFCAE8 - 0xFFFFCA7C + 4 (tamanho do ebp) = 112.

<img src="imagens/Captura de ecrã 2023-10-20, às 21.06.23.png">

Executamos o ficheiro "exploit.py" para criar o "badfile" necessário para usar o exploit.
Conseguimos completar com sucesso esta tarefa pois obtivemos acesso à root shell.

<img src="imagens/Screenshot from 2023-10-20 12-58-17.png">