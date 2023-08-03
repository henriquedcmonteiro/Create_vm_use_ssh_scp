<h2 p align="center" > Criando VMs, utilizando conexão SSH e cópias de arquivos entre redes através do SCP. </h2></p>

<i> <p align="center"> Para este exemplo utilizarei a Virtual Box da Oracle.</p>

## Passo 1 - Baixando a ISO.
<p>- Para o nosso exemplo, utilizaremos a ISO CentOS 7.9.2009-Minimal. <br>
- http://mirror.ufscar.br/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso </p>

        
## Passo 2 - Criando e configurando uma VM.

![Passo 1](imgs/Passo_1.jpg)

<p> - Selecione a opção seguinte pelo mouse ou pelo atalho Ctrl+N. </p><br>

![Passo 2](imgs/Passo_2.jpg)
        
<p>- Escreva o nome da sua ISO da forma que preferir e selecione a imagem da mesma, buscando-a no diretório que ela foi baixada, no meu caso "C:/Users/Henrique/Downloads/CentOS-7-x86_64-Minimal-2009.iso".

<p> - Marque a caixa "Skip Unattened Installation" para pular configurações mais detalhadas da instalação. </p><br>

![Passo 3](imgs/Passo_3.jpg)

<p>- Selecione o hardware da sua VM conforme a capacidade da sua máquina. </p><br>

![Passo 4](imgs/Passo_4.jpg)

<p> - Faça o mesmo com o armazenamento, não precisamos mais do que alguns GBs de espaço. </p><br>

![Passo 5](imgs/Passo_5.jpg)

</p> - Após finalizar a configuração primaria voltaremos a tela inicial da VM, clique no ícone da engrenagem ou utilize o atalho Ctrl+S. </p> 

<p> - Selecionaremos na aba de Rede a opção Placa em modo <b>Brige</b>. </p>

<p> - Caso exista dúvida sobre os tipos de conectores vou deixar uma breve explicação sobre as conexões mais comuns das VMs. </p>

<p> - Somente host : a VM receberá um IP, mas só estará acessível na caixa em que a VM está sendo executada. Nenhum outro computador pode acessá-lo. </p>

<p> - NAT : assim como a sua rede doméstica com um roteador sem fio, a VM será atribuída em uma sub-rede separada, como 192.168.6.1 é seu computador host e a VM é 192.168.6.3 , sua VM pode acessar a rede externa como seu host, mas sem acesso externo à sua VM diretamente, ela está protegida. </p>

<p> - Bridged : sua VM estará na mesma rede que seu host, se seu IP de host for 172.16.120.45 , sua VM será como 172.16.120.50 . Ele pode ser acessado por todos os computadores na sua rede host. </p><br>

## Passo 3 - Inicializando a VM

<p> - Aqui começaremos a utilizar a nossa VM recém criada, mas antes precisamos configurar mais alguns detalhes. </p>

![Passo 6](imgs/Passo_6.jpg)

<p> - Inicie a sua VM em modo normal para ter acesso a CLI. </p>

![Passo 7](imgs/Passo_7.png)

<p> - Escolhemos uma ISO sem interface gráfica. </p>

<p> - Selecione a opção de instalação da ISO através do teclado. </p>

<p> - Após uns instantes (dependendo da sua máquina) a instalação te levara a tela de seleção de idioma, selecione o idioma desejado e avance a instalação. </p>

![Passo 8](imgs/Passo_8.png)

<p> - Selecione a partição criada anteriormente. </p>

![Passo 9](imgs/Passo_9.png)

<p> - Clique no botão finalizado no canto superior esquerdo após selecionar o disco. </p>

![Passo 10](imgs/Passo_10.png)

<p> - Clique na opção de Rede & Nome do Host. </p>

![Passo 11](imgs/Passo_11.png)

<p> - Clique no interruptor para ativar sua conexão, recebendo as configurações de rede, como ip, mask e dns padrão. </p>
<p> - Caso pulasse esta etapa teria de ser configurado manualmente no sistema através do comando <b>nmtui</b> (confira o comando caso se sinta curioso). </p>

![Passo 12](imgs/Passo_12.png)

<p> - Estamos quase lá, caso deseje crie um usuário e depois atribua suas permissões no <b>/etc/sudoers</b> </p>
<p> - Finalizado as configurações, desligue sua VM, pois iremos clonar ela para utilizarmos uma conexão <b>SSH</b>. </p>

