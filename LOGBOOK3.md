## Desafio 1
Após acedermos o endereço http://ctf-fsi.fe.up.pt:5001 encontramos um servidor de Wordpress, o qual foi possível aceder através dos passos: Home > Services > WordpressHosting e, posteriormente, através da "Additional information" obtivemos as informações que nos levariam a resolver parte do problema. São estas:

![](/Users/leticiacoelho/Desktop/FSI/informações wordpress.png)

Para posteriormente encontrarmos a vulnerabilidade associada a este software, utilizamos uma plataforma de base de dados com informações sobre todas as CVE: https://www.exploit-db.com.
Assim, encontramos o CVE 2021-34646, sendo assim flag{CVE-2021-34646} a resposta a este desafio.

## Desafio 2
Para este desafio começamos por descarregar o ficheiro do exploit associado ao CVE 2021-34646 e chamamos-lhe "CVE-2021-34646.py".
Depois, a partir do terminal executamos o comando: python3 CVE-2021-34646.py https://ctf-fsi.fe.up.pt:5001 1, utilizamos o URL do site WordPress pois é este em questão que queremos "atacar" e também o argumento 1 pois é o ID do usuário que queremos aceder (normalmente o ID 1 costuma ser o do administrador).
Obvtivemos esta resposta:

Tentamos logo o primeiro link que nos permitiu autenticar no servidor como administradores.
Posteriormente, acedemos ao link http://ctf-fsi.fe.up.pt:5001/wp-admin/edit.php onde tivemos acesso a uma mensagem privada:

que nos permitiu chegar à resposta do desafio.
