# Trabalho realizado nas Semanas #7 #8 e #9

- Primeiro, começamos por correr o servidor e testar se está a funcionar. Depois do servidor estar carregado usamos o comando: 
```bash
$ echo hello | nc 10.9.0.5 9090"
```
- Depois pressionamos ctrl+c para acabar. Podemos ver que o servidor está a correr bem pois obtivemos a mensagem "Returned properly".

<img src="imagens/Screenshot from 2023-11-17 17-17-26.png">

## Task 1

- Para crashar o servidor bastou inserir a seguinte string de input:

```bash
$ echo '%s' | nc 10.9.0.5 9090
```

<img src="imagens/Screenshot from 2023-11-17 17-18-38.png">

- Como podemos ver não obtivemos desta vez a mensagem "Returned properly", confirmando assim que o servidor crashou.

## Task 2 : Printing Out the Server Program's Memory

### Task 2.A : Stack Data 

- Para imprimir os dados da stack, considerando que temos uma vulnerabilidade de format string na função myprintf, precisamos apenas ecoar para o servidor o parser %08x, para que possamos descobrir o endereço de memória dos valores da stack. Portanto, para identificar exatamente onde começa o buffer inserimos 4 caracteres aleatórios que podem ser facilmente reconhecidos por nós, no nosso caso usamos "ZZZZ" (valor ASCII em hexadecimal = 5A5A5A5A), dessa forma podemos encontrar exatamente o tamanho do buffer.

<img src="imagens/Screenshot from 2023-11-17 17-24-59.png">
<img src="imagens/Screenshot from 2023-11-17 17-25-11.png">

O final "A5A5A5A5" é o endereço da string "ZZZZ" dado como input. Note-se que os bytes constituintes estão invertidos. Entre a string e o codigo ASCII do nosso input existem 504 caracteres e sendo cada endereço 8 caracteres então existem 63 endereços na stack entre a format string e o buffer.

### Task 2.B : Heap Data 

- Para imprimir os dados heap, armazenamos o endereço da variável na mensagem secreta da memória heap, usando a vulnerabilidade de format string, passando o endereço de memória variável para a stack e, em seguida, ecoando %8x (%08x deu erro) 63 vezes, seguido por %s para que ele leia o endereço de memória armazenado e obtenha o valor dele.

<img src="imagens/Screenshot from 2023-11-17 18-01-06.png">
<img src="imagens/Screenshot from 2023-11-17 18-01-27.png">

- A mensagem secreta é: "null".

## Task 3 : Modifying the Server Program's Memory

### Task 3.B : Change the target variable's to 0x5000

- Para mudarmos o valor do target para 0x5000, 20480 em decimal. Sabendo que com '%n' só iremos escrever o número de caracteres até ali, então o input terá de ser 20408 caracteres. Usamos o input de %.19913x para termos todos os caracteres. 19913 foi usado pois 20408 - 64*8 - 63 = 19913

<img src="imagens/Screenshot from 2023-11-17 18-19-49.png">
<img src="imagens/Screenshot from 2023-11-17 18-20-02.png">

- Como observado, o valor de target é agora 0x00005000.