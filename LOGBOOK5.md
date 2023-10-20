# Trabalho realizado nas Semanas #4 #5 e #6
## SEED Labs - Buffer-Overflow Attack Lab (Set-UID Version)

### 2 Environment Setup
### 2.1 Turning Off Countermeasures

Começamos por desativar a funcionalidade de randomização de espaço de endereço (de pilha e do heap) de forma a facilitar a tentativa de adivinhar os endereços exatos e posteriormente alteramos a shell vinculada a /bin/sh, de modo que a shell executada permita ataques a programas Set-UID.

<img src="imagens/Screenshot from 2023-10-20 11-14-04.png">