![Passo 13](imgs/Passo_13.png)

<p> - Clique com o botão direito no ícone da ISO criado ou utilize o atalho Ctrl + O para o mesmo destino.
<p>- Utilize o nome que desejar, neste exemplo utilizaremos <b>DB CentOS 7 </b>. </p>
<p> - Clique no botão próximo e selecione a opção soft link, para clonar uma ISO em cima do espaço de armazenamento da nossa ISO original, evitando a necessidade de se criar outro disco de armazenamento. </p>

## Passo 4 - Conexão SSH entre VMs

<p> - Ligue ambas as VMs em modo normal. </p> 
<p> - Faça login com as credências que você criou. </p>

![Passo 15](imgs/Passo_15.png)

<p> - Utilize o comando <b>ip a ou ip addr</b> para identificar o ip setado para a VM. </p>
<p> </p>- Note os retângulos em vermelho, o primeiro campo identificado pelo número 1: seguido de lo: com o <b>ip 127.0.0.1/8 que é o ip de loopback</b>. </p>
<p> - Abaixo temos a nossa placa de rede identificada como 2: <b> enp0s3 com o ip abaixo de 192.168.0.113/24</b> que é o ip da nossa VM por onde poderemos fazer a conexão SSH. </p>

![Passo 16](imgs/Passo_16.png)

<p> - Verifique se o servidor SSHD está ativo através do comando <b>systemctl status sshd ou service sshd status</b>. Caso na sua VM o serviço não esteja ativo, ative-o através do <b>systemctl start sshd</b>. </p>
<p> - Caso queira deixar o serviço ativo após o reboot set a opção: <b>systemctl enable sshd</b>. </p>

![Passo 17](imgs/Passo_17.png)

<p> - Para acessar a outra VM antes devemos saber o ip dela utilizando o comando <b>ip a ou ip addr show</b>, neste caso o ip é 192.168.0.112. </p>
<p> - Para fazer a conexão SSH basta utilizar o comando ssh, seguido do usuário da vm que quer acessar separando o ip do nome pelo carácter <b>"@"</b>. </p>
<p> - Desta forma o acesso a outra VM ficou sendo <b>ssh monteiro@192.168.0.112</b>. Aperte enter e o terminal pedira a senha, após efetuar o login você estará dentro da outra VM. </p>

![Passo 18](imgs/Passo_18.png)

<p> - Podemos ver que na VM que estamos logados não existe arquivo nenhum na home do usuário monteiro. </p>
<p> - Cheque o diretório home com o comando <b>ls "/home/"usuário"/". </b></p>
        
## Passo 5 - Copia de arquivos entre VMs através do SCP

<p> - Agora utilizaremos o comando <b>SCP que deriva da junção do SSH + COPY </b>, semelhante ao cp, mas que faz a cópia através da <b>conexão SSH</b>. </p>

![Passo 19](imgs/Passo_19.png)

<p> - Vamos sair desta VM com o comando <b>exit</b>. Após o logout vamos criar um arquivo de texto vazio com o comando <b>touch</b> na nossa home. </p>
<p> - Agora através do comando SCP vamos fazer a cópia deste arquivo para a nossa outra VM. </p>

![Passo 20](imgs/Passo_20.png)
     
<p> - Semelhante ao comando cp, nós estamos copiando o arquivo texto do diretório atual que estamos sem precisar passar o caminho absoluto que seria /home/henrique/arquivo.txt </p>
<p>- Utilizando o comando scp arquivo.txt monteiro@192.168.0.112:/home/monteiro nós estamos fazendo o seguinte: </p>
        
<p> - Primeiro copiamos o arquivo do diretório atual, depois passamos a conexão ssh que desejamos, neste caso <b>monteiro@192.168.0.112</b>, utilizamos a sintaxe: logo após ao último digito para concatenar a conexão ssh com o próprio diretório absoluto que neste caso é o /home/monteiro </p>

<p> - Desta forma podemos acessar novamente a outra Vm através do SSH e ver a cópia do arquivo. </p>

![Passo 21](imgs/Passo_21.png)

<p> - Utilizando o pwd para mostrar o diretório que estamos e o comando ls para mostrar o conteúdo do mesmo, notamos que o arquivo foi copiado com sucesso. </p>




        













        














    
        
