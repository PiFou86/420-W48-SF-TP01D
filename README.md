# TP01 - SécurITé

## 1 - Directives

### 1.1 - Déroulement du TP

- Le projet est réalisé en équipe de 2 personnes
- Vous devez utiliser Git pour gérer vos sources
- Vous devez utiliser SharePoint pour gérer votre document de rapport (Onglet "Fichiers" de votre équipe Teams)
- La remise du travail doit être effectuée sur et à la date indiquée sur la plateforme d'enseignement Omnivox

### 1.2 - À remettre sur la plateforme d'enseignement

- Un document word contenant le détail du projet
- Votre code source
- Le lien YouTube de votre vidéo de présentation

### 1.3 - Structure de la remise

- Vous devez remplir le fichier "AUTHORS.md". Il contient le nom et le matricule des membres de l'équipe, le lien YouTube et le lien du dépôt GitHub.
- Votre code source doit être dans le répertoire  `src` du présent dépôt Git
- Le répertoire source doit suivre la structure d’un projet Platform.io
- Vous devez fournir une vidéo de 5 minutes illustrant le circuit, le code et le fonctionnement :
  - La vidéo doit être déposée sur YouTube avec une option de partage « non listée »
  - Le lien de la vidéo doit être indiqué dans le document word AINSI QUE dans le fichier `AUTHORS.md`

- Le répertoire "documents" doit contenir votre rapport de TP en version finale. Les versions intermédiaires peuvent être rédigées à partir de l'onglet "Fichiers" de votre équipe Teams afin que vous gardiez une trace de l'évolution de votre travail et que nous puissions valider la participation des membres de l'équipe.

### 1.4 - Évaluation

L'évaluation du travail est effectuée par les enseignants de l'UE en se basant sur :

- L'historique de Git (code source) et de Teams/Sharepoint (documentation) font office de référence pour évaluer la proportion du travail effectué par chaque équipier

- La qualité et le contenu du code source :

  - Conformité du code et des normes d'écriture utilisées dans le cours
  - Fonctionnalité du code
  - Facilité de lecture du code
  - Modularité
  - Modèle objet
  - Paramétrisation du code
  - Utilisation de constantes
  - Utilisation de fichiers de configuration
  - etc.

- La qualité et le contenu du document word :
  
  - Français: fautes grammaticales ou syntaxiques
  - Clarté et précision des explications
  - Schéma du circuit: doit pouvoir être assembler par un étranger
  - Mise en page et présentation selon les normes du cégep
  - etc.

- La qualité et le contenu de la présentation vidéo :

  - Vidéo
  - Audio
  - Explication orale
  - etc.

