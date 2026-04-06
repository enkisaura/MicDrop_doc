# MicDrop <img src="./ros2_ws/logo.png" title="logo" width="30"/>

<!-- TOC -->
* [Environnement](#environnement)
  * [GitHub](#github)
  * [Les bases de Linux/Ubuntu](#les-bases-de-linuxubuntu)
  * [ROS2](#ros2)
* [Installation](#installation)
* [Noeuds](#noeuds)
  * [ch_01_nodes/camera_node](#ch_01_nodescamera_node)
  * [ch_01_simulation/ahrs_node](#ch_01_simulationahrs_node)
  * [ch_01_simulation/booya_saujon_node](#ch_01_simulationbooya_saujon_node)
  * [ch_01_simulation/gnss_node](#ch_01_simulationgnss_node)
  * [ch_01_simulation/tachometer_node](#ch_01_simulationtachometer_node)
  * [ihm/ihm_node](#ihmihm_node)
  * [rotary_encoder_lgpio/encoder_node](#rotary_encoder_lgpioencoder_node)
  * [telecom/telecom_node](#telecomtelecom_node)
  * [tools/logger](#toolslogger)
  * [ubx/ubx_node](#ubxubx_node)
  * [ubx/ntrip_node](#ubxntrip_node)
  * [watchdog/watchdog_node](#watchdogwatchdog_node)
  * [xsens_mti_ros2_driver/xsens_mti_node](#xsens_mti_ros2_driverxsens_mti_node)
<!-- TOC -->

___
---
# Environnement
## GitHub
> - On ne modifie jamais le code de la branche "main"
> - On ne travaille que sur une branche créée par vous pour vous
> - On ne travaille pas sur la branche de quelqu'un d'autre

GitHub est une plateforme d'hébergement de code. GitHub utilise Git, un logiciel permettant de travailler à plusieurs 
sur le même projet de programmation. Pour utiliser GitHub, il faut installer Git sur votre ordinateur.

MicDrop est hébergé ici : https://github.com/enkisaura/micdrop

### Clone
GitHub permet de cloner le projet MicDrop localement sur un ordinateur.
Plusieurs méthodes existent pour cloner un projet GitHub. Le lien à cloner se cache derrière le bouton vert "<> Code" de
la page d'accueil du projet.

### Issues
L'onglet "Issues" de GitHub regroupe toutes les tâches de programmation à faire.

Pour toute nouvelle tâche, créer une nouvelle Issue en décrivant correctement la tâche.

**Pour travailler sur une tâche:**
1. Cliquer sur l'issue
2. Vous l'assigner (à droite onglet "Assignees")
3. Creer une nouvelle branche liée à l'issue (à droite, onglet "Development" -> "Create a branch")

### Branches
Le projet Git est organisé en "branches". Chaque branche est une version du code.

La branche "main" est la branche principale. Elle contient la version de MicDrop fonctionnelle, débuggée, testée et 
propre. On ne travaille donc pas sur cette branche mais sur une branche perso, créée par vous pour vous.

> "fetch" mets à jour les informations du projet sur votre ordinateur, incluant les nouvelles branches créées

> "Checkout" change de branche de travaille

> "pull" mets à jour le code sur votre ordinateur depuis le code de GitHub

### Commit
A chaque étape de programmation, il faut sauvegarder vos modifications en utilisant un commit.
Un commit permet d'envoyer proprement une selection de fichiers modifiés dans votre branche de travail sur GitHub.
Un commit s'accompagne toujours d'un commentaire décrivant le contenu des modifications.

Une fois le commit créé, il faut l'envoyer sur le serveur de GitHub en utilisant "push".

> "push" envoie vos commits sur GitHub

### Pull request
Une fois votre tâche terminée et votre code débuggé, testé et propre, il faut le mettre dans la branche "main".
La "pull request" permet de fusionner votre branche avec la branche "main".

**Pour mettre vos travaux sur la branche main**
1. Sur l'interface de MicDrop sur GitHub, sélectionner votre branche
2. Contribute -> Open pull request
3. Create pull request
4. Merge

*La bonne pratique dans un projet industriel est que la personne qui fait un pull request doit être différente de la 
personne qui la merge. En pratique, dans notre cas, c'est un peu trop lourd, faites juste gaffe.*

___
## Les bases de Linux/Ubuntu
Pour parler avec Ubuntu, rien de mieux qu'un terminal !
- Pour ouvrir un terminal: *ctrl+alt+t*.
- Dans un terminal, on utilise le langage **bash**.

### Changer de dossier
Dans un terminal, pour changer de dossier, on utilise le logiciel *cd* (Change Directory). Le dossier actuel s'affiche à
gauche de la ligne active du terminal, à côté du nom d'utilisateur
```bash
cd micdrop/rs2_ws
```

On peut ensuite lister le contenu du dossier en utilisant le logiciel *ls* (list).

```bash
ls
```

Tous les capteurs branchés à Ubuntu se retrouvent dans le dossier /dev (devices).

```bash
cd /dev
ls
```

### Mettre à jour Ubuntu
Dans un terminal, pour mettre à jour Ubuntu, il faut:
- Lancer une commande en tant qu'administrateur en utilisant *sudo* (Super Utilisateur DO)
- Utiliser le logiciel de gestion de paquet *apt* (Advanced Packaging Tool, pas la chanson de K-pop)
- Mettre à jour les informations sur les logiciels installés avec l'option **update** de **apt**
- Mettre à jour les logiciels installés avec l'option **upgrade** de **apt**

```bash
sudo apt update
sudo apt upgrade
```

___
## ROS2
ROS (Robot Operating System) est un middleware (une sorte d'OS dans un OS) open-source.
Créé pour la robotique, il permet d'associer efficacement capteurs hétérogènes et traitement des données en temps réél.

### Compiler MicDrop
Même si python est un langage interprété et n'a pas besoin d'être compilé, ROS2 a besoin de packages compilés pour 
tourner.
Pour cela, il faut:
1. Sourcer ros2
2. Ce mettre dans le dossier contenant nos scripts (ros2_ws)
3. **Build** nos paquets en utilisant [colcon](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html)
4. Sourcer nos paquets construits

```bash
source /opt/ros2/$ROS_DISTRO/setup.bash
cd ros2_ws
colcon build
source install/setup.bash
```

Pour chaque nouveau terminal, il faudra sourcer à nouveau ros2 ainsi que les paquets compilés.

L'option "--symlink-install" permet de créer des liens symboliques vers les scripts python. Plus besoin de recompiler à 
chaque modification !

```bash
colcon build --symlink-install
```

### Nodes
Les noeuds sont des programmes (chez nous en python) qui tournent en parallèle.
Un noeud peut être responsable de lire un capteur, d'effectuer une action ou bien de traiter de la donnée.

Les noeuds se trouvent dans des packages placés dans micdrop/ros2_ws/src

Pour lancer un noeud dans un terminal, on utilise l'option **run** de **ros2** en lui spécifiant le package du noeud et son 
nom.
```bash
ros2 run ubx ubx_node.py
```

Une fois les noeuds lancés, on peut lister les noeuds actifs en utilisant l'option **list** de l'option **node** de 
**ros2**.
```bash
ros2 node list
```

### Topics
Les noeuds communiquent entre eux en utilisant des topics. 
Un noeud peut écouter un topic en s'y abonnant par le biais d'un **subscriber**.
Un noeud peut écrire sur un topic en utilisant un **publisher**.

Un topic est défini par:
- Son nom
- La définition du message qu'il transmet

Dans un terminal, il est possible de lister les topics actifs en utilisant l'option **list** de l'option **topic** de 
**ros2**.
```bash
ros2 topic list
```

Une fois le nom d'un noeud identifié, il est possible d'écouter son contenu en utilisant l'option **echo** de l'option 
**topic** de **ros2**.
```bash
ros2 topic echo diagnostics
```
![ros2_nodes](https://docs.ros.org/en/rolling/_images/Nodes-TopicandService.gif)

### Launch file
MicDrop utilise une dizaine de noeuds en parallèle. Pour simplifier le lancement, on utilise des "launch files".

Les launch files définissent les noeuds à lancer ainsi que leurs paramètres.

Depuis un terminal, on lance un launch file en utilisant l'option **launch** de **ros2** et en lui spécifiant le chemin 
du launch file.
```bash
ros2 launch ch_01_launch.py
```

___
---
# Installation
## Requirements
- [Python3](https://www.python.org/downloads/)
- [Ros2](https://www.ros.org/blog/getting-started/)
- [Colcon](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html)

Des tonnes de paquets python sont nécéssaires à MicDrop. La liste de ces paquets se trouve dans le fichier 
[install_requirements.sh](./install_requirements.sh).

## Microcontrôleurs
Pour mettre à jour le code des microcontrôleurs, il est recommandé d'utiliser 
[Arduino IDE](https://support.arduino.cc/hc/en-us/articles/360019833020-Download-and-install-Arduino-IDE).

Les codes des microcontrôleurs se trouvent dans le dossier [microcontroler_code](./microcontroler_code)

## MicDrop App
Pour simplifier le process de lancement et permettre le click & play, il est possible d'installer MicDrop sur Ubuntu 
comme une application.

### Installation
Pour installer MicDrop App dans un terminal il faut:
1. Se mettre dans le dossier ros2_ws en utilisant [**cd**](#changer-de-dossier)
2. Rendre le script [micdrop_app.bash](./ros2_ws/micdrop_app.bash) executable en utilisant **chmod**
3. Rendre le script [install.bash](./ros2_ws/install.bash) executable en utilisant **chmod**
5. Rendre le script [at_reboot.sh](./ros2_ws/at_reboot.sh) executable en utilisant **chmod**
4. Lancer le script [install.bash](./ros2_ws/install.bash)
5. Redémarrer

```bash
cd ros2_ws
sudo chmod +x ./micdrop_app.bash
sudo chmod +x ./install.bash
sudo chmod +x ./at_reboot.sh
sudo ./install.bash
```
Pour désinstaller MicDrop App:
```bash
sudo ./install.bash uninstall
```

### Utilisation
MicDrop App lance le script [micdrop_app.bash](./ros2_ws/micdrop_app.bash). Ce script lance un 
[launch file](#launch-file) qu'il faut spécifier en haut du script [micdrop_app.bash](./ros2_ws/micdrop_app.bash).

MicdDrop lance à chaque redémarrage de l'ordinateur le script [at_reboot.sh](./ros2_ws/at_reboot.sh).

___
---
# Noeuds
Les noeuds sont placés dans le dossier [src](ros2_ws/src)

## [ch_01_nodes/camera_node](ros2_ws/src/ch_01_nodes/ch_01_nodes/camera_node.py)
Récupère les images d'une caméra.
### Paramètres
| Nom              | Description                                                | type |
|------------------|------------------------------------------------------------|------|
| camera_id        | ID de la caméra                                            | int  |
| m/pic            | Mètres parcouru entre deux images (mettre à 0 pour toutes) | int  |
| exposure_fisheye | Exposition de la caméra ciel                               | int  |

### Topics
| Nom               | Pub/sub | Description | Message                                                                                 |
|-------------------|---------|-------------|-----------------------------------------------------------------------------------------|
| cam_{camera_id}   | Pub     |             | [sensor_msgs/Image](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Image.html) |
| fused_tachometer  | Sub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)                      |

---

## [ch_01_simulation/ahrs_node](ros2_ws/src/ch_01_simulation/ch_01_simulation/ahrs_node.py)
Simule le noeud [xsens_mti_ros2_driver/xsens_mti_node](#xsens_mti_ros2_driverxsens_mti_node).
### Paramètres
| Nom      | Description                                         | type  |
|----------|-----------------------------------------------------|-------|
| sigma    | Écart-type de la distribution de l'erreur (degrés)  | float |

### Topics
| Nom                     | Pub/sub | Description | Message                                                                             |
|-------------------------|---------|-------------|-------------------------------------------------------------------------------------|
| /imu/data               | Pub     |             | [sensor_msgs/Imu](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Imu.html) |
| simulated_ground_truth  | Sub     |             | [interfaces/GnssPvt](ros2_ws/src/interfaces/msg/GnssPvt.msg)                        |

---
## [ch_01_simulation/booya_saujon_node](ros2_ws/src/ch_01_simulation/ch_01_simulation/booya_saujon_node.py)
Simule une vérité terrain de train en utilisant booya1.
### Paramètres
| Nom               | Description                                 | type  |
|-------------------|---------------------------------------------|-------|
| time_between_meas | Temps écoulé entre deux itérations de booya | float |

### Topics
| Nom                     | Pub/sub | Description | Message                                                                             |
|-------------------------|---------|-------------|-------------------------------------------------------------------------------------|
| simulated_ground_truth  | Pub     |             | [interfaces/GnssPvt](ros2_ws/src/interfaces/msg/GnssPvt.msg)                        |

---
## [ch_01_simulation/gnss_node](ros2_ws/src/ch_01_simulation/ch_01_simulation/gnss_node.py)
Simule le noeud [ubx/ubx_node](#ubxubx_node).

Work in progress
### Paramètres
| Nom         | Description                                     | type                |
|-------------|-------------------------------------------------|---------------------|
| sigma_pos   | Écart-type de la distribution de l'erreur (m)   | float               |
| sigma_speed | Écart-type de la distribution de l'erreur (m/s) | float               |
| ref_wgs     | Position de référence (lat, lon)                | tuple[float, float] |

### Topics
| Nom | Pub/sub | Description | Message |
|-----|---------|-------------|---------|
|     |         |             |         |
|     |         |             |         |

---
## [ch_01_simulation/tachometer_node](ros2_ws/src/ch_01_simulation/ch_01_simulation/tachometer_node.py)
Simule le noeud [rotary_encoder_lgpio/encoder_node](#rotary_encoder_lgpioencoder_node).

Work in progress
### Paramètres
| Nom                  | Description                                                      | type                             |
|----------------------|------------------------------------------------------------------|----------------------------------|
| tachometer_names     | tachometer names                                                 | list[str, str, str, str]         |
| reduction_ratio_list | Les rapports de réduction sont en tick/m -> inverser pour m/tick | list[int, int, int, int]         |
| slip_threshold       | Slip threshold                                                   | float                            |
| sigma                | accuracy                                                         | list[float, float, float, float] |
| init_wgs             | Position de référence (lat, lon)                                 | tuple[float, float]              |

### Topics
| Nom                    | Pub/sub | Description | Message                                                            |
|------------------------|---------|-------------|--------------------------------------------------------------------|
| simulated_ground_truth | Sub     |             | [interfaces/GnssPvt](ros2_ws/src/interfaces/msg/GnssPvt.msg)       |
| tachometer             | Pub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg) |

---
## [ihm/ihm_node](ros2_ws/src/ihm/ihm/ihm_node.py)
IHM de monitoring des capteurs
### Paramètres
| Nom              | Description | type                     |
|------------------|-------------|--------------------------|
| start_pos_wgs    |             | tuple[float, float]      |
| tachometer_names |             | list[str, str, str, str] |
| camera_name_list |             | list[int, int, int, int] |
| demo_mode        |             | bool                     |

### Topics
| Nom              | Pub/sub | Description | Message                                                                                 |
|------------------|---------|-------------|-----------------------------------------------------------------------------------------|
| nmea_gga         | Sub     |             | [nmea_msgs/Gpgga](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gpgga.html)            |
| nmea_rmc         | Sub     |             | [nmea_msgs/Gprmc](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gprmc.html)            |
| fused_tachometer | Sub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)                      |
| tachometer       | Sub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)                      |
| /imu/data        | Sub     |             | [sensor_msgs/Imu](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Imu.html)     |
| cam_id           | Sub     |             | [sensor_msgs/Image](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Image.html) |

---
## [rotary_encoder_lgpio/encoder_node](ros2_ws/src/rotary_encoder_lgpio/rotary_encoder_lgpio/encoder_node.py)
Driver des tachymètres banc léger/rpi5
### Paramètres
| Nom                  | Description                                                      | type                     |
|----------------------|------------------------------------------------------------------|--------------------------|
| tachometer_names     | tachometer names                                                 | list[str, str, str, str] |
| reduction_ratio_list | Les rapports de réduction sont en tick/m -> inverser pour m/tick | list[int, int, int, int] |
| slip_threshold       | Slip threshold                                                   | float                    |

### Topics
| Nom                    | Pub/sub | Description | Message                                                            |
|------------------------|---------|-------------|--------------------------------------------------------------------|
| tachometer             | Pub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg) |

---
## [telecom/telecom_node](ros2_ws/src/telecom/telecom/telecom_node.py)
Utilise une radio LORA pour transmettre la position et la vitesse et recevoir une commande de FU.
### Paramètres
| Nom                 | Description                      | type |
|---------------------|----------------------------------|------|
| port                | Chemin du microcontroleur LORA   | str  |
| refresh_rate_s      | Temps entre deux envoies telecom | int  |
| received_msg_header | Header d'un message reçu         | str  |

### Topics
| Nom                 | Pub/sub | Description | Message                                                                        |
|---------------------|---------|-------------|--------------------------------------------------------------------------------|
| nmea_gga            | Sub     |             | [nmea_msgs/Gpgga](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gpgga.html)   |
| fused_tachometer    | Sub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)             |
| tachometer          | Sub     |             | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)             |
| emergency_braking   | Pub     |             | [interfaces/EmergencyBraking](ros2_ws/src/interfaces/msg/EmergencyBraking.msg) |


---
## [tools/logger](ros2_ws/src/tools/tools/logger.py)
Enregistre les données GNSS, IMU et tachymètre dans des fichiers textes.
### Paramètres
| Nom              | Description                   | type                     |
|------------------|-------------------------------|--------------------------|
| folder_path      | Chemin du dossier acquisition | str                      |
| camera_id_list   |                               | list[int, int, int, int] |
| tachometer_list  |                               | list[str, str, str, str] |

### Topics
| Nom               | Pub/sub | Description                                                             | Message                                                                                          |
|-------------------|---------|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| fused_tachometer  | Sub     |                                                                         | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)                               |
| tachometer        | Sub     |                                                                         | [interfaces/Tachometer](ros2_ws/src/interfaces/msg/Tachometer.msg)                               |
| gnss_raw          | Sub     | Données brutes GNSS. /!\ freqId contient sigId /!\                      | [ublox_msgs/RxmRAWX](https://docs.ros.org/en/kinetic/api/ublox_msgs/html/msg/RxmRAWX.html)       |
| nmea_gga          | Sub     | [trames NMEA GGA](https://docs.novatel.com/OEM7/Content/Logs/GPGGA.htm) | [nmea_msgs/Gpgga](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gpgga.html)                     |
| nmea_rmc          | Sub     | [trames NMEA RMC](https://docs.novatel.com/OEM7/Content/Logs/GPRMC.htm) | [nmea_msgs/Gprmc](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gprmc.html)                     |
| nav_pos           | Sub     | Transmet des informations sur la précision du positionnement            | [sensor_msgs/NavSatFix](https://docs.ros.org/en/kinetic/api/sensor_msgs/html/msg/NavSatFix.html) |
| ephem             | Sub     | Paramètres orbitaux des satellites                                      | [interfaces/GnssEphem](ros2_ws/src/interfaces/msg/GnssEphem.msg)                                 |
| atm               | Sub     | Paramètres liés à l'atmosphère                                          | [interfaces/GnssAtm](ros2_ws/src/interfaces/msg/GnssAtm.msg)                                     |
| /imu/data         | Sub     |                                                                         | [sensor_msgs/Imu](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Imu.html)              |
| cam_id            | Sub     |                                                                         | [sensor_msgs/Image](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Image.html)          |


---
## [ubx/ubx_node](ros2_ws/src/ubx/ubx/ubx_node.py)
Driver du GNSS.
### Paramètres
| Nom           | Description          | type |
|---------------|----------------------|------|
| ubx_port      | Chemin du port UART1 | str  |
| ubx_baudrate  | Typiquement 38400    | int  |
| nmea_port     | Chemin du port UART2 | str  |
| nmea_baudrate | Typiquement 38400    | int  |

### Topics
| Nom        | Pub/sub | Description                                                                          | Message                                                                                          |
|------------|---------|--------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| reset      | Service | Service permettant de redémarrer le recepteur                                        | [std_srvs/Trigger](https://docs.ros.org/en/noetic/api/std_srvs/html/srv/Trigger.html)            |
| gnss_raw   | Pub     | Transmet les données brutes GNSS. /!\ freqId contient sigId /!\                      | [ublox_msgs/RxmRAWX](https://docs.ros.org/en/kinetic/api/ublox_msgs/html/msg/RxmRAWX.html)       |
| nmea_gga   | Pub     | Transmet des [trames NMEA GGA](https://docs.novatel.com/OEM7/Content/Logs/GPGGA.htm) | [nmea_msgs/Gpgga](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gpgga.html)                     |
| nmea_rmc   | Pub     | Transmet des [trames NMEA RMC](https://docs.novatel.com/OEM7/Content/Logs/GPRMC.htm) | [nmea_msgs/Gprmc](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gprmc.html)                     |
| gnss_clock | Pub     | Transmet des infos sur la précision de l'horloge                                     | [ublox_msgs/NavCLOCK](https://docs.ros.org/en/kinetic/api/ublox_msgs/html/msg/NavCLOCK.html)     |
| nav_pos    | Pub     | Transmet des informations sur la précision du positionnement                         | [sensor_msgs/NavSatFix](https://docs.ros.org/en/kinetic/api/sensor_msgs/html/msg/NavSatFix.html) |
| ephem      | Pub     | Transmet les paramètres orbitaux des satellites                                      | [interfaces/GnssEphem](ros2_ws/src/interfaces/msg/GnssEphem.msg)                                 |
| atm        | Pub     | Transmet les paramètres liés à l'atmosphère                                          | [interfaces/GnssAtm](ros2_ws/src/interfaces/msg/GnssAtm.msg)                                     |
| rtcm       | Sub     | Corrections RTK                                                                      | [rtcm_msgs/Message](https://docs.ros.org/en/kilted/p/rtcm_msgs/msg/Message.html)                 |

---
## [ubx/ntrip_node](ros2_ws/src/ubx/ubx/ntrip_node.py)
Envoie des corrections RTK en se connectant à un serveur ntrip.
### Paramètres
| Nom          | Description             | type |
|--------------|-------------------------|------|
| ntrip_server | Chemin du serveur ntrip | str  |
| ntrip_port   |                         | int  |
| mountpoint   |                         | str  |
| username     |                         | str  |
| password     |                         | str  |

### Topics
| Nom        | Pub/sub | Description                                                                        | Message                                                                                          |
|------------|---------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| nmea_gga   | Sub     | Recois des [trames NMEA GGA](https://docs.novatel.com/OEM7/Content/Logs/GPGGA.htm) | [nmea_msgs/Gpgga](https://docs.ros.org/en/humble/p/nmea_msgs/msg/Gpgga.html)                     |
| rtcm       | Pub     | Corrections RTK                                                                    | [rtcm_msgs/Message](https://docs.ros.org/en/kilted/p/rtcm_msgs/msg/Message.html)                 |

---
## [watchdog/watchdog_node](ros2_ws/src/watchdog/watchdog/watchdog_node.py)
Surveille l'état des noeuds qui rapportent leurs diagnostics en utilisant le module [heartbeat.py](ros2_ws/src/watchdog/watchdog/heartbeat.py).

Watchdog rapporte les états:
- ok: le noeud tourne normalement
- warn: le noeud tourne anormalement
- error: le noeud ne fonctionne plus correctement
- stale: le noeud ne tourne plus ou est bloqué

Pour intégrer [heartbeat.py](ros2_ws/src/watchdog/watchdog/heartbeat.py) à un noeud:
```python
from watchdog.heartbeat import Heartbeat

class UbxNode(Node):
    def __init__(self, **kwargs):
        super().__init__('nom_du_noeud')
        self.heartbeat = Heartbeat(self, "nom_du_noeud", period_sec=1)
    
    def une_fonction(self):
        self.heartbeat.ok("Tout va bien")
        self.heartbeat.warn("Ya un truc bizarre, j'essaie de le réparer")
        self.heartbeat.error("Erreur fatale, le noeud ne fonctionne plus")
        self.heartbeat.stale("Le capteur ou une fonction ne répond plus")
```

### Paramètres
| Nom              | Description                                                                                                                                | type  |
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-------|
| diagnostic_topic | Default = /diagnostics                                                                                                                     | str   |
| check_rate_s     | Nombre de fois par seconde que les messages diagnostics sont checkés                                                                       | float |
| warn_ratio       | Ratio par rapport au taux de message attendues à partir duquel un warn est levé. (typiquement après 2 secondes sans nouvelle d'un noeud)   | int   |
| stale_ratio      | Ratio par rapport au taux de message attendues à partir duquel un stale est levé. (typiquement après 10 secondes sans nouvelle d'un noeud) | int   |

### Topics
| Nom          | Pub/sub | Description                                | Message                                                                                                              |
|--------------|---------|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| /diagnostics | Sub     | Diagnostics envoyé par les noeuds          | [diagnostic_msgs/DiagnosticArray](https://docs.ros.org/en/noetic/api/diagnostic_msgs/html/msg/DiagnosticArray.html)  |
| /diagnostics | Pub     | Tous les diagnostics consolidés des noeuds | [diagnostic_msgs/DiagnosticArray](https://docs.ros.org/en/noetic/api/diagnostic_msgs/html/msg/DiagnosticArray.html)  |

---
## [xsens_mti_ros2_driver/xsens_mti_node](ros2_ws/src/xsens_mti_ros2_driver)
Driver du fournisseur pour l'IMU.

### Installation
Le driver doit être compilé spécifiquement pour l'ordinateur sur lequel il fonctionne.

Pour compiler le driver dans un terminal:
1. [Se placer](#changer-de-dossier) dans le dossier [xspublic](ros2_ws/src/xsens_mti_ros2_driver/lib/xspublic)
2. Copier les librairies à compiler
3. Compiler les librairies
```bash
cd ros2_ws/src/xsens_mti_ros2_driver/lib/xspublic
cp -r ./to_be_built/xscommon/ .
cp -r ./to_be_built/xscontroller/ .
cp -r ./to_be_built/xstypes/ .
make clean
make -j$(nproc)
```

### Paramètres
| Nom                                 | Description | type |
|-------------------------------------|-------------|------|
| enable_deviceConfig                 |             | bool |
| baudrate                            |             | int  |
| enable_high_rate                    |             | bool |
| pub_accelerationhr                  |             | bool |
| pub_angular_velocity_hr             |             | bool |
| pub_euler                           |             | bool |
| pub_quaternion                      |             | bool |
| enable_active_heading_stabilization |             | bool |

### Topics
| Nom       | Pub/sub | Description | Message                                                                             |
|-----------|---------|-------------|-------------------------------------------------------------------------------------|
| /imu/data | Pub     |             | [sensor_msgs/Imu](https://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Imu.html) |
