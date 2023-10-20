# Trabalho realizado nas Semanas #4 #5 e #6
## SEED Labs - Buffer-Overflow Attack Lab (Set-UID Version)

### 2 Environment Setup
### 2.1 Turning Off Countermeasures

Começamos por desativar a funcionalidade de randomização de espaço de endereço (de pilha e do heap) de forma a facilitar a tentativa de adivinhar os endereços exatos e posteriormente alteramos a shell vinculada a /bin/sh, de modo que a shell executada permita ataques a programas Set-UID.

<img src="imagens/Screenshot from 2023-10-20 11-14-04.png">


### 3 Task 1: Getting Familiar with Shellcode

Após compilar e executar o código, obtivemos acesso ao shell de root tanto nas versões de 32 bits (arquivo a32.out) quanto nas de 64 bits (arquivo a64.out). O código copia um shellcode para a pilha (no array char code[500]) e, ao forçar sua interpretação como um inteiro e chamá-lo com a função func(), conseguimos acessar o shell de root.

<img src ="imagens/Screenshot from 2023-10-20 11-14-56.png">