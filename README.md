# zebra_zpl_autoexec_font_lpt_ad
Utilidade pública - Manager Zebra print. Script de inicialização, envio de fonte, instalação na LPT1 em ambiente AD.


# Script de inicialização.

Para que as fontes sejam selecionadas por padrão ao ligar a impressora, é necessário adicionar um arquivo AUTOEXEC.zpl na memória flash da impressora.
 

No painel web, acesse: **Listagem de diretórios**

A Impressora Zebra possuí 2 memórias, RAM e Flash, todos os arquivos de configuração devem estar dentro da memória Flash, dispositivo E:

 ![Title](image/1.png)


**Iremos utilizar as fontes**: TAH000.ttf e TAH001.ttf
 
![Title](image/2.png)
 

 Crie um novo Script na memória Flash integrada.

nome: ativa_fonte.zpl

 ![Title](image/3.png)

```

^XA
^CWL,E:TAH000.TTF
^CWK,E:TAH001.TTF
^XZ

```

Ex:

^XA = Inicia um bloco de comando ZPL.

^CWL,E:TAH000.TTF = Coloca a Identificação L na fonte TAH000.TTF que está localizada em E:

^CWK,E:TAH001.TTF = Mesma explicação. Comando utilizar as fontes marcadas com L ou K

^XZ = Finaliza o bloco zpl

 ![Title](image/4.png)

 **Crie outro script chamado AUTOEXEC.zpl para iniciar o arquivo ativa_fonte.zpl**

AUTOEXEC.ZPL

```
^XA
^XF E:ativa_fonte.zpl^FS
^XZ

```

 ![Title](image/5.png)

Realize os testes após desligar e ligar a impressora.


--------------------------


# Enviando fonts para impressora zebra.


Abra o Zebra Setup Utilites.

Selecione a impressora > Escolha a opção Download Fonts and Graphics

Salve um novo arquivo *.mmf*

![Title](image/6.png)
 
Escolha uma opção de tamanho de Card adequada para sua fonte.

![Title](image/7.png)
 
No campo Fonts, click botão direito e Add...

![Title](image/8.png)
 
Faça o Processo de selecionar a fonte desejada, A fonte precisa está instalada no Windows, caso contrário, irá ter que enviar como arquivo.
 
![Title](image/9.png)![Title](image/10.png)
![Title](image/11.png)![Title](image/12.png)

 

Faça o Download para enviar para impressora.
 
![Title](image/13.png)

Arquivos também podem ser enviados para impressora através do Driver.
Propriedades da impressora > Preferências...> Tools > Action > Send File.

![Title](image/14.png)
 
 -------------------------------------------------

# Utilizando Porta USB como LPT1.

O Host está ingressado no AD, portanto, o usuário não possuí permissão para executar scripts.

1º - Instale o Driver com o usuário Administrador.

2º - Compartilhe a impressora instalada, ex: zebra_compartilhada.

![Title](image/17.png)

Acesse \\\127.0.0.1 para verificar o compartilhamento.

![Title](image/16.png)

Para usar a porta USB como LPT1, podemos utilizar o net use pelo cmd para jogar os dados de lpt1 no compartilhamento zebra_compartilhada.

*net use lpt1: \\\127.0.0.1\zebra_compartilhada /persistent:yes*

**OBS:** Se você utilizar o comando como ADM, a porta lpt1 será atribuída apenas ao seu usuário ADM.

No AD, o usuário comum irá necessitar das credenciais Administradoras para poder conceder acesso na porta LPT.

Para contorna isso, utilize o Agendador de tarefas.

3º - Abra o **taskschd.msc**

Importe o arquivo já configurado: Inicia_ltp1.xml.

![Title](image/15.png)


Presets:

![Title](image/lpt.PNG)![Title](image/lpt2.PNG)![Title](image/lpt3.PNG)![Title](image/lpt4.PNG)

*Deixe o diretório "Scripts" em C:*

O Agendador irá inicializar o script lpt1.bat toda vez que o computador inicializar. O script será executado com permissões de Administrador pelo taskschd.msc.


Teste a porta lpt1. Após o computador ser reinicializado, verifique se consta no "net use".

**Instale uma nova impressora pela LPT1.**

Adicione uma impressora como Local e adicione a porta LPT1.

![Title](image/18.png)![Title](image/19.png)

Para deletar a porta: *net use LPT1: /delete*

OBS: Caso algo de errado, verifique se exista algo que possa atrapalhar os drivers da impressora, exemplo: antivírus ou atualização do Windows quebrada, ex: KB3118754...




