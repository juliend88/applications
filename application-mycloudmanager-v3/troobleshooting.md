# Ma stack est montée mais je n'arrive pas à mettre en place la connexion VPN, que faire ?

Si vous êtes dans ce cas, vous avez probablement été trop impatient !

Une (petite) attente de 10 minutes est nécessaire pour que l'ensemble du cluster MyCloudManager soit initialisé et que toutes les images docker soient téléchargés et configurés. Donc, quand la stack heat est terminée et que toutes les ressources sont “vertes”, attendez vraiment 10 minutes avant d'initialiser votre connexion VPN.


# Ma stack est montée, mon accès VPN en place mais je n'arrive pas à accéder à MyCloudManager, que faire ?

Si votre MyCloudManager est bien actif, que vous êtes bien connecté au VPN, mais que vous n’arrivez pas à accéder à l’adresse http://10.1.1.10:30000 essayer avec une autre adresse d'un des membres du cluster (ex: http://10.1.1.11:30000 ou http://10.1.1.12:30000).

# Je ne peux pas accéder aux applications de MyCloudManager, que faire ?

il faut se connecter en ssh sur une des instances du cluster, et taper la commande `kubectl get pods` afin de vérifer l'état des pods pourtant l'ensemble de vos applications,

![pods](imgtrooble\pods.png)

si un ou des pods ne sont pas en état `running` au bout de 5min après le lancement de votre application c'est que celui ci à rencontré un problème à son lancement. Pour résoudre le problème il suffit de supprimer le pod en question avec la commande `kubectl delete pods PODNAME` exemple `kubectl delete pods app-zabbix-4280307708-qqf8n`. kubernetes va ensuite s'occuper de le relancer.

# Mes instances ne s'enrôlent pas dans MyCloudManager, que faire ?

Si vos nouvelles instances n’apparaissent pas dans MyCloudManager, vérifiez que vous avez bien inclut le security group de votre stack MyCloudManager dans votre instance. Attention aussi aux aspects réseaux: vos instances doivent pouvoir communiquer avec votre MyCloudManager pour être instrumentalisées. Afin que les playbooks Ansible puissent s’installer correctement, il faut que l’utilisateur cloud soit présent sur la machine. Pour info, celui-ci est présent sur l’ensemble des images fournies par Cloudwatt.

# Comment utiliser les applications de MyCloudManager ?

Nous consacrons une page web dédiée pour chaque application open source composant MyCloudManager. Vous en trouverez l’accès ici https://www.cloudwatt.com/fr/labs/mycloudmanager.html dans la section “Applications”.

# J'ai une demande d'évolution, qui contacter ?

Vous pouvez contacter directement l’équipe par mail apps.cloudwatt@orange.com. Nous vous répondrons dans les meilleurs délais.

# Qu'est-ce que MyCloudManager ?

L’application MyCloudManager est une innovation de l’offre Cloudwatt d’Orange Cloud for Business permettant d’administrer au jour le jour vos instances avec un ensemble d’outils open source leaders sur leurs domaines respectifs. Ainsi, MyCloudManager inclut :

Du monitoring, de la supervision et de l’alerting

Du log management de vos instances

De la planification de taches d’administration

Un miroir antivirus

Un gestionnaire de répertoires applicatifs

Du backup full ou incrémental

Enfin, un synchroniseur de temps

# Quelles distributions Linux sont supportées par MyCloudManager ?

MyCloudManager permet d’instrumentaliser les instances Linux configurées avec les distributions suivantes ?

Ubuntu 16.04

Ubuntu 14.04

Debian Jessie

Debian Wheezy

CentOS 7.2

CentOS 7.0

CentOS 6.7

Nous avons testé MyCloudManager avec les distributions standards fournies par Cloudwatt. Nous ne pouvons garantir le bon fonctionnement avec des images de distributions importées.

# Quelles distributions Windows sont supportées par MyCloudManager ?

MyCloudManager ne permet pas l’instrumentalisation d’instances Windows.

# Quels sont les login/motdepasse par défaut des applications ?

Nous laissons la possibilité via le bouton Settings présent sur chaque vignette de saisir l’ensemble des paramètres applicatifs au lancement du conteneur. Cependant le login et le mot de passe ne pouvant pas être changés partout, il faudra le faire à l’intérieur de l’application une fois celle-ci lancée.

Les login et mot de passe par défaut des applications MyCloudManager:

Zabbix - Login : Admin - Mot de passe : zabbix

Graylog - Login : admin - Mot de passe : admin

Rundeck - Login : admin - Mot de passe: admin

Les autres applications n’ont pas d’interface web, donc pas de login/mot de passe, excepté Artifactory qui n’a pas d’authentification.

# J'ai lancé ma stack mais je ne vois rien après 15 minutes sur la page http://10.1.1.10:30000

Connectez vous en ssh sur une des instances du cluster soit via son IP privée si vous êtes connecté en VPN sinon via une des IP publique fournies en output de la stack.

Une fois sur l'instance tapez la commande suivante `fleetctl list-units`.

*Fleet est un des composant de notre stack qui permet à l'ensemble des machines composant le cluster de se connaitre et de lancer l'ensemble des services qui compose MyCloudManager.*

Si l'ensemble des services n'est pas en état `running` comme sur l'image si dessous, c'est que votre cluster à rencontré un problème à son lancement.
La solution la plus efficace mais certes radicale est de redémarrer l'ensemble des machines du cluster soit via la console soit via l'invite de commande.

![fleet](imgtrooble\fleet.png)

Une fois que l'ensemble des services sont en running vous pouvez taper la commande `kubectl get pods` et vous verrez que les pods permettant de fonctionnement de MyCloudManager seront en création.

![podsmcm](imgtrooble\podsmcm.png)
