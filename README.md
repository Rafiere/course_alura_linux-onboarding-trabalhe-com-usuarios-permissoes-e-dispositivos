# AULA 01

## CRIANDO USUÁRIOS COM USERADD E ADDUSER

O comando `sudo useradd ricardo2` cria o usuário `ricardo2`.

O comando `sudo adduser ricardo3` pedirá a senha e as informações do usuário `ricardo3`, criando o usuário.

O `adduser` é um script que automatizará a configuração de criação de um novo usuário, configurando a senha do usuário, as informações e etc.

Quando um usuário é criado no Linux, um grupo com o mesmo nome do usuário também é criado. Esse será o grupo **primário** do usuário. Os grupos secundários são os grupos em que vamos inserindo os usuários.

É sempre recomendado criarmos um usuário com o script `adduser`, pois ele criará um diretório `/home` e fará todas as configurações para esse novo usuário.

Na `/home` do novo usuário criado, o script `adduser` copiará os arquivos do diretório `/etc/skel`, nesse arquivo temos o **esqueleto** dos usuários, assim, sempre que um usuário for criado com o comando `adduser`, o conteúdo desse diretório **será copiado para a** `home` **desse novo usuário**.

O `useradd` normalmente é utilizado em scripts, assim, o script fará toda a configuração desses novos usuários. O comando `useradd -m`, por exemplo, permite que criemos o `/home` do usuário ao criar. O `adduser` é só uma forma manual e automatizada de criarmos um usuário.

O `UID` é o identificador de cada usuário no Linux. O `GID` é o identificador de cada grupo no Linux.

O `root` possui o ID `0`. Todos os usuários comuns são criados com IDs acima de `1000`, na maioria das distribuições Linux.

## ADICIONANDO GRUPOS AOS USUÁRIOS

O comando `groups` exibe a quais grupos o usuário pertence.

O comando `su - ricardo3` permite que alteremos de usuário para o `ricardo3`.

O comando `sudo groupadd nome-do-grupo` cria um grupo no sistema.

O comando `sudo usermod -aG nome-do-grupo nome-do-usuario` permite que adicionemos um usuário em um grupo. O parâmetro `-G` significa que estamos adicionando esse usuário em **grupos suplementares**. Se utilizássemos o `-g`, estamos dizendo que queremos trocar o **grupo primário** do usuário, e isso causará muitos problemas no sistema.

O comando `groups nome-do-usuario` permite a visualização dos grupos de um determinado usuário.

O comando `sudo userdel nome-do-usuario` permite que excluamos um arquivo do sistema.

O comando `sudo groupdel nome-do-grupo` permite que excluamos um grupo do sistema.

# AULA 02

## ENTENDENDO O PERMISSIONAMENTO DE ARQUIVOS

Quando utilizamos o comando `ls -l`, temos uma linha como a abaixo.

```
drwxrwxr-x 3 ricardo ricardo 4096 Feb 3 17:32 backup
```

A primeira letra, no exemplo acima, indica se estamos lidando com um diretório, um arquivo ou outro tipo de arquivo.

Um diretório e um arquivo **só podem ter um dono e pertencer a um único grupo**.

Os outros três primeiros caracteres indicam as permissões do usuário que é o dono, dos usuários que pertencem ao grupo do arquivo e dos outros usuários. As permissões estão divididas em **leitura, escrita e execução**.

Em diretórios, a permissão de **execução significa a permissão de acesso**.

Em arquivos, a permissão de execução significa que esse arquivo **é um executável, logo, pode ser executado.**

Temos números que representam permissões, por exemplo:

| Números | Permissões |
| ------- | ---------- |
| 7       | rwx        |
| 6       | rw-        |
| 5       | r-x        |
| 4       | r--        |
| 3       | -wx        |
| 2       | -w-        |
| 1       | --x        |
| 0       | ---        |

Para lembrarmos do exemplo acima, basta definirmos a permissão `r` com o valor `4`, a permissão `w` com o valor `2` e a permissão `x` com o valor `1`, agora, basta somarmos.

