# 🎯 Projet Data Analyst - Analyse de l’Attrition RH chez Atlas Labs (Power BI + SQL + DAX)

> *“Et si les données pouvaient prévenir les départs avant qu’ils ne surviennent ?”*

Ce projet explore les causes de l’attrition des employés au sein d’**Atlas Labs**, une entreprise tech fictive. À travers une analyse croisée des données RH (effectifs, démographie, performance, conditions de travail), j’ai construit un **dashboard interactif Power BI** pour aider les RH à piloter la rétention des talents.

---

## 🧭 Objectif métier

**Secteur ciblé :** Ressources Humaines (RH)
**Problématique :**
> *Quels sont les profils les plus à risque de départ, et quels leviers activer pour améliorer la fidélisation des talents ?*

---

## 🧱 Méthodologie (STAR)

### **S – Situation**
Dans un contexte de tension sur le marché de l’emploi, notamment dans la tech, l’entreprise constate un taux d’attrition préoccupant (16,1 %) .
L’entreprise fictive Atlas Labs, en croissance rapide, souhaite comprendre pourquoi certains profils quittent plus souvent que d’autres, malgré un investissement constant dans le bien-être et le développement interne.

### **T – Tâche**
- Analyser les données historiques RH et de performance.
- Identifier les facteurs associés à un départ (profil, poste, satisfaction, etc.).
- Concevoir un tableau de bord dynamique pour guider la stratégie RH.

### **A – Actions**
- Nettoyage & modélisation des données SQL (employés, rôles, salaires, évaluations).
- Création de mesures DAX personnalisées (voir ci-dessous).
- Construction de 4 pages Power BI : **Overview**, **Demographics**, **Performance Tracker**, **Attrition**.
- Ajout de filtres dynamiques (ancienneté, genre, poste, fréquence des déplacements…).
- Ajouté une dimension inclusion et diversité, en croisant les données d’ethnicité, de genre et d’âge avec les salaires moyens pour identifier les écarts à surveiller.

### **R – Résultats**
- Identification de **postes à fort turnover** : Sales Reps, RH, Data Scientists.
- Mise en évidence de **facteurs aggravants** : heures supplémentaires, voyages fréquents, faible ancienneté.
- **Cibler les profils** en perte de satisfaction, avant que la situation ne se dégrade
- Prioriser des **actions de rétention** sur les jeunes talents à moins de 2 ans d’ancienneté
- Comprendre l’impact de **l’équilibre vie pro/perso** et des déplacements fréquents sur la fidélité des collaborateurs

---
### Intentions d’apprentissage
Ce projet m’a permis de :

- Approfondir la modélisation temporelle et la gestion des jointures dans Power BI et SQL.
- Renforcer mes compétences en DAX
- Concevoir un dashboard orienté utilisateur métier, simple et impactant.
- Explorer des thématiques humaines comme l’inclusion, la satisfaction et la diversité salariale.



)

## 🧮 Quelques formules DAX utilisées



### 🔹 InactiveEmployees

Calcule le nombre total d'employés ayant quitté l'entreprise (Attrition = "Yes").
InactiveEmployees = 
CALCULATE(
    [TotalEmployees],
    FILTER(DimEmployee, DimEmployee[Attrition] = "Yes")
)
Mesure le pourcentage d’attrition globale.
Le DIVIDE() est utilisé pour éviter les erreurs de division par zéro.
% Attrition Rate = 
DIVIDE([InactiveEmployees], [TotalEmployees])

Calcule la note moyenne attribuée par les managers lors des revues de performance.
Avg Manager Rating = 
AVERAGE(FactPerformanceRating[ManagerRating])

Utilise une relation inactive (via USERELATIONSHIP) entre l’environnement de travail et une table de niveaux de satisfaction (DimSatisfiedLevel).
EnvironmentSatisfaction = 
CALCULATE (
    MAX ( FactPerformanceRating[EnvironmentSatisfaction] ),
    USERELATIONSHIP (
        FactPerformanceRating[EnvironmentSatisfaction], 
        DimSatisfiedLevel[SatisfactionID]
    )
)
Affiche la date de la dernière évaluation si elle existe, ou "No Review Yet" sinon.
LastReviewDate = 
IF (
    MAX(FactPerformanceRating[ReviewDate]) = BLANK(),
    "No Review Yet",
    MAX(FactPerformanceRating[ReviewDate])
)