Tout partage de code, d'explications, de bouts de texte, etc. est considéré comme du plagiat. Pour plus de détails, consultez le site (et ses vidéos) [Sois intègre du Cégep de Sainte-Foy](http://csfoy.ca/soisintegre) ainsi que [l'article 6.1.12 de la PÉA](https://www.csfoy.ca/fileadmin/documents/notre_cegep/politiques_et_reglements/5.9_PolitiqueEvaluationApprentissages_2019.pdf)

## 2 - Description du projet

La startup ```SécurITé``` est une startup qui propose un service de sécurité pour les entreprises. Elle est spécialisée dans la sécurité des serrures. Elle veut se lancer dans le numérique et proposer un modèle électronique basé sur une combinaison de quatre (4) valeurs choisies parmi l'alphabet 'A', 'B', 'C' qui permettent de déverrouiller un coffre-fort ou d'en changer la combinaison.

Ce système comprend :

- Un microcontrôleur de type ATmega328P
- Une plaque d'expérimentation
  - Un ensemble de DELs
  - Trois boutons poussoirs, chacun représenté par les valeurs 'A', 'B' ou 'C'.
  - Un potentiomètre pour le choix du mode de fonctionnement
- Câbles de connexion

Votre tâche générale consiste à mettre en place une procédure pour déverrouiller la serrure électronique ou en modifier la clef secrète. La clef secrète est entreposée dans l'EEPROM du MCU.

La serrure s'ouvre si la combinaison de quatre (4) valeurs tapées sur les boutons poussoirs dans le bon ordre correspond à la clef secrète. La serrure reste ouverte pendant cinq (5) secondes puis se verrouille à nouveau.

La clef secrète peut être changée par une procédure décrite plus loin.

Des DELs rouge et verte clignotent selon différents scénarios afin de guider l'utilisateur dans ses opérations.

### 2.1 - Procédures pour déverrouiller la serrure

- Au démarrage du programme, la serrure est verrouillée. Elle est en mode ```saisie du code```. Une DEL rouge clignote selon l'alternance suivante: dix (10) secondes allumée, une (1) seconde éteinte, et reprise du cycle aussi longtemps que la combinaison n'est pas complétée. La DEL verte est éteinte durant ce cycle.
- Pour déverrouiller la serrure, l'utilisateur appuie en séquence sur les boutons pour obtenir une combinaison à quatre (4) valeurs. Si la combinaison est correcte, la DEL rouge s'éteint et la DEL verte s'allume pour indiquer que la serrure est déverrouillée. L'état déverrouillée n'est actif que pendant cinq (5) secondes. Une fois ce délai passé, la serrure reprend son mode ```saisie du code```.
- En cas d'échec, il faut attendre 10 secondes avant de pouvoir saisir une nouvelle combinaison. Évidemment, les saisies sont ignorée durant cette période et la serrure reste verrouillé ! La DEL clignote à une fréquence de deux (2) Hertz pour indiquer le blocage. À la deuxième tentative, le temps d'attente passe à trente (30) secondes, puis à une (1) minute pour la troisième tentative. Après trois tentatives infructueuses, la serrure se bloque pour une durée de cinq (5) minutes.

### 2.2 - Modification du code

- Le changement de clef secrète n'est autorisé que si la serrure est déverrouillée. Cette demande est signalée lorsque le potentiomètre est placé dans la deuxième moitié de sa valeur de résistance (milieu à maximum).
- L'utilisateur saisit deux fois la nouvelle combinaison. Si les deux saisies sont identiques,la DEL verte clignote trois (3) fois à la fréquence de un (1) Hertz. La nouvelle clef est alors enregistrée dans l'EEPROM. 
- En cas d'échec, la DEL verte s'éteint et la DEL rouge clignote trois (3) fois à la fréquence de un (1) Hertz. La serrure se vérouille et toutes les opérations sont bloquées pendant dix (10) secondes. Une fois ce délai passé, la serrure reprend son mode ```saisie du code```.

### 2.3 - Gestion des statistiques

- Compter le nombre de tentatives de saisie de la combinaison et le nombre de succès
- Enregistrer et gérer ces statistiques dans l'EEPROM de manière optimisée pour limiter l'usure

## Consignes supplémentaires

- Optimisation EEPROM : Consulter la documentation disponible dans le fichier README.md du répertoire random sur le Git
- Sécurité du code : S'assurer que le système est résistant contre les tentatives de décodage par force brute en limitant les tentatives de saisie
- Test de démarrage: S'assurer que les DELS sont fonctionnelles. Un message doit être envoyé sur la console pour une confirmation
- Documentation : Fournir une documentation technique du projet incluant le schéma de montage, le code source commenté et un guide d'utilisation facile pour une personne novice

### 2.4 - BONUS 10%

Proposer et implanter la fonctionnalité `clefMaitre` pour permettre aux administrateurs de déverrouiller la serrure en cas extrême.

### 2.5 - Considérations techniques

- À la première utilisation de votre serrure, le code est "ABCA"
- A chaque démarrage de votre serrure, le décompte des statistiques doit être affiché à la console

## 3 - Description détaillée du document à remettre

Le document word doit décrire le contexte du projet, sa planification, la répartition des tâches avec un registre des heures passées, les schémas du montage, les diagrammes UML pertinents ainsi que l'inventaire des composants et une estimation des coûts pour une pompe

## 4 - Répartition des points

1. Document word : (35%)
   - Contexte du projet (5%)
   - Planification, attribution et répartition des tâches (5%)
   - Description des étapes du projet (5%)
   - Diagramme de classes (5%)
   - Esquisse du montage (5%)
     - Schéma technique: (pensez au réparateur qui devra comprendre le circuit et le dépanner)
   - Inventaire des pièces et évaluation du coût total (5%)
   - Registre des heures consacrées au projet (5%)

Le registre doit indiquer la répartition des tâches. Le registre doit montrer les tâches respectives que chaque personne aura fait avec le nombre d’heures par tâche.

2. Vidéo de 5 minutes illustrant le fonctionnement (15%)
   - Présentation rapide du circuit (5%)
   - Présentation rapide de la structure du code et des choix de conception (5%)
   - Présentation du fonctionnement (5%)

3. Code (50%)
   - Bon fonctionnement du programme (30%)
     - Saisie correct des codes
       - Validations
       - Temporisation
       - Sécurité contre les tentatives de décodage par force brute
     - Modification de la clef secrète
       - Validations
       - Temporisation
     - Sauvegarde de la clef secrète
       - Utilisation de l'EEPROM
       - Optimisation de l'EEPROM
     - Récupération de la clef secrète au démarrage
     - Animation des DELs selon la séquence des opérations: verrouillage, déverrouillage, modification de la clef, etc
   - Respect des bonnes pratiques de programmation modulaire et orientée objet (20%)