Uma permissão `441` significa que o criador do arquivo terá a permissão de leitura, os usuários que estão no grupo do arquivo terão a permissão de leitura, e qualquer outro usuário terá apenas a permissão de execução.

## ALTERANDO AS PERMISSÕES, DONOS E GRUPOS

O comando `sudo chmod 770 /projetos/` fornecerá todas as permissões para o usuário que é o criador do diretório e para os usuários que estão no grupo do usuário que criou o diretório.

O comando `sudo usermod -aG projetos ricardo3` adicionará o usuário `ricardo3` ao grupo `projetos`.

O comando `sudo chown ricardo /projetos` alterará o dono do diretório `/projetos` para o usuário `ricardo`.

O comando `sudo chown ricardo:projetos /projetos` alterará o dono desse diretório para o usuário `ricardo` e o grupo desse diretório para o grupo `projetos`.

## PERMISSIONAMENTOS RESTRITIVOS E ESPECIAIS

Se utilizarmos o comando `chmod o-r proj1`, estaremos retirando a permissão de leitura de todos os outros usuários do arquivo `proj1`.

Para entrarmos em um diretório, devemos ter a **permissão de execução** nesse diretório.

Não entendi completamente a permissão `s`.

O comando `sudo chmod g+s /projetos/` fará com que o grupo do diretório `/projetos/` seja herdada do diretório em que ela está inserida.

## LINKS SIMBÓLICOS E SUAS UTILIDADES

O comando `ln -s /projetos projetos` criará o link simbólico `projetos` que apontará para o `/projetos`. Se executarmos o comando `cd projetos`, seremos redirecionados para o diretório `/projetos`, pois estamos criando um **atalho**.

Ao removermos um link simbólico, apagaremos apenas o link simbólico, e não o diretório em que ele está apontando.

# AULA 03

## GERENCIAMENTO DE PACOTES COM O APT

Quando executamos o comando `sudo apt install nome-do-pacote`, estamos instalando esse pacote em nosso sistema Linux.

O comando `apt search apache2` procurará a string `apache2` nos repositórios que estão adicionados em nosso sistema.

Dentro de sistemas baseados em Red Hat ou Fedora, utilizamos a ferramenta `yum` ao invés do `apt` ou `apt-get`.

O comando `apt show nome-do-pacote` exibe informações sobre um determinado pacote.

## INFORMAÇÕES E UPGRADE DOS PACOTES INSTALADOS

O comando `sudo apt remove nome-do-pacote` remove um determinado pacote do sistema.

O comando `sudo apt autoremove` removerá todas as dependências que não estão sendo utilizadas pelos pacotes do sistema.

O comando `apt list --upgradable` exibe as versões dos softwares que precisamos realizar o upgrade.

O comando `sudo apt upgrade` atualizará todos os pacotes que precisam ser atualizados.

O comando `sudo apt --installed` exibe os pacotes que estão instalados no sistema.

# AULA 04

## INSTALANDO E PARTICIONANDO UM NOVO DISCO

O comando `sudo poweroff` serve para desligarmos a máquina.

O comando `fdisk -l` listará informações sobre os discos, como as partições de cada um dos discos. Um disco pré-instalado deve ser particionado.

Primeiramente, devemos instalar o disco fisicamente, plugando-o na máquina. Após isso, devemos particionar o disco que foi inserido. Para isso, devemos utilizar o comando `sudo fdisk /dev/sdb`, sendo que `sdb` é o novo disco que foi inserido e que ainda não está particionado.

Dentro do `fdisk`, se apertarmos o `p`, exibiremos a tabela de partição.

O comando `n` serve para criarmos uma nova partição.

O primeiro setor do disco é onde vamos começar a escrever no disco. Ele deve estar no `default`, que é o início do disco. Após isso, devemos inserir que ela termina no `default`, ou seja, no último setor do disco.

## INSTALANDO O FILESYSTEM EXT4

Após particionarmos o disco, precisamos instalar o filesystem, ou seja, **precisamos formatar o disco**.

