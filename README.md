# Configurando Postfix com Gmail

1. Abra o terminal e digite o seguinte comando para instalar o Postfix:
 ```ruby
sudo apt-get install postfix
```

Durante a instalação, você será solicitado a selecionar o tipo de configuração. Selecione "Site da Internet" e digite o nome de domínio do seu servidor quando solicitado, para uma configuração básica, tanto faz o nome do dominio.

2. Obtenha uma Senha de Aplicativo do Google:
Em vez de usar a senha que você usa para entrar na sua caixa de entrada do Gmail, é melhor gerar uma senha de aplicativo que possa ser usada no lugar dela.

3. Crie o arquivo /etc/postfix/sasl/sasl_passwd e adicione o seguinte conteúdo:
``` ruby
[smtp.gmail.com]:587 seu_email@gmail.com:sua_senha_de_app_seguros_do_gmail
```

4. Execute o seguinte comando no terminal para criar o arquivo de hash para sasl_passwd:
``` ruby
sudo postmap /etc/postfix/sasl/sasl_passwd
``` 

5. Para proteger a senha que está em texto simples, altere o proprietário e as permissões dos arquivos SASL da seguinte forma:
 ```ruby
sudo chown root:root /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
sudo chmod 0600 /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
```
6. Abra o arquivo /etc/postfix/main.cf em um editor de texto.

7. No final do arquivo, adicione as seguintes linhas:
 ```ruby
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
smtp_sasl_security_options = noanonymous
```

8. Reinicie o Postfix digitando o seguinte comando no terminal:
```ruby
sudo service postfix restart
```
9. Realize um teste enviando um e-mail do Gmail. Utilize o comando sendmail para enviar um e-mail de teste com o seguinte conteúdo:
```ruby
sendmail user@example.com
```
```ruby
De: me@myhost.com 
Para: user@example.com
Olá, esta é uma mensagem de teste.
.
```



