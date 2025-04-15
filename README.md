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



## 🧮 Quelques formules DAX utilisées

```dax
InactiveEmployees = CALCULATE([TotalEmployees], FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))
% Attrition Rate = DIVIDE([InactiveEmployees], [TotalEmployees])
Avg Manager Rating = 
)


EnvironmentSatisfaction = 
CALCULATE (
    MAX ( FactPerformanceRating[EnvironmentSatisfaction] ),
    USERELATIONSHIP ( FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionID] )
)
DimDate = 
VAR _minYear = YEAR(MIN(DimEmployee[HireDate]))
VAR _maxYear = YEAR(MAX(DimEmployee[HireDate]))
VAR _fiscalStart = 4 

RETURN
ADDCOLUMNS(
    CALENDAR(
                DATE(_minYear,1,1),
                DATE(_maxYear,12,31)

),

"Year",YEAR([Date]),
"Year Start",DATE( YEAR([Date]),1,1),
"YearEnd",DATE( YEAR([Date]),12,31),
"MonthNumber",MONTH([Date]),
"MonthStart",DATE( YEAR([Date]), MONTH([Date]), 1),
"MonthEnd",EOMONTH([Date],0),
"DaysInMonth",DATEDIFF(DATE( YEAR([Date]), MONTH([Date]), 1),EOMONTH([Date],0),DAY)+1,
"YearMonthNumber",INT(FORMAT([Date],"YYYYMM")),
"YearMonthName",FORMAT([Date],"YYYY-MMM"),
"DayNumber",DAY([Date]),
"DayName",FORMAT([Date],"DDDD"),
"DayNameShort",FORMAT([Date],"DDD"),
"DayOfWeek",WEEKDAY([Date]),
"MonthName",FORMAT([Date],"MMMM"),
"MonthNameShort",FORMAT([Date],"MMM"),
"Quarter",QUARTER([Date]),
"QuarterName","Q"&FORMAT([Date],"Q"),
"YearQuarterNumber",INT(FORMAT([Date],"YYYYQ")),
"YearQuarterName",FORMAT([Date],"YYYY")&" Q"&FORMAT([Date],"Q"),
"QuarterStart",DATE( YEAR([Date]), (QUARTER([Date])*3)-2, 1),
"QuarterEnd",EOMONTH(DATE( YEAR([Date]), QUARTER([Date])*3, 1),0),
"WeekNumber",WEEKNUM([Date]),
"WeekStart", [Date]-WEEKDAY([Date])+1,
"WeekEnd",[Date]+7-WEEKDAY([Date]),
"FiscalYear",if(_fiscalStart=1,YEAR([Date]),YEAR([Date])+ QUOTIENT(MONTH([Date])+ (13-_fiscalStart),13)),
"FiscalQuarter",QUARTER( DATE( YEAR([Date]),MOD( MONTH([Date])+ (13-_fiscalStart) -1 ,12) +1,1) ),
"FiscalMonth",MOD( MONTH([Date])+ (13-_fiscalStart) -1 ,12) +1
)