O comando `sudo mkfs -t ext4 /dev/sdb1` serve para instalarmos o filesystem `ext4` na **partição** `sdb1`.

Após instalarmos o disco fisicamente, particionarmos o disco e instalarmos o filesystem, devemos **montar** o disco.

## MONTANDO O DISCO DE FORMA AUTOMÁTICA (:ETC:FSTAB)

O **"montar"** um disco significa **disponibilizarmos esse disco para o usuário**.

O comando `mount` exibe todos os discos que estão montados na aplicação.

O **ponto de montagem** é um diretório. O diretório `/media` é o lugar onde os discos são montados.

Podemos utilizar o comando `sudo mkdir /media/disk2` para criarmos um diretório que servirá como um ponto de montagem.

Para realizarmos a montagem, devemos utilizar o comando `sudo mount /dev/sdb1 /media/disk2`.

Após isso, se utilizarmos o comando `mount`, veremos que o disco está montado. Dessa forma, se realizarmos qualquer operação de leitura ou escrita no `/media/disk2`, essa operação estará sendo realizada no novo disco que foi montado.

Até o momento, se reiniciarmos a máquina, perderemos a montagem, para isso, devemos que, sempre que ligarmos a máquina, **montarmos o disco**.

O arquivo `/etc/fstab` contém os ponteiros dos discos que queremos montar na inicialização do sistema.

O arquivo `/etc/fstab` utiliza o UUID do disco como um identificador.

O comando `sudo blkid` exibirá os IDs que foram gerados para cada um dos dispositivos.

Devemos obter o ID do dispositivo necessário, copiar esse ID

É uma boa prática, sempre que mexermos com arquivos críticos, utilizarmos o comando `cp fstab fstab.bkp` para criarmos um backup desse arquivo antes de realizarmos alterações nele.

Após isso, no arquivo `fstab`, devemos inserir a linha abaixo.

`UUID="id-que-foi-copiado" /ponto-de-montagem sistema-de-arquivos defaults 0 2`, por exemplo:

`UUID="4aaaf0a7-d33c-402f-854d-05dabb3168f4" /media/disk2 ext4 defaults 0 2`.

Dessa forma, sempre que inicializarmos o sistema, essa partição será montada.

## TESTANDO A INICIALIZAÇÃO E O ACESSO AO DISPOSITIVO

Para verificarmos se o arquivo `fstab` foi editado corretamente, evitando problemas na inicialização, podemos utilizar o comando `sudo umount /media/disk2`, para desmontarmos esse dispositivo.

Após isso, podemos utilizar o comando `sudo mount -a` para montarmos todas as partições. Esse é o comando que o sistema operacional utiliza ao inicializar.

Se o arquivo `fstab` for editado com algum erro, esse comando avisará que algo está errado, e, dessa forma, podemos arrumar o arquivo, substituindo-o pelo arquivo de backup.

O comando `sudo reboot` reinicializará o servidor.

# AULA 05

## GERENCIANDO SERVIÇOS

Atualmente, as grandes distribuições utilizam o `systemd`, que é o responsável pelo gerenciamento dos serviços, dos processos e etc. O `systemd` possui a ferramenta `systemctl`. Essa ferramenta permitirá que interajamos com o `systemd`.

Existem alguns outros gerenciadores, como o `init`, o `upstart` e etc.

O comando `systemctl status nome-do-servico` exibe o status de um serviço.

O comando `systemctl start nome-do-servico` serve para inicializarmos um serviço.

O comando `systemctl stop nome-do-servico` serve para interrompermos um serviço.

Tanto o `service` quanto o `systemctl` são ferramentas utilizadas para gerenciar os serviços, só que a ferramenta `service` é mais antiga.

O comando `systemctl disable nome-do-servico` serve para desabilitarmos a inicialização automática de um serviço quando o sistema é inicializado.

O comando `systemctl enable nome-do-servico` serve para que, sempre que a máquina for inicializada, o serviço suba de forma automática.

O comando `service --status-all` exibirá o status de todos os serviços do sistema.
