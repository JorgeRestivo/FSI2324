
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

## CTF Semana #5 (Buffer-Overflow)
### Desafio 1

Começamos por examinar os arquivos disponíveis na plataforma CTF, os quais são idênticos aos que estão sendo executados no servidor na porta 4003. Utilizando o comando checksec, analisamos o programa (main.c compilado) e observamos que ele possui arquitetura x86. 
Além disso, notamos que o binário não está sujeito a randomização, não contém proteções como canários para o endereço de retorno e também não apresenta proteções de execução na pilha.

<img src="imagens/Screenshot from 2023-10-27 17-33-25.png">

Depois avaliamos o código do main.c . Observamos que há uma alocação de 8 bytes de memória para a variável "meme_file," que armazena o nome do arquivo, e 32 bytes para a variável "buffer," que guarda a resposta do usuário.


<img src="imagens/Screenshot from 2023-10-27 17-34-02.png">

Também observamos que a função scanf permite a cópia de até 40 bytes provenientes da entrada padrão (stdin) para o buffer previamente declarado. Isso implica que em situações em que a entrada excede 32 bytes, pode ocorrer uma sobrecarga do buffer.

<img src="imagens/Screenshot from 2023-10-27 17-34-16.png">

Conforme o funcionamento da pilha, a região de memória alocada é contígua e depende da ordem de declaração das variáveis. Assim, se ultrapassarmos a capacidade do buffer, acabaremos por sobrescrever a área de memória destinada à variável "meme_file." Dado que as instruções subsequentes no arquivo main.c exibem o conteúdo de "meme_file," é de nosso interesse sobrescrever o nome do arquivo a ser lido, a fim de revelar o conteúdo do arquivo "flag.txt."

No programa python disponibilizado, na zona em que injetamos o conteúdo para o servidor, bastou escrever 32 caracteres seguidos do nome do ficheiro que queremos ler.

```
r.sendline(b"xxxxxxxxxxxxxxxxxxxxxxxxxxxxflag.txt")
```

Ao executar conseguimos ter acesso ao conteúdo do ficheiro flag.txt e à flag do desafio:

<img src="imagens/Screenshot from 2023-10-27 17-45-21.png">

### Desafio 2

Da mesma forma que no primeiro desafio, decidimos avaliar as medidas de segurança do programa em execução no servidor. O programa mantém sua arquitetura x86, seu binário não é randomizado e não possui proteções contra sobrescrição do endereço de retorno usando canários, nem proteções de execução na pilha.

Em seguida, analisamos o código-fonte do arquivo main.c e notamos que existem alocações de memória: 9 bytes para a variável "meme_file," 4 bytes para a variável "val," e 32 bytes para a resposta do usuário, chamada "buffer."

<img src="imagens/Screenshot from 2023-10-27 18-13-45.png">

Verificamos que a técnica a usar é semelhante ao desafio anterior, no entanto o conteúdo do ficheiro só era mostrado quando o valor em val fosse 0xfefc2324.

O ouput inicial do programa é sempre o mesmo "0xdeadbeed" e revela o valor inicial de val declarado. 

<img src="imagens/Screenshot from 2023-10-27 18-55-03.png">

Com base nessa informação, conseguimos reconstruir os bytes necessários para que o valor de "val" seja igual a 0xfefc2324.

```
\xef\xbe\xad\xde" > 0xdeadbeef
0xfefc2223 > "\x24\x23\xfc\xfe
```
No programa Python fornecido, na parte em que inserimos o conteúdo para enviar ao servidor, foi suficiente escrever 32 caracteres seguidos pelo novo valor de "val" e o nome do arquivo que desejamos ler.

```
r.sendline(b"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\x24\x23\xfc\xfeflag.txt")
```

Ao executar conseguimos ter acesso ao conteúdo do ficheiro flag.txt e à flag do desafio:

<img src="imagens/Screenshot from 2023-10-27 18-28-43.png">
