# Vagrant devbox - Guide d'utilisateur

Une machine virtuelle Vagrant permet � un d�veloppeur de disposer d'un environnement de d�veloppement augment� pour tester le d�ploiement de ces services localement avec des technologies similaires � celles pr�sentes en production (Kubernetes, Docker, Docker Registry, Consul...).

Les objectifs de ce dispositif sont multiples:
* Am�liorer le rendement de d�veloppement en r�duisant la dur�e des it�rations d�v/build/deploy.
* Eviter de r�server des machines sur le cloud ou on-premise � l'usage de chaque d�veloppeur ce qui r�duit le co�t en investissement et en maintenance.
* Promouvoir la culture DevOps en donnant au d�veloppeur la possibilit� de se familiariser avec des environnements serveur proches de la production, ce qui augmentera la r�activit� dans l'environnement de run. 

L'environnement d'ex�cution des services est bas� sur une machine Vagrant incluant les composants suivants:
* OS Centos 7
* Kubernetes
* Docker engine
* Docker registry
* Git
* jq

Des composants suppl�mentaires � installer sur la machine sont pr�sents sur GitLab AccorHotels, ils sont r�cup�rables simplement par `git clone`. 
L'installation de chacun de ces composants est d�crite dans son fichier `readme.md` associ�.  

### Pr�requis

T�l�charger et installer [Virtualbox](https://download.virtualbox.org/virtualbox/5.2.22/VirtualBox-5.2.22-126460-Win.exe)

T�l�charger et installer [Vagrant](https://releases.hashicorp.com/vagrant/2.2.2/vagrant_2.2.2_x86_64.msi)

	NB: Intel VT-x doit �tre activ� dans le BIOS du poste de travail

T�l�charger et installer [Putty](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.70-installer.msi)


### D�marrage de la VM Vagrant

R�cup�rer les fichiers de la VM Vagrant depuis Gitlab via la commande:

```shell
git clone https://github.com/belgadi/vagrant-kubernetes.git
```
	Le r�pertoire doit contenir le fichier Vagrantfile et le r�pertoire .vagrant
		
Se placer dans ce r�pertoire et entrer la commande:

```shell
vagrant up
```
La machine est d�marr�e lorsque les messages suivants (en surlign�) sont affich�s:
![Drag Racing](images/img13.png)

Pour arr�ter la VM:
```shell
vagrant halt
```

Pour supprimer d�finitivement la VM:
```shell
vagrant destroy
```

### Acc�s � la VM en SSH

Une fois la VM d�marr�e, entrer la commande Putty suivante � partir d'un terminal cmd.exe Windows pour vous connecter en SSH � la VM:

```shell
set VAGRANT_PRIV_KEY=<Absolute path to the vagrant PPK private key>

start putty -ssh vagrant@localhost -P 2222 -i  %VAGRANT_PRIV_KEY%
```

	NB: VAGRANT_HOME ci-dessus, correspond au r�pertoire contenant le fichier Vagrantfile et le r�pertoire .vagrant

### Monter un tunnel SSH vers la VM

Il est possible d'utiliser Putty pour ouvrir un Tunnel SSH vers des services s'ex�cutant sur la machine virtuelle.
Pour ce faire, les commandes suivantes doivent �tre ex�cut�e sur un terminal cmd.exe Windows:

```shell
set VAGRANT_PRIV_KEY=<Absolute path to the vagrant PPK private key>
set REMOTE_SERVICE_IP=<IP of the remote service running on K8s>
set REMOTE_SERVICE_PORT=<Port of the remote service running on K8s>
set LOCAL_SERVICE_PORT=<Local port to the service. e.g. the one that will be used on the local web browser>

start putty -ssh vagrant@localhost -P 2222 -i  %VAGRANT_PRIV_KEY% -L %LOCAL_SERVICE_PORT%:%REMOTE_SERVICE_IP%:%REMOTE_SERVICE_PORT%
```

Les commandes suivantes permettent par exemple d'acc�der via http://localhost:8084 sur le PC, � un Pod K8s exposant un port 8080 via son service associ� dont la clusterIP est 10.98.93.27:

```shell
set VAGRANT_PRIV_KEY=<Absolute path to the vagrant PPK private key>
set REMOTE_SERVICE_IP=10.98.93.27
set REMOTE_SERVICE_PORT=8080
set LOCAL_SERVICE_PORT=8084

start putty -ssh vagrant@localhost -P 2222 -i  %VAGRANT_PRIV_KEY% -L %LOCAL_SERVICE_PORT%:%REMOTE_SERVICE_IP%:%REMOTE_SERVICE_PORT%
```

	NB: Il est possible de monter plusieurs tunnels SSH � condition que les ports locaux soient diff�rents (Ex: localhost:8081, localhost:8082...)

### Installation d'un composant dans la VM

# Annexes

## Acc�s � la VM en SSH via Putty en mode graphique

#### Conversion de la cl� priv�e SSH en cl� Putty (*.ppk)

Ouvrir PuttyGen:

![Drag Racing](images/img1.png)

Charger la cl� priv�e SSH g�n�r�e par Vagrant:

![Drag Racing](images/img2.png)

![Drag Racing](images/img3.png)

![Drag Racing](images/img4.png)

Convertir la cl� SSH en cl� Putty (*.ppk)

![Drag Racing](images/img5.png)

![Drag Racing](images/img6.png)

![Drag Racing](images/img7.png)

#### Acc�s en SSH via Putty � la VM Vagrant devbox

Ouvrir Putty:

![Drag Racing](images/img8.png)

Configurer la cl� *.ppk pr�c�demment cr��e:

![Drag Racing](images/img9.png)

![Drag Racing](images/img10.png)

Configurer les propri�t�s de la session SSH: 

![Drag Racing](images/img11.png)

![Drag Racing](images/img12.png)



