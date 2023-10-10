## Desafio 1
Após acedermos o endereço http://ctf-fsi.fe.up.pt:5001 encontramos um servidor de Wordpress, o qual foi possível aceder através dos passos: Home > Services > WordpressHosting e, posteriormente, através da "Additional information" obtivemos as informações que nos levariam a resolver parte do problema. São estas:

Para posteriormente encontrarmos a vulnerabilidade associada a este software, utilizamos uma plataforma de base de dados com informações sobre todas as CVE: https://www.exploit-db.com.
Assim, encontramos o CVE 2021-34646,

, sendo assim flag{CVE-2021-34646} a resposta a este desafio.

## Desafio 2
