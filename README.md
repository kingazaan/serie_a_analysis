Analysis of Serie A data

##Step 1: Convert all files into 1 and add season column

##Step 2: Delete all betting columns
Keep:

Season      Season
Date	    Match Date (dd/mm/yy)
AwayTeam    Away Team
HomeTeam	Home Team

AS          Away Team Shots
HS	        Home Team Shots
AST         Away Team Shots on Target
HST	        Home Team Shots on Target
HTAG        Half Time Away Team Goals
HTHG	    Half Time Home Team Goals
HTR	        Half Time Result (H=Home Win, D=Draw, A=Away Win)
            (Dummies)   H=3, D=1, A=0
FTAG	    Full Time Away Team Goals
FTHG	    Full Time Home Team Goals
FTR	        Full Time Result (H=Home Win, D=Draw, A=Away Win)
            (Dummies)   H=3, D=1, A=0
AC	        Away Team Corners
HC	        Home Team Corners
AF	        Away Team Fouls Committed
HF	        Home Team Fouls Committed
AY	        Away Team Yellow Cards
HY	        Home Team Yellow Cards
AR	        Away Team Red Cards
HR	        Home Team Red Cards

##Step 3: Correlation

Very obvious ant-correlations, not much learned here
One thing, away shots on target much less or more than home shots on target. Away team is more varied in shots

##Step 4: Creating Index for categorical variables

I converted the Half-time and Full time results into ordinal columns, with a home win = 3, a draw = 1, and a home loss = 0
Made Home and Away teams into numeric values. Their index:

    Home_Team_Index        Team
0                 0           0
1                 1    Atalanta
2                 2        Bari
3                 3   Benevento
4                 4     Bologna
5                 5     Brescia
6                 6    Cagliari
7                 7       Carpi
8                 8     Catania
9                 9      Cesena
10               10      Chievo
11               11     Crotone
12               12      Empoli
13               13  Fiorentina
14               14   Frosinone
15               15       Genoa
16               16       Inter
17               17    Juventus
18               18       Lazio
19               19       Lecce
20               20     Livorno
21               21       Milan
22               22      Napoli
23               23      Novara
24               24     Palermo
25               25       Parma
26               26     Pescara
27               27        Roma
28               28   Sampdoria
29               29    Sassuolo
30               30       Siena
31               31        Spal
32               32      Spezia
33               33      Torino
34               34     Udinese
35               35      Verona

##Step 5: Logistic Regression

We really used a softmax regression, as the dependent variable has 3 outcomes. First normalize the model's x and y, then look at things like accuracy and baseline score. Looked at baseline score as well, pretty poor in comparison. Using Cohen's score to observe randomness, there is no randomness in our model

Feature Importance - found that:
The biggest factor in a home win is Full_Time_Away_Team_Goals, followed by Away_Team_Shots_on_Target
The biggest factor in ahome loss is Full_Time_Home_Team_Goals, followed by Half_Time_Result, and Home_Team_Shots_on_Target

![](img\feature_importance_softmax_regression.png)

I used Ridge regression instead of lasso to highlight categories that were pretty effective on the dataset

##Step 6: Random Forest

Used another model called random forest to validate earlier findings
The accuracy, baseline accuracy, and cohens score were relatively the same

![](img\feature_importance_random_forest.png)

##Step 7: K-Folds

Use K-folds along with the random forest model so that the model oculd be testeed with "Real-World data" and see how it reacts during every fold. Was generally pretty solid, model ended at 0.993 accuracy

![](img\K-folds_6_splits.png)

##Step 8: Hyperparameters

Used a hyperparameter grid with some out-there paamateres so that the model could adjust itself. An example is features; if I were to run hyper parameters for max_features, and the grid found that 0.05 is a better parameter, it would mean that most of my categories were useless