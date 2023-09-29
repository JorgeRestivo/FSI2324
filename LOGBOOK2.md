# Trabalho realizado nas Semanas #2 e #3

## Identificação

- A CVE-2020-0601, também conhecida como "Windows CryptoAPI Vulnerability" é uma falha de segurança que afeta a validação de certificados digitais no Windows CryptoAPI que permite a falsificação de certificados.
- Afeta sistemas Windows, incluindo várias versões do Windows 10, que utilizam a CryptoAPI.

## Catalogação

- A vulnerabilidade foi relatada pela Microsoft por pesquisadores de segurança sendo divulgada em janeiro de 2020.
- Devido ao seu potencial impacto na segurança esta vulnerabilidade foi classificada como crítica.

## Exploit

- Existe uma “spoofing vulnerability” na maneira como o Windows CryptoAPI valida certificados de criptografia de curva elíptica (ECC).
- Um invasor poderia explorar esta vulnerabilidade usando um certificado de assinatura de código falsificado para assinar um executável malicioso, fazendo parecer que o arquivo era de uma fonte legítima e confiável.
- O usuário não teria como saber que o arquivo era malicioso pois a assinatura digital era semelhante à de um provedor confiável.

## Ataques

- Não há registo de ataques bem sucedidos, no entanto, se esta vulnerabilidade fosse amplamente explorada, poderia permitir que os invasores conduzissem ataques man-in-the-middle e acedessem a informações confidenciais sobre as conexões do usuário com o software afetado.
