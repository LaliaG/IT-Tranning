# SPECIFICATIONS (cahier des charges du projet de refonte du système d'information et du portail web pour IT-Training)

## Contexte
IT-Training souhaite moderniser son système d'information et son portail web, avec une durée de projet fixée à trois mois. Le projet est axé sur l'amélioration de l'accès au catalogue de formations et sur l'automatisation des processus liés à l'enregistrement et à la logistique des formations.

Principaux Points du Projet
Catalogue de Formation

## Accès en ligne à un catalogue de plus de 800 formations.
Formations regroupées par thèmes et sous-thèmes, avec possibilité de navigation entre sous-thèmes.
Chaque formation comporte des détails sur le contenu, le prix, les dates, et les lieux des sessions.
Liens pour tester les prérequis et pour contacter le service commercial.
Enregistrement en Ligne

## Inscription en ligne pour des sessions de formation.
Possibilité de demander des sessions intra-entreprises ou des formations personnalisées.
Automatisation de la logistique dès qu'une demande de formation est faite.
Logistique pour l’Organisation de la Formation

Deux types de formations : inter-entreprises et intra-entreprises.
Gestion des inscriptions, des annulations, et de la logistique (salles, machines, supports).
Alerte en cas de mauvaise note dans les évaluations pour réagir rapidement.
Évaluation de la Session

Appréciations des stagiaires sur divers aspects de la formation.
Un système d'évaluation détaillé pour le contenu et le formateur.
Processus d'alerte pour les évaluations inférieures à un seuil.
Service de Monitoring

Tableau de bord pour le suivi de l'état des formations.
Suivi des participants et des évaluations en temps réel.
Enregistrement et Sélection des Formateurs

Processus d'inscription des formateurs, validation par un responsable.
Espace personnel pour chaque formateur avec leurs formations passées et futures.
Service Commercial et SAV

Prospection de nouveaux clients et suivi des clients existants.
Gestion des statistiques et des indicateurs de performance.


Explication détaillée, section par section, pour mieux comprendre chaque aspect du projet :

### 1. **Contexte de la demande**
L’organisme de formation IT-Training souhaite refaire son système d’information (SI) et son portail web. Le cahier des charges (CDC) est préliminaire et peut être modifié tout au long du projet. Le délai pour ce projet est de **trois mois**.

Le SI à développer est complexe et se concentre principalement sur les aspects suivants :
- **Catalogue de formation**
- **Enregistrement en ligne pour suivre une session de formation**
- **Logistique pour l'organisation de la formation**
- **Évaluation de la session**
- **Service de monitoring**
- **Enregistrement et sélection des formateurs**
- **Service commercial et SAV**

---

