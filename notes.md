
# Fedora Silverblue

## Qu'est Fedora Silverblue

Fedora est une distribution libre de Redhat qui sert de version en amont de Redhat Enterprise Linux.

## Precuseurs

* Fedora Atomic
* CoreOS
* Container Linux
* Fedora

## Particularités

* OS immuable
* Fait pour rouler des containers
* Flatpak Pour les applications graphiques

## OS immuable

* OS pas installé en paquets (yum, apt, dnf)
* Image composé de couches, semblable à une image Docker
* Mises-à-jour atomique
* OSTree gestionnaire d'images

## Comment on fait un OS immuable?

* Il faut séparer entre le immuable et le modifiable
* exemple: `/usr` et `/bin` peuvent être read-only
* `/etc` et `/home` doivent être modifiable

## L'arboresence de fichiers Unix

* Basé sur la stratégie FHS (Filesystem Hierarchy Standard)
* Déjà, UsrMove (depuis 2012) a déplacés les répertoires pour faciliter le boot du réseau, etc:

```
/
|-- etc
|-- usr
|   |-- bin
|   |-- sbin
|   |-- lib
|   `-- lib64
|-- run
|-- var
|-- bin -> usr/bin
|-- sbin -> usr/sbin
|-- lib -> usr/lib
`-- lib64 -> usr/lib64
```
## L'arboresence de fichiers

* Dans Silverblue, il n'y a que deux répertoires systémes modifiables:

```
/
|-- etc *
|-- usr *
|-- run
|-- var
|-- sysroot
```

* `/home` naturellement aussi


## Systèmes de packaging vs systèmes d'images

* Packaging:
  * Versatile, dynamique, accès à beaucoup de logiciels
  * Fonctionne aux niveau des fichiers
  * Gestion d'état simple dasn `/etc` et `/var`
  * Vérification plus compliqué plus la taille augmente
  * Mutation directe du système, pas de retours en arrière

* Systèmes d'images
  * Un état connu, pareil pour tout usager
  * Supporte retour en arriière (rollback)
  * Peut être vérifié avec des outils comme `dm-verity`
  * Opère au niveau de blocks plutôt que de fichiers
  * requiert reboot pour installer une nouvelle image

## OSTree

* Système hybride d'images
* Inspiré par Git
* Mais un checkout c'est par `hardlink`, donc doit être immuable
* Fonctionne en ajoutant des couches pour faire l'arborescence complète
* permet des retours en arrière (rollbacks)
* un repository interne à `/sysroot`, autre repos

## RPM-ostree

* RPM-OSTree est la combinaison RPM/DNF et OSTree.
* permet d'installer de packages RPM
* génère une nouvelle image et on reboot

## RPM-OSTree

* checkout un nouveau système
* modifie le système (avec installations)
* commit le résultat
* reboot pour installer le résultat
* rollback s'il y a un problème


## Flatpak

* installe les applications avec toute leurs dépendances
* roule dans un sandbox (isolé du reste du système)
* un peu sembalble au snap de Ubuntu (mais libre)

## Flatpak

* sert pour installer et rouler des applications graphiques
* installation et destruction facile


## Toolbox

* container ou on peut rouler DNF







## Liens

FHS specs: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

UsrMove policy: https://fedoraproject.org/wiki/Features/UsrMove

Présenation Silverblue français: https://blog.fedora-fr.org/renault/post/Pr%C3%A9sentation-de-Fedora-Silverblue

dm-verity: https://lwn.net/Articles/459420/

OSTree: https://ostree.readthedocs.io/en/latest/

RPM-OSTree: https://rpm-ostree.readthedocs.io/en/latest/
