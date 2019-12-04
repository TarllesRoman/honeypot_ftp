---
description: >-
  Trabalho realizado pelo aluno Tarlles Roman Sfredo para a disciplina de
  Segurança em Sistemas Computacionais do curso de Sistemas para Internet -
  IFSEMG Barbacena 2019
---

# Honeypot FTP no CentOS 7

Para executar o honeypot disponível em: [https://github.com/TarllesRoman/honeypot\_ftp](https://github.com/TarllesRoman/honeypot_ftp) tenha disponivel o python 2.7 em sua máquina juntamente com o python 3.6. Um breve tutorial de instalçao dessas duas ferramentas encontra-se logo em seguida.

#### Configurando o Python no CentOS

Instale o repositório centos release caso ainda não o possua

```text
# yum install centos-release-scl
```

Instale o Python

```text
# yum install rh-python36
```

{% code title="\# python --version" %}
```text
Python 2.7.5
```
{% endcode %}

Agora instale as ferramentas de desenvolvedor padrões

```text
# yum groupinstall 'Development Tools'
```

#### Instalando a biblioteca Twisted

Para instalar a biblioteca Twisted execute o comando abaixo

```text
# yum install python-pip
# pip install twisted
# pip install pyOpenSSL
```

#### Executando o honeypot

Primeiro clone o repositório

```text
# git clone https://github.com/TarllesRoman/honeypot_ftp.git
```

```text
# cd honeypot_ftp
```

Se você possui as configurações corretas basta executar o comando abaixo e você terá um honeypot ftp executando.

```text
# python ftphoney.py
```

{% hint style="info" %}
O código python original gerará uma saída no arquivo `/root/ftp.log`
{% endhint %}

Agora você pode criar um script shell para executar o comando anterior e habilita-lo utilizando o controlador systemctl dessa maneira sempre que sua máquina for ligada o honeypot estará funcionando. Para habilitar o script criado siga os passos abaixo.

Vamos criar um arquivo de serviço para o nosso programa. Crie como root um arquivo chamado ftphoneyd.service no diretório `/lib/systemd/system.`

```text
# vim /lib/systemd/system/ftphoneyd.service
```

{% code title="@ ftphoneyd.service" %}
```text
[Unit]
Description=FTP
After=network.target

[Service]
Type=simple
RemainAfterExit=yes
ExecStart=/etc/init.d/ftphoneyd.sh

[Install]
WantedBy=default.target
```
{% endcode %}

{% hint style="danger" %}
Seu script deve estar localizado em `/etc/init/`
{% endhint %}

Após isso bastar executar os comandos abaixo para iniciar e habilitar seu honeypot.

```text
# systemctl start ftphoneyd
# systemctl enable ftphoneyd
```

