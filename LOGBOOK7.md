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

## CTF Semana #7 (Format String)

### Desafio 1 

Inicialmente exploramos os ficheiros disponibilizados na plataforma CTF que são os mesmos que estão a ser executados no servidor na porta 4004.

Com o comando checksec verificamos que program (main.c compilado) não tem o binário randomizado mas existem proteções do endereço de retorno usando canários.

<img src="imagens/Screenshot from 2023-11-24 17-15-37.png">

Em seguida avaliamos como funciona o main.c. Podemos ver que o input e guardado num buffer de 32 bytes através da função scanf e depois volta a ser impresso por printf. Assim podemos usar o "format string attack" já que não há randomização dos endereços.

A função load_flag quando invocada lê a flag que se encontra no diretório para outro buffer que é uma variável global logo está alocado no Heap.

Para descobrir o endereço da função,usamos o debugger gdb e assim podemos retornar o valor da string do buffer.

<img src="imagens/Screenshot from 2023-11-24 17-21-32.png">

O resultado foi o endereço de retorno "0x08049256", que é "\x60\xC0\x04\x08" em formato de string.

Modificando o programa em python injetamos com a nossa string para termos o valor da string do buffer:

<img src="imagens/Screenshot from 2023-11-24 17-49-42.png">

<img src="imagens/Screenshot from 2023-11-24 18-19-05.png">

Ao executar conseguimos obter a flag, que constava na flag.txt.

## Desafio 2 

Após avaliar o novo program que continua sem randomização de endereços, avaliamos o funcionamento do código main.c. 

<img src="imagens/Screenshot from 2023-11-24 18-20-38.png">

Olhando para o código fornecido, podemos ver que desta vez não temos uma função que já abra nosso código, porém se conseguirmos definir key = 0xBEEF podemos executar o BASH e abri-lo nós mesmos.

Para manipular o valor de key, que é uma variável global e por isso está alocada na Heap, recorremos à format string attack do input através da opção '%n'. Inicialmente precisamos do valor do endereço da variável key, e para isso recorremos ao gdb:

<img src="imagens/Screenshot from 2023-11-24 18-20-59.png">

<img src="imagens/Screenshot from 2023-11-24 18-21-23.png">

O resultado foi o endereço "0x0804b324" que é "\x08\x04\xB3\x24" em hexadecimal.

Sabemos onde a chave está localizada. Podemos usar scanf a nosso favor (mais uma vez), podemos colocar o endereço onde a chave está localizada em nossa string e passá-la 0xBEEF usando '%n". Mas há um pequeno problema:

'%n' escreve o número de caracteres que escrevemos até agora em nossa string.

Para resolver isso podemos usar '%.Ax" (onde A é a quantidade de caracteres que queremos ler uma var) e definir o tamanho que desejamos.

Como '%x' é lido de um endereço, devemos ter isso em mente. Então, podemos formatar nossa string para ficar assim:

endereço para %x + endereço para %n + '%.Ax' + '%n'

Só temos que lembrar que escrever os endereços leva alguns caracteres de tamanho, então temos que subtraí-los de 0xBEEF.

Calculando A:

Agora que sabemos como escrever nossa string, temos que calcular 'A':

0xBEEF = 48879

Sabemos que escrevemos 8 caracteres até agora para o nosso endereço, o que nos deixou com 48879 - 8 = 48871.

Este é o número pelo qual devemos substituir 'A'.

Mudamos o código python do desafio anterior e utilizamos aqui também.

<img src="imagens/Screenshot from 2023-11-24 18-30-54.png">

Depois do executar podemos ver a flag:

<img src="imagens/Screenshot from 2023-11-24 18-32-41.png">

Assim conseguimos obter acesso a flag abrindo a flag.txt depois de executar o código python.