### 2. **Catalogue de formation**
- **Accès au catalogue en ligne** : Le site doit fournir un accès à un catalogue de plus de 800 formations, regroupées par **thèmes** (comme Réseaux, Systèmes d'exploitation, Langages de développement, etc.). Les thèmes peuvent être subdivisés en **sous-thèmes** (par exemple, le thème Langages de développement inclut Java, C#, C++, etc.).
  
- **Formations multi-thèmes** : Une formation peut appartenir à plusieurs sous-thèmes. Par exemple, XML pourrait être à la fois dans les langages du Web et la modélisation des données.

- **Accès aux détails d'une formation** : Lorsqu’un utilisateur clique sur une formation (comme PHP), il accède à des détails :
  - **Contenu** de la formation.
  - **Prix**, **dates**, et **lieux** des prochaines sessions.
  - Un lien pour tester les **prérequis** de la formation.
  
- **Inscription** : Les utilisateurs doivent pouvoir contacter le service commercial ou s'inscrire directement en ligne.

### Microservices nécessaires :
1. **Catalogue Service** : Gérer les thèmes, sous-thèmes, et formations.
2. **Prerequisite Test Service** : Permettre aux utilisateurs de tester leur niveau pour une formation spécifique.

### Entités associées :
- **Theme** (id, name)
- **SubTheme** (id, name, parentTheme)
- **Formation** (id, title, content, price, duration, prerequisites)
- **Session** (id, date, location, formation)

---

### 3. **Enregistrement en ligne pour suivre une session**
- **Inscription à une session** : Les clients doivent pouvoir s’inscrire à une session de formation en choisissant une date et un lieu.
  
- **Demande de formation intra-entreprise** : Le client peut demander une formation dans ses propres locaux à une date personnalisée.
  
- **Formation personnalisée** : Si un client souhaite une formation sur-mesure, un plan de formation doit être élaboré et envoyé pour validation.

- **Logistique** : Une fois l’inscription effectuée, une logistique se met en place pour organiser la formation.

### Microservices nécessaires :
1. **Enrollment Service** : Gérer les inscriptions, aussi bien pour les formations standards qu’intra-entreprise.
2. **Custom Training Service** : Gérer les demandes de formation sur-mesure et les plans à envoyer.

### Entités associées :
- **Enrollment** (id, clientDetails, session, status)
- **CustomTrainingRequest** (id, clientDetails, plan, status)

---

### 4. **Logistique pour l'organisation de la formation**
- **Types de formations** : 
  - **Inter-entreprises** : Sessions programmées à l’avance.
  - **Intra-entreprise** : Sessions personnalisées selon les besoins du client.
  
- **Annulation de session** : Si le nombre de participants est inférieur à un seuil (ex : 3), une session doit être annulée et toute la logistique réajustée (annulation de la réservation de la salle, des formateurs, etc.).

- **Gestion des formateurs** : Les formateurs doivent être planifiés à l’avance. Ils sont choisis en fonction de leur expérience et des évaluations des stagiaires.

### Microservices nécessaires :
1. **Logistics Service** : Gérer la logistique des sessions, y compris les réservations de salles, la confirmation des formateurs, etc.
2. **Trainer Management Service** : Gérer les formateurs, leurs disponibilités, et leurs évaluations.

### Entités associées :
- **Logistics** (id, sessionId, trainerId, roomReservation, material)
- **Trainer** (id, name, rating, expertise, availability)

---

### 5. **Évaluation de la session**
- À la fin de chaque session, les stagiaires doivent évaluer la qualité de la formation :
  - **Accueil** chez IT-Training.
  - **Qualité des salles et machines**.
  - **Contenu de la formation** et **qualité du formateur**.

- **Évaluation obligatoire** avant 14h30 le dernier jour de formation. Si une mauvaise évaluation est donnée (moins de 3/5), une alerte est déclenchée, et le formateur peut être exclu du programme.

### Microservices nécessaires :
1. **Evaluation Service** : Gérer la collecte et le traitement des évaluations des formations.

### Entités associées :
- **Evaluation** (id, formationId, trainerId, contentRating, trainerRating, feedback)

---

### 6. **Service de monitoring**
- Ce service permet de suivre l’état de la logistique d’une formation (réservation de salle, matériel, confirmation des formateurs, etc.) via un **tableau de bord**. Il permet également de voir l’état des évaluations en temps réel.

### Microservices nécessaires :
1. **Monitoring Service** : Fournir une vue d’ensemble en temps réel des formations et de leur état.

### Entités associées :
- **MonitoringDashboard** (id, sessionDetails, logisticsStatus, evaluationStatus)

---

### 7. **Enregistrement et sélection des formateurs**
- Un formateur peut s’enregistrer en ligne en remplissant un formulaire avec ses informations (nom, email, expérience, TJM, etc.). 
- Si le formateur est validé, il accède à un espace personnel où il peut consulter ses sessions passées, ses évaluations, et accepter de nouvelles missions.

### Microservices nécessaires :
1. **Trainer Registration Service** : Gérer l’enregistrement des formateurs et leur sélection.

### Entités associées :
- **TrainerApplication** (id, name, email, expertise, status)

---

### 8. **Service commercial et SAV**
- Ce service est chargé de prospecter de nouveaux clients, relancer les anciens, et répondre aux questions avant et après la formation.
- Il fournit également des **statistiques** sur le nombre de clients, le chiffre d’affaires, etc.

### Microservices nécessaires :
1. **CRM Service** : Gérer la relation client et les statistiques commerciales.

### Entités associées :
- **Client** (id, name, contactDetails)
- **SalesStats** (id, totalClients, totalRevenue, totalTrainings)

---

### Conclusion

Pour ce projet, j'ai besoin de concevoir et déployer un système d’information complexe basé sur des microservices indépendants. Chaque aspect du cahier des charges peut être modélisé sous forme de microservices, chacun ayant ses propres responsabilités. Voici un résumé de la structure globale :

### Microservices à implémenter :
1. **Catalogue Service**
2. **Prerequisite Test Service**
3. **Enrollment Service**
4. **Custom Training Service**
5. **Logistics Service**
6. **Trainer Management Service**
7. **Evaluation Service**
8. **Monitoring Service**
9. **Trainer Registration Service**
10. **CRM Service**

### Entités principales :
- **Theme**, **SubTheme**, **Formation**, **Session**
- **Enrollment**, **CustomTrainingRequest**
- **Logistics**, **Trainer**
- **Evaluation**
- **MonitoringDashboard**
- **TrainerApplication**
- **Client**, **SalesStats**

Avec ces services bien définis, je pourrai répondre à tous les besoins du cahier des charges et structurer le projet efficacement.Cette structure permet une bonne répartition des responsabilités entre les microservices tout en facilitant la scalabilité et la maintenance de l'application.


Avec les explications détaillées que nous avons passées en revue, je pense que le cahier des charges est déjà bien structuré et couvre la majorité des besoins de l'organisme IT-Training. Cependant, voici quelques points à examiner pour voir s'il manque quelque chose ou si des ajustements pourraient être apportés :

### 1. **Gestion des utilisateurs**
- **Formateurs et clients** : Le cahier des charges parle bien des formateurs, mais il n'y a pas beaucoup de détails sur la gestion des **comptes clients** (utilisateurs qui souhaitent s’inscrire aux formations). Tu pourrais peut-être ajouter une section dédiée à la gestion des utilisateurs pour clarifier :
  - L’**inscription des clients** (via formulaire avec validation d’email, etc.).
  - La **connexion** et la **gestion de compte** (modification d'informations, historique des formations, etc.).

### 2. **Authentification et rôles**
- Le CDC ne mentionne pas s’il faut implémenter une authentification avec des rôles (par exemple : **administrateur**, **formateur**, **client**). Si c’est pertinent pour ton projet, tu devrais ajouter cette information :
  - **Clients** : Pour s’inscrire et voir leur historique.
  - **Formateurs** : Pour accéder à leurs sessions et évaluations.
  - **Administrateurs** : Pour gérer les formations, formateurs, et les utilisateurs.

### 3. **Notifications et rappels**
- Le cahier des charges ne parle pas de **notifications** (par exemple, envoyer des rappels par email ou SMS pour rappeler aux participants les dates des formations, informer les formateurs d’une annulation, etc.). Cela pourrait être un plus pour améliorer l’expérience utilisateur.
  - **Notifications automatiques** pour les inscriptions, rappels de formation, annulations, etc.

### 4. **Gestion des paiements**
- Le CDC n'aborde pas les aspects liés au **paiement** des formations :
  - Est-ce que les clients peuvent payer en ligne ? Par carte, virement, etc.
  - Si oui, il faudrait gérer les **factures**, les **réductions** (par exemple, si un client s’inscrit à plusieurs formations), et **les remboursements** en cas d’annulation.
  
Si les paiements en ligne sont nécessaires, il serait bon d’inclure ces détails dans le CDC et d’ajouter un microservice dédié à la **gestion des paiements**.

### 5. **Statistiques plus détaillées**
- Le CDC mentionne des statistiques pour le service commercial, mais tu pourrais étoffer cette section :
  - Statistiques détaillées sur les **sessions** (combien de participants ont assisté, quels sont les taux de réussite, etc.).
  - Statistiques sur les **formateurs** (combien de sessions animées, évaluation moyenne, etc.).

### 6. **Support technique et FAQ**
- Est-ce que tu veux offrir un **support technique** ou une section **FAQ** pour aider les utilisateurs à naviguer sur le site et comprendre les processus d’inscription, de paiement, ou de gestion des comptes ? Ce point n'est pas mentionné dans le CDC, mais peut être une valeur ajoutée.

### 7. **Scalabilité**
- Le cahier des charges ne mentionne pas de **contraintes techniques** spécifiques en termes de scalabilité (ex : combien de clients peuvent se connecter en même temps, quelle est la charge maximale du système, etc.). Il peut être intéressant de définir ces attentes pour éviter les problèmes de performance lors de pics d’utilisation (par exemple, à la rentrée scolaire).

### Conclusion
Le cahier des charges est déjà solide et bien orienté. Toutefois, tu pourrais envisager d'ajouter les aspects suivants :
- **Gestion des utilisateurs** (inscription, rôles).
- **Notifications et rappels**.
- **Paiements en ligne**.
- **Statistiques détaillées** sur les performances.
- **Support technique et FAQ**.
- **Contraintes techniques/scalabilité**.

Cela dépendra de la portée du projet et des besoins exacts de l’organisme. Si ces fonctionnalités sont jugées non nécessaires pour la version initiale du projet, tu peux les exclure ou les envisager comme des évolutions futures.



Excellente question ! Ces rôles sont effectivement essentiels à bien comprendre pour définir leur place dans l’application. Voici une analyse pour situer chaque rôle et expliquer comment ils pourraient interagir dans le système.

### 1. **Responsable Catalogue**
- **Rôle** : Ce responsable gère le **catalogue des formations**. Il décide des formations à proposer, les modifie ou les supprime. Il peut aussi gérer les contenus associés aux formations (descriptions, objectifs, prérequis).
- **Type** : **Admin** ou **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un **module de gestion des formations** (ajout, modification, suppression de formations).
  - Gestion des **catégories** et sous-catégories de formation.
  - Consultation des statistiques liées aux formations (popularité, inscriptions, etc.).

### 2. **Responsable d’Enregistrement des Formations**
- **Rôle** : Ce responsable s’occupe de **l’inscription des formations** dans le système. Il enregistre les nouvelles formations créées par le Responsable Catalogue dans l’application et s’assure qu’elles sont correctement visibles pour les utilisateurs.
- **Type** : **Admin** ou **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un module d’**enregistrement des formations** pour les ajouter dans la base de données et les rendre disponibles dans le catalogue en ligne.
  - Validation des informations saisies par le Responsable Catalogue avant leur mise en ligne.

### 3. **Responsable de Formation Logistique**
- **Rôle** : Ce responsable gère les aspects logistiques liés aux formations. Il coordonne la réservation des salles, les équipements nécessaires (ordinateurs, projecteurs, etc.), et s'assure que tout est en place pour le bon déroulement des sessions.
- **Type** : **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un **module logistique** pour réserver des salles et des équipements pour chaque session.
  - Gestion des **informations logistiques** associées aux formations (adresse de la salle, matériel requis, etc.).
  - Envoi de notifications aux formateurs et participants pour leur rappeler les informations logistiques (salle, matériel, etc.).

### 4. **Responsable Installation des Salles**
- **Rôle** : Ce responsable s’occupe de l’installation physique des salles avant chaque formation. Il met en place les équipements nécessaires (vidéoprojecteur, ordinateurs, etc.) et s’assure que tout est fonctionnel.
- **Type** : **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à une liste des **installations nécessaires** pour chaque session de formation.
  - Possibilité de consulter et valider les **demandes d’installation** émises par le Responsable Logistique.
  - Mise à jour du statut de la salle (par exemple, "installation en cours" ou "installation terminée").

### 5. **Responsable Planning des Intervenants Externes**
- **Rôle** : Ce responsable gère la planification des **intervenants externes** (formateurs ou conférenciers) qui viennent donner des formations. Il planifie les horaires et s’assure de la disponibilité des intervenants.
- **Type** : **Admin** ou **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un **module de gestion des plannings** pour organiser les créneaux des intervenants externes.
  - Envoi de notifications automatiques aux intervenants pour confirmer leurs horaires et sessions.
  - Suivi des **disponibilités** des intervenants (mise à jour du planning en fonction de leurs disponibilités).

### 6. **Responsables des Formations (inter ou intra entreprises)**
- **Rôle** : Ils sont responsables de l’organisation des formations qui se déroulent soit en **inter-entreprises** (groupes d’apprenants venant de différentes entreprises) soit en **intra-entreprise** (formation organisée spécifiquement pour une seule entreprise).
- **Type** : **Admin** ou **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un module pour la gestion des **formations inter/intra**.
  - Planification des sessions de formation en fonction des besoins des entreprises.
  - Suivi des inscriptions, gestion des demandes des entreprises, et envoi d’informations logistiques.

### 7. **Responsable Commercial**
- **Rôle** : Il est chargé de la partie **commerciale** de l’organisation des formations. Il contacte les entreprises pour vendre des sessions de formation et gère les relations commerciales (négociation des prix, signature des contrats, etc.).
- **Type** : **Admin** ou **Personnel interne**.
- **Interaction dans l’application** :
  - Accès à un module de gestion des **clients et des entreprises**.
  - Suivi des **demandes de formations** des entreprises.
  - Consultation de **rapports commerciaux** (revenus générés, formations vendues, etc.).
  - Envoi d’**offres commerciales** et de devis pour les formations intra ou inter-entreprises.

### Synthèse des interactions et types d’utilisateurs
- **Admins** : Responsable Catalogue, Responsable Enregistrement des Formations, Responsable Planning des Intervenants Externes, Responsable des Formations (inter/intra entreprises), Responsable Commercial. Ces utilisateurs ont généralement accès à des **modules avancés de gestion** et peuvent gérer plusieurs aspects de l’application (ajout de formations, plannings, statistiques, etc.).
- **Personnel interne** : Responsable de Formation Logistique, Responsable Installation des Salles. Ces utilisateurs interagissent principalement avec des modules spécifiques (gestion des salles, matériels, installations) et ne gèrent pas toute l'application.

### Conclusion
Ces responsables occupent des rôles clés dans l'organisation et le bon fonctionnement des formations. Dans l'application, ils interagiront chacun avec des modules spécifiques en fonction de leurs missions. Les admins auront un accès plus large et complet, tandis que le personnel interne aura des droits limités à la logistique et l'installation.

C'est une excellente question essentielle pour structurer efficacement votre système d'information et assurer une gestion fluide des rôles et des responsabilités au sein de votre application. Voici une analyse détaillée pour déterminer le nombre d'administrateurs et de personnel nécessaires, ainsi que la meilleure façon de les structurer dans votre application.

1. Définition des Rôles et Responsabilités
Rôles Administratifs (Admins)
Les rôles administratifs sont responsables de la gestion globale du système et de la supervision des différents aspects opérationnels. Dans votre cahier des charges, les rôles suivants ont été identifiés :

Responsable Catalogue
Responsable d’Enregistrement des Formations
Responsable Planning des Intervenants Externes
Responsable des Formations (inter/intra entreprises)
Responsable Commercial
Rôles du Personnel
Les rôles du personnel sont plus opérationnels et spécifiques à certaines tâches logistiques ou techniques :

Responsable de Formation Logistique
Responsable Installation des Salles
2. Nombre d'Administrateurs et de Personnel
Administrateurs (Admins)
Pour une gestion efficace et sécurisée, il est recommandé de :

Créer des rôles distincts pour chaque type d’administrateur. Cela permet de contrôler les permissions et d’assurer que chaque administrateur a accès uniquement aux modules nécessaires à ses responsabilités.

Avoir au moins un administrateur par rôle. Cela signifie que vous devriez avoir au minimum :

1 Responsable Catalogue
1 Responsable d’Enregistrement des Formations
1 Responsable Planning des Intervenants Externes
1 Responsable des Formations (inter/intra entreprises)
1 Responsable Commercial
Total minimum d’admins : 5

Évolutivité : Si votre organisation s'agrandit ou si plusieurs personnes partagent une même responsabilité, vous pouvez ajouter plus d’administrateurs par rôle. Par exemple, si vous avez plusieurs Responsables Commercial, chacun peut avoir son propre compte admin avec les mêmes permissions.

Personnel
Pour le personnel opérationnel, la structuration dépendra de la taille de votre organisation et du volume des tâches. Cependant, voici une recommandation de base :

Créer des rôles distincts pour chaque type de personnel. Cela permet une gestion claire et des permissions adaptées.

Avoir au moins un membre du personnel par rôle. Donc :

1 Responsable de Formation Logistique
1 Responsable Installation des Salles
Total minimum de personnel : 2

Évolutivité : Si vous avez plusieurs responsables pour une même fonction (par exemple, plusieurs Responsables Installation des Salles pour couvrir différents centres de formation), ajoutez des comptes supplémentaires avec le même rôle.

3. Structuration des Rôles dans l'Application
Rôles Administratifs
Chaque rôle administratif devrait avoir des permissions spécifiques correspondant à ses responsabilités :

Responsable Catalogue

Accès complet au module de gestion des formations et du catalogue.
Permissions pour ajouter, modifier, supprimer des formations, thèmes et sous-thèmes.
Responsable d’Enregistrement des Formations

Accès au module d’enregistrement des formations.
Permissions pour valider et enregistrer les formations proposées par le Responsable Catalogue.
Responsable Planning des Intervenants Externes

Accès au module de gestion des plannings des intervenants.
Permissions pour planifier, modifier et confirmer les plannings des formateurs externes.
Responsable des Formations (inter/intra entreprises)

Accès au module de gestion des formations inter et intra entreprises.
Permissions pour planifier, suivre et gérer les sessions de formation selon les besoins des entreprises.
Responsable Commercial

Accès au module CRM (Customer Relationship Management).
Permissions pour gérer les clients, suivre les ventes, et consulter les statistiques commerciales.
Rôles du Personnel
Chaque rôle du personnel devrait également avoir des permissions adaptées :

Responsable de Formation Logistique

Accès au module logistique.
Permissions pour réserver des salles, gérer les équipements et coordonner la logistique des formations.
Responsable Installation des Salles

Accès au module d'installation des salles.
Permissions pour planifier et confirmer les installations techniques nécessaires avant chaque session de formation.
4. Interaction entre les Rôles
Admins
Coordination : Les différents administrateurs devront collaborer étroitement pour assurer que les formations sont correctement enregistrées, planifiées, et que la logistique est bien gérée.
Accès Centralisé : Utilisez Jira pour suivre les tâches et les responsabilités de chaque administrateur. Chaque rôle peut avoir son propre épic ou projet dans Jira pour mieux organiser le travail.
Personnel
Opérationnel : Le personnel travaillant sur la logistique et l’installation des salles interagira principalement avec les modules dédiés dans l’application, en se basant sur les informations fournies par les administrateurs.
Communication : Utilisez des notifications automatiques et des outils de communication intégrés (comme le chat ou les forums) pour assurer une coordination fluide entre le personnel et les administrateurs.
5. Mise en Œuvre dans Jira
Création des Rôles dans Jira
Définir les rôles dans Jira en fonction des administrateurs et du personnel identifié.
Attribuer des permissions spécifiques à chaque rôle, en alignement avec les responsabilités décrites.
Créer des Épics et User Stories spécifiques à chaque rôle. Par exemple :
Épic "Gestion des Formations" pour le Responsable Catalogue.
Épic "Logistique des Formations" pour le Responsable de Formation Logistique.
Gestion des Tâches
Épicer par rôle : Organisez les tâches Jira en fonction des rôles. Chaque responsable peut avoir des tickets assignés à son épic correspondant.
Priorisation et Sprint Planning : Priorisez les tâches en fonction des besoins du projet et planifiez-les dans des sprints pour une gestion Agile efficace.
6. Recommandations Finales
Sécurité et Permissions : Assurez-vous que chaque rôle a uniquement accès aux modules et fonctionnalités nécessaires pour éviter toute fuite d'information ou modification non autorisée.
Documentation : Documentez clairement les responsabilités et les permissions de chaque rôle dans votre cahier des charges pour faciliter la compréhension et la gestion future.
Évolutivité : Prévoyez la possibilité d'ajouter ou de modifier des rôles et des permissions à mesure que votre organisation évolue ou que de nouvelles responsabilités apparaissent.
Conclusion
En structurant votre application avec plusieurs rôles administratifs correspondant à chaque responsabilité clé et en définissant clairement les rôles du personnel, vous assurez une gestion efficace et sécurisée de votre système d'information. Cela facilite également la collaboration et la coordination entre les différentes parties prenantes du projet IT-Training.

Si tu as besoin de précisions supplémentaires sur la configuration des rôles dans Jira ou sur la gestion des permissions, n'hésite pas à me le faire savoir !