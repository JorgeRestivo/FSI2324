# Trabalho realizado nas Semanas #10, #11 e #12
## SEED Labs - Secret Key Encryption 

### Task 1: Frequency Analysis


<img src="imagens/Captura_de_ecra_2023-11-21_as_15.49.48.png">


Após analizarmos o ficheiro "ciphertext.txt", fomos mudando letra a letra conforme a frequência de letras/ bigramas/ trigramas e conforme as palavras que íamos desvendando

<img src="imagens/image.png">

### Task 2: Encryption using Different Ciphers and Modes

Começamos por encriptar o conteúdo do ficheiro ciphertext.txt e criamos um ficheiro cipher.bin para guardar o conteúdo encriptado. Para isso utilizamos a flag -e (encrypt) e o cipher type -aes-128-cfb. 

<img src="imagens/imag1.png">

Para verificarmos que a encriptação foi bem sucedida, utilizamos o comnado inverso, para podermos desencriptar o ficheiro cipher.bin e compararmos ao original. Para isso utilizamos a flag -d (decrypt) e trocamos o ficheiro in para cipher.bin e criamos o ficheiro teste2.txt para receber o conteúdo.

<img src="imagens/imag2.png">

## Task 3: Secret-Key Encryption Lab



