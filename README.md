# Configurando Postfix com Gmail

1. Abra o terminal e digite o seguinte comando para instalar o Postfix e os complementos:
 ```ruby
sudo apt install postfix  mailutils ca-certificates mutt
```

Durante a instalação, você será solicitado a selecionar o tipo de configuração. Selecione "Site da Internet" e digite o nome de domínio do seu servidor quando solicitado, para uma configuração básica, tanto faz o nome do dominio.

2. Obtenha uma Senha de Aplicativo do Google:
Em vez de usar a senha que você usa para entrar na sua caixa de entrada do Gmail, você deve utilizar o recurso de "Senha de Apps" do Google.

3. Crie o arquivo /etc/postfix/sasl_passwd e adicione o seguinte conteúdo:
``` ruby
[smtp.gmail.com]:587 seu_email@gmail.com:sua_Senha_de_Apps_do_gmail
```

4. Faça um backup do arquivo de configuração /etc/postfix/main.cf:
``` ruby
mv /etc/postfix/main.cf /etc/postfix/main.cf.bkp
```

5. Crie um novo arquivo /etc/postfix/main.cf em um editor de texto. Adicione as seguintes linhas:
 ```ruby
# SMTP relayhost
relayhost = [smtp.gmail.com]:587

# Configurações TLS
smtp_tls_loglevel = 1
smtp_use_tls = yes
smtpd_tls_received_header = yes

# TLS
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_sasl_tls_security_options = noanonymous
```  

6. Execute o seguinte comando no terminal para criar o arquivo de hash para sasl_passwd:
``` ruby
sudo postmap /etc/postfix/main.cf ; sudo postmap /etc/postfix/sasl/sasl_passwd 
```

7. Reinicie o Postfix digitando o seguinte comando no terminal:
```ruby
sudo systemctl restart postfix
```
8. Realize um teste enviando um e-mail do Gmail. Utilize o comando mutt para enviar um e-mail de teste com o seguinte conteúdo:
```ruby
echo 'Corpo do Email' | mutt -s 'Assunto' seu_email@gmail.com
```

## Veja também o meu script de automatização para este procedimento:
https://github.com/hyagoshodan/Hermes
