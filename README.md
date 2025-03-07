# Ironhack Final Project: Case AME-Digital

![GitHub top language](https://img.shields.io/github/languages/top/hbatistuzzo/Ironhack-Final-Project)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/hbatistuzzo/Ironhack-Final-Project)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/hbatistuzzo/Ironhack-Final-Project)
![GitHub last commit](https://img.shields.io/github/last-commit/hbatistuzzo/Ironhack-Final-Project)

Project status: Completed
<p align="right"><img src="Images/ironbadge.png" width="40%" alt="Logo"></p>

---
# Project objective

This data viz project comes from the fintech AME-Digital's case for Data Engineers.
The case itself is based on the Stack Overflow 2018 Developer Survey (available on Kaggle: https://www.kaggle.com/datasets/stackoverflow/stack-overflow-2018-developer-survey).
Nearly 100,000 developers took the 30-minute survey in January 2018, answering a total of 129 questions ranging from basic info (gender, age, job title, annual salary estimate) to subjective opinions regarding ethics in coding and responsability towards AI creations.

![plot](Images/intro.jpg)

The case sets specific goals: answering questions regarding average salary of respondents, where they are from, what technologies and communication tools they use etc.

# Technologies

- Python 3.9.15
	- Pandas 1.5.2
	- Numpy 1.21.6
	- Pycaret 2.3.10
	- Seaborn 0.12.1
	- Matplotlib 3.6.2
	- SQLAlchemy 1.4.42
	- Scikit-learn 1.0.2
- MySQL 8.0
- Tableau 2022.3

# Project Description

The case itself is heavily focused on SQL queries. It is divided into 2 portions:

1) Populating a data bank from the raw survey data.

2) Performing SQL queries to answer the given questions.

My final project aimed to focus on data viz, thus after answering the case's questions my goal was to generate insights from respondents' salary data.

# Steps

## Data Inspection and cleaning

- The `survey_results_schema.csv` file contains each column name from the main results along with the question text corresponding to that column. The excerpt below provides a snapshot into it:

|   |     Column |                                                                           QuestionText |
|--:|-----------:|---------------------------------------------------------------------------------------:|
| 0 | Respondent |                 Randomized respondent ID number (not in order of survey response time) |
| 1 |      Hobby |                                                                Do you code as a hobby? |
| 2 | OpenSource |                                             Do you contribute to open source projects? |
| 3 |    Country |                                              In which country do you currently reside? |
| 4 |    Student | Are you currently enrolled in a formal, degree-granting college or university program? |
| 5 | Employment |                  Which of the following best describes your current employment status? |

- The `surveyresultspublic.csv` file (available on the Kaggle link provided above) contains the main survey results, one respondent per row and one column per question.

Of a total of 98,855 respondents, 67,441 (68.2%) fully completed the survey. Their answers were transported into a Pandas DataFrame for inspection.
As the snapshot below shows, most of the data was previously curated (for example, not all respondents - 1st column - were included, mostly due to incomplete answers).

|   | Respondent | Hobby | OpenSource |        Country |        Student |         Employment |                                   FormalEducation |                                    UndergradMajor |              CompanySize |                                           DevType |      YearsCoding | YearsCodingProf |                    JobSatisfaction |                 CareerSatisfaction |                                     HopeFiveYears |                                   JobSearchStatus |                LastNewJob | AssessJob1 | AssessJob2 | AssessJob3 | AssessJob4 | AssessJob5 | AssessJob6 | AssessJob7 | AssessJob8 | AssessJob9 | AssessJob10 | AssessBenefits1 | AssessBenefits2 | AssessBenefits3 | AssessBenefits4 | AssessBenefits5 | AssessBenefits6 | AssessBenefits7 | AssessBenefits8 | AssessBenefits9 | AssessBenefits10 | AssessBenefits11 | JobContactPriorities1 | JobContactPriorities2 | JobContactPriorities3 | JobContactPriorities4 | JobContactPriorities5 | JobEmailPriorities1 | JobEmailPriorities2 | JobEmailPriorities3 | JobEmailPriorities4 | JobEmailPriorities5 | JobEmailPriorities6 | JobEmailPriorities7 |                                       UpdateCV |                    Currency | Salary | SalaryType | ConvertedSalary | CurrencySymbol |                                CommunicationTools | TimeFullyProductive |                                    EducationTypes |                                   SelfTaughtTypes | TimeAfterBootcamp |                 HackathonReasons | AgreeDisagree1 | AgreeDisagree2 |             AgreeDisagree3 |                               LanguageWorkedWith |                           LanguageDesireNextYear |                                DatabaseWorkedWith |                            DatabaseDesireNextYear |                PlatformWorkedWith |            PlatformDesireNextYear | FrameworkWorkedWith | FrameworkDesireNextYear |                                        IDE | OperatingSystem | NumberMonitors |                                       Methodology |    VersionControl |                     CheckInCode | AdBlocker | AdBlockerDisable |                                  AdBlockerReasons |          AdsAgreeDisagree1 |          AdsAgreeDisagree2 |          AdsAgreeDisagree3 |                                        AdsActions | AdsPriorities1 | AdsPriorities2 | AdsPriorities3 | AdsPriorities4 | AdsPriorities5 | AdsPriorities6 | AdsPriorities7 |                                       AIDangerous |                                     AIInteresting |                                AIResponsible |                                          AIFuture |          EthicsChoice |                     EthicsReport |                            EthicsResponsible | EthicalImplications | StackOverflowRecommend |              StackOverflowVisit | StackOverflowHasAccount |                          StackOverflowParticipate |                                 StackOverflowJobs |                      StackOverflowDevStory | StackOverflowJobsRecommend | StackOverflowConsiderMember |      HypotheticalTools1 |      HypotheticalTools2 |      HypotheticalTools3 |      HypotheticalTools4 |      HypotheticalTools5 |               WakeTime | HoursComputer |         HoursOutside |            SkipMeals |            ErgonomicDevices |                   Exercise | Gender |        SexualOrientation |                                  EducationParents |                RaceEthnicity |               Age | Dependents | MilitaryUS |                        SurveyTooLong |    SurveyEasy |
|--:|-----------:|------:|-----------:|---------------:|---------------:|-------------------:|--------------------------------------------------:|--------------------------------------------------:|-------------------------:|--------------------------------------------------:|-----------------:|----------------:|-----------------------------------:|-----------------------------------:|--------------------------------------------------:|--------------------------------------------------:|--------------------------:|-----------:|-----------:|-----------:|-----------:|-----------:|-----------:|-----------:|-----------:|-----------:|------------:|----------------:|----------------:|----------------:|----------------:|----------------:|----------------:|----------------:|----------------:|----------------:|-----------------:|-----------------:|----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|--------------------:|--------------------:|--------------------:|--------------------:|--------------------:|--------------------:|--------------------:|-----------------------------------------------:|----------------------------:|-------:|-----------:|----------------:|---------------:|--------------------------------------------------:|--------------------:|--------------------------------------------------:|--------------------------------------------------:|------------------:|---------------------------------:|---------------:|---------------:|---------------------------:|-------------------------------------------------:|-------------------------------------------------:|--------------------------------------------------:|--------------------------------------------------:|----------------------------------:|----------------------------------:|--------------------:|------------------------:|-------------------------------------------:|----------------:|---------------:|--------------------------------------------------:|------------------:|--------------------------------:|----------:|-----------------:|--------------------------------------------------:|---------------------------:|---------------------------:|---------------------------:|--------------------------------------------------:|---------------:|---------------:|---------------:|---------------:|---------------:|---------------:|---------------:|--------------------------------------------------:|--------------------------------------------------:|---------------------------------------------:|--------------------------------------------------:|----------------------:|---------------------------------:|---------------------------------------------:|--------------------:|-----------------------:|--------------------------------:|------------------------:|--------------------------------------------------:|--------------------------------------------------:|-------------------------------------------:|---------------------------:|----------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|-----------------------:|--------------:|---------------------:|---------------------:|----------------------------:|---------------------------:|-------:|-------------------------:|--------------------------------------------------:|-----------------------------:|------------------:|-----------:|-----------:|-------------------------------------:|--------------:|
| 0 |          1 |   Yes |         No |          Kenya |             No | Employed part-time |          Bachelor’s degree (BA, BS, B.Eng., etc.) |                         Mathematics or statistics |       20 to 99 employees |                              Full-stack developer |        3-5 years |       3-5 years |                Extremely satisfied |                Extremely satisfied | Working as a founder or co-founder of my own c... | I’m not actively looking, but I am open to new... |      Less than a year ago |       10.0 |        7.0 |        8.0 |        1.0 |        2.0 |        5.0 |        3.0 |        4.0 |        9.0 |         6.0 |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |              NaN |              NaN |                   3.0 |                   1.0 |                   4.0 |                   2.0 |                   5.0 |                 5.0 |                 6.0 |                 7.0 |                 2.0 |                 1.0 |                 4.0 |                 3.0 | My job status or other personal status changed |                         NaN |    NaN |    Monthly |             NaN |            KES |                                             Slack | One to three months | Taught yourself a new language, framework, or ... | The official documentation and/or standards fo... |               NaN | To build my professional network | Strongly agree | Strongly agree | Neither Agree nor Disagree |                       JavaScript;Python;HTML;CSS |                       JavaScript;Python;HTML;CSS | Redis;SQL Server;MySQL;PostgreSQL;Amazon RDS/A... | Redis;SQL Server;MySQL;PostgreSQL;Amazon RDS/A... |          AWS;Azure;Linux;Firebase |          AWS;Azure;Linux;Firebase |        Django;React |            Django;React |              Komodo;Vim;Visual Studio Code |     Linux-based |              1 |                                       Agile;Scrum |               Git |          Multiple times per day |       Yes |               No |                                               NaN |             Strongly agree |             Strongly agree |             Strongly agree | Saw an online advertisement and then researche... |            1.0 |            5.0 |            4.0 |            7.0 |            2.0 |            6.0 |            3.0 | Artificial intelligence surpassing human intel... |             Algorithms making important decisions | The developers or the people creating the AI | I'm excited about the possibilities more than ... |                    No |                Yes, and publicly | Upper management at the company/organization |                 Yes |       10 (Very Likely) |          Multiple times per day |                     Yes | I have never participated in Q&A on Stack Over... | No, I knew that Stack Overflow had a jobs boar... |                                        Yes |                        NaN |                         Yes |    Extremely interested |    Extremely interested |    Extremely interested |    Extremely interested |    Extremely interested | Between 5:00 - 6:00 AM |  9 - 12 hours |          1 - 2 hours |                Never |               Standing desk |       3 - 4 times per week |   Male | Straight or heterosexual |          Bachelor’s degree (BA, BS, B.Eng., etc.) |  Black or of African descent | 25 - 34 years old |        Yes |        NaN | The survey was an appropriate length |     Very easy |
| 1 |          3 |   Yes |        Yes | United Kingdom |             No | Employed full-time |          Bachelor’s degree (BA, BS, B.Eng., etc.) | A natural science (ex. biology, chemistry, phy... | 10,000 or more employees | Database administrator;DevOps specialist;Full-... | 30 or more years |     18-20 years |            Moderately dissatisfied | Neither satisfied nor dissatisfied | Working in a different or more specialized tec... |                   I am actively looking for a job |     More than 4 years ago |        1.0 |        7.0 |       10.0 |        8.0 |        2.0 |        5.0 |        4.0 |        3.0 |        6.0 |         9.0 |             1.0 |             5.0 |             3.0 |             7.0 |            10.0 |             4.0 |            11.0 |             9.0 |             6.0 |              2.0 |              8.0 |                   3.0 |                   1.0 |                   5.0 |                   2.0 |                   4.0 |                 1.0 |                 3.0 |                 4.0 |                 5.0 |                 2.0 |                 6.0 |                 7.0 |              I saw an employer’s advertisement | British pounds sterling (£) |  51000 |     Yearly |         70841.0 |            GBP | Confluence;Office / productivity suite (Micros... | One to three months | Taught yourself a new language, framework, or ... | The official documentation and/or standards fo... |               NaN |                              NaN |          Agree |          Agree | Neither Agree nor Disagree |                     JavaScript;Python;Bash/Shell |                                        Go;Python |                        Redis;PostgreSQL;Memcached |                                        PostgreSQL |                             Linux |                             Linux |              Django |                   React |         IPython / Jupyter;Sublime Text;Vim |     Linux-based |              2 |                                               NaN |    Git;Subversion |            A few times per week |       Yes |              Yes | The website I was visiting asked me to disable it |             Somewhat agree | Neither agree nor disagree | Neither agree nor disagree |                                               NaN |            3.0 |            5.0 |            1.0 |            4.0 |            6.0 |            7.0 |            2.0 |                     Increasing automation of jobs |                     Increasing automation of jobs | The developers or the people creating the AI | I'm excited about the possibilities more than ... | Depends on what it is |            Depends on what it is | Upper management at the company/organization |                 Yes |       10 (Very Likely) | A few times per month or weekly |                     Yes |                   A few times per month or weekly |                                               Yes |        No, I have one but it's out of date |                          7 |                         Yes | A little bit interested | A little bit interested | A little bit interested | A little bit interested | A little bit interested | Between 6:01 - 7:00 AM |   5 - 8 hours |      30 - 59 minutes |                Never | Ergonomic keyboard or mouse |  Daily or almost every day |   Male | Straight or heterosexual |          Bachelor’s degree (BA, BS, B.Eng., etc.) | White or of European descent | 35 - 44 years old |        Yes |        NaN | The survey was an appropriate length | Somewhat easy |
| 2 |          4 |   Yes |        Yes |  United States |             No | Employed full-time |                                  Associate degree | Computer science, computer engineering, or sof... |       20 to 99 employees |          Engineering manager;Full-stack developer |      24-26 years |       6-8 years |               Moderately satisfied |               Moderately satisfied | Working as a founder or co-founder of my own c... | I’m not actively looking, but I am open to new... |      Less than a year ago |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |         NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |              NaN |              NaN |                   NaN |                   NaN |                   NaN |                   NaN |                   NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                                            NaN |                         NaN |    NaN |        NaN |             NaN |            NaN |                                               NaN |                 NaN |                                               NaN |                                               NaN |               NaN |                              NaN |            NaN |            NaN |                        NaN |                                              NaN |                                              NaN |                                               NaN |                                               NaN |                               NaN |                               NaN |                 NaN |                     NaN |                                        NaN |             NaN |            NaN |                                               NaN |               NaN |                             NaN |       NaN |              NaN |                                               NaN |                        NaN |                        NaN |                        NaN |                                               NaN |            NaN |            NaN |            NaN |            NaN |            NaN |            NaN |            NaN |                                               NaN |                                               NaN |                                          NaN |                                               NaN |                   NaN |                              NaN |                                          NaN |                 NaN |                    NaN |                             NaN |                     NaN |                                               NaN |                                               NaN |                                        NaN |                        NaN |                         NaN |                     NaN |                     NaN |                     NaN |                     NaN |                     NaN |                    NaN |           NaN |                  NaN |                  NaN |                         NaN |                        NaN |    NaN |                      NaN |                                               NaN |                          NaN |               NaN |        NaN |        NaN |                                  NaN |           NaN |
| 3 |          5 |    No |         No |  United States |             No | Employed full-time |          Bachelor’s degree (BA, BS, B.Eng., etc.) | Computer science, computer engineering, or sof... |     100 to 499 employees |                              Full-stack developer |      18-20 years |     12-14 years | Neither satisfied nor dissatisfied |              Slightly dissatisfied | Working as a founder or co-founder of my own c... | I’m not actively looking, but I am open to new... |      Less than a year ago |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |        NaN |         NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |             NaN |              NaN |              NaN |                   NaN |                   NaN |                   NaN |                   NaN |                   NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                 NaN |                       A recruiter contacted me |            U.S. dollars ($) |    NaN |        NaN |             NaN |            NaN |                                               NaN | Three to six months | Completed an industry certification program (e... | The official documentation and/or standards fo... |               NaN |                              NaN |       Disagree |       Disagree |          Strongly disagree | C#;JavaScript;SQL;TypeScript;HTML;CSS;Bash/Shell | C#;JavaScript;SQL;TypeScript;HTML;CSS;Bash/Shell | SQL Server;Microsoft Azure (Tables, CosmosDB, ... | SQL Server;Microsoft Azure (Tables, CosmosDB, ... |                             Azure |                             Azure |                 NaN | Angular;.NET Core;React |           Visual Studio;Visual Studio Code |         Windows |              2 |                                Agile;Kanban;Scrum |               Git |          Multiple times per day |       Yes |              Yes | The ad-blocking software was causing display i... | Neither agree nor disagree |             Somewhat agree |             Somewhat agree | Stopped going to a website because of their ad... |            NaN |            NaN |            NaN |            NaN |            NaN |            NaN |            NaN | Artificial intelligence surpassing human intel... | Artificial intelligence surpassing human intel... |      A governmental or other regulatory body | I don't care about it, or I haven't thought ab... |                    No | Yes, but only within the company | Upper management at the company/organization |                 Yes |       10 (Very Likely) |            A few times per week |                     Yes |                   A few times per month or weekly |                                               Yes |        No, I have one but it's out of date |                          8 |                         Yes |     Somewhat interested |     Somewhat interested |     Somewhat interested |     Somewhat interested |     Somewhat interested | Between 6:01 - 7:00 AM |  9 - 12 hours | Less than 30 minutes | 3 - 4 times per week |                         NaN | I don't typically exercise |   Male | Straight or heterosexual | Some college/university study without earning ... | White or of European descent | 35 - 44 years old |         No |         No | The survey was an appropriate length | Somewhat easy |
| 4 |          7 |   Yes |         No |   South Africa | Yes, part-time | Employed full-time | Some college/university study without earning ... | Computer science, computer engineering, or sof... | 10,000 or more employees | Data or business analyst;Desktop or enterprise... |        6-8 years |       0-2 years |                 Slightly satisfied |               Moderately satisfied | Working in a different or more specialized tec... | I’m not actively looking, but I am open to new... | Between 1 and 2 years ago |        8.0 |        5.0 |        7.0 |        1.0 |        2.0 |        6.0 |        4.0 |        3.0 |       10.0 |         9.0 |             1.0 |            10.0 |             2.0 |             4.0 |             8.0 |             3.0 |            11.0 |             7.0 |             5.0 |              9.0 |              6.0 |                   2.0 |                   1.0 |                   4.0 |                   5.0 |                   3.0 |                 7.0 |                 3.0 |                 6.0 |                 2.0 |                 1.0 |                 4.0 |                 5.0 | My job status or other personal status changed |     South African rands (R) | 260000 |     Yearly |         21426.0 |            ZAR | Office / productivity suite (Microsoft Office,... | Three to six months | Taken a part-time in-person course in programm... | The official documentation and/or standards fo... |               NaN |                              NaN | Strongly agree |          Agree |          Strongly disagree |               C;C++;Java;Matlab;R;SQL;Bash/Shell |             Assembly;C;C++;Matlab;SQL;Bash/Shell |              SQL Server;PostgreSQL;Oracle;IBM Db2 |                         PostgreSQL;Oracle;IBM Db2 | Arduino;Windows Desktop or Server | Arduino;Windows Desktop or Server |                 NaN |                     NaN | Notepad++;Visual Studio;Visual Studio Code |         Windows |              2 | Evidence-based software engineering;Formal sta... | Zip file back-ups | Weekly or a few times per month |        No |              NaN |                                               NaN |             Somewhat agree |             Somewhat agree |          Somewhat disagree | Clicked on an online advertisement;Saw an onli... |            2.0 |            3.0 |            4.0 |            6.0 |            1.0 |            7.0 |            5.0 |             Algorithms making important decisions |             Algorithms making important decisions | The developers or the people creating the AI | I'm excited about the possibilities more than ... |                    No | Yes, but only within the company | Upper management at the company/organization |                 Yes |       10 (Very Likely) |           Daily or almost daily |                     Yes |               Less than once per month or monthly | No, I knew that Stack Overflow had a jobs boar... | No, I know what it is but I don't have one |                        NaN |                         Yes |    Extremely interested |    Extremely interested |    Extremely interested |    Extremely interested |    Extremely interested |         Before 5:00 AM | Over 12 hours |          1 - 2 hours |                Never |                         NaN |       3 - 4 times per week |   Male | Straight or heterosexual | Some college/university study without earning ... | White or of European descent | 18 - 24 years old |        Yes |        NaN | The survey was an appropriate length | Somewhat easy |

So far:
- No duplicates
- But some columns are massively empty, as seen through the following proportion of NaNs:

```
percent = data.isnull().sum()/ len(data) *100
percent.sort_values(ascending = False)
```

which yields (first 5 lines):
```
TimeAfterBootcamp  |            93.270952
MilitaryUS         |            84.036215
HackathonReasons   |            74.011431
ErgonomicDevices   |            65.547519
AdBlockerReasons   |            61.817814
```

- *Which* does __not__ imply lack of data per se (i.e. not all respondents participated in bootcamps, nor in the military, hence empty values). 


Cleaning was therefore limited to reindexing, converting NaNs to 0.0 for salaries and converting Yes/No questions to 1's and 0's, as requested on the case description.
This proved useful later when building the predictive model due to a faster processing of numerical rather than categorical variables.
Some questions e.g. "LanguageWorkedWith" which involved multiple inputs required processing for isolating multiple answers with split and explode methods.

<img src="/Images/snapshot2.png" align="center" width="100%"/>

Care should be taken when using this sort of data as a proxy for the state of Information Technology around the world. As we will show, the vast majority of respondents are men from the United States. Although several other countries are present
in this survey, some are artificially excluded by a matter of language barriers: users from China, South Korea and Japan, for example, are massively underrepresented in this survey.

Some of the most precious variables from this dataset are undoubtedly the ones concerning salaries. The Stack Overflow survey included an extra variable "Converted Salary" which
normalizes all the inputs to a single format: yearly salary in US$. This is exceptional considering that input by respondents came in myriad formats regarding currencies
and salary types (e.g. yearly, monthly or weekly). These were all done for the current exchange rates from January 2018.

## Insights from salary data:

<p align="center">
    <img width="50%" src="/Images/money1.png">
</p>

A simple scatter plot of normalized salary entries yields a gamma distribution with some expected results:
Out of 47,702 entries, about 80% of entries are up to 100k US$/year. This rises to  over 93% when up to 200k US$/year i.e. only a minority of respondents have "astronomical" salaries.
Highlighted in the plot are two conspicuous features: outliers for salaries exactly at 1 million and 2 million US$/year. These suggest that some respondents rounded their revenue for convenience.


<img src="/Images/money2.png" align="right" width="50%"/>

If we filter a range of people earning at least a minimum wage (for Brazilian standards in 2018, roughly 3,000 US$/year) up to 500,000 US$/year (rich, but not astronomically so),
the distribution becomes roughly logarithmic.



Hidden beneath this plot, however, are discrepancies that arise from the respondent profile: their gender, for example, betray a salary gap *dependent* on their nationality, as we will see below in detail.



A number of other factors play a role here, ranging from the years of experience that an employee has with coding to the size of the company in which they are employed.

<br><br>

### Nationality and Gender

Stack Overflow users from the United States dominate the survey, followed by countries from the G7 and BRICS groups (China being the biggest exception due to the language barrier discussed in the beginning).
<p align="center">
    <img width="75%" src="Images/countries.png">
</p>


On all of these countries, gender displays a persistent pattern: on average, less than 10% of respondents are female.
<p align="center">
    <img width="75%" src="Images/gender.png">
</p>


The distribution of salaries is heavily dependent on nationality, as we can see below. Rich countries like the US and England display normal distributions (as Kolmogorov-Smirnov tests quickly shows us), although the average salary shifts almost by 50% between the 2 curves.
The US by far concentrates the number of rich and "astronomically" rich respondents, but there are no discrepancies or excessive kurtosis in either side.

<p align="center">
    <img width="75%" src="Images/money3.png">
</p>

Germany is an interesting case, shown below, as the distribution is actually bimodal. A fellow data analyst from Germany believes that this could have a sociological explanation as part of a recent migration flux (mostly from Turkey) is absorbed into less regulated (and less paying) jobs in IT companies.

<p align="center">
    <img width="75%" src="Images/money4.png">
</p>

India (red in the first graph) shows yet another pattern, more logarithmic than normal, with a massive number of low-paying jobs. Brazil finds itself roughly between the pattern shown in India and those of 1st-world countries.
As the IT field expands in Brazil, it is expected that the distribution will shift to the right, becoming more normal as well.

A number of other inferences can be shown when plotting salary against other variables. Some are unsurprising (there is a linear correlation between age and better salaries. Bigger companies usually correlate with better paying jobs etc).
Some are interesting, perhaps, as didactic exercise: as shown in the graph below, people with better salaries tend to exercise more. The question is: do they exercise more *because* they have better salaries (improving quality of live) or could there be a relationship between productivity and those who are more active?

<p align="center">
    <img width="75%" src="Images/exercise2.png">
</p>

At this point many a frustrated statistician could feel inclined to shout at the screen the maxim that "correlation does not imply causation!", and we will move on to more objective (or, rather, *less* *subjective*) insights.


I should, however, point to the fact that there are myriad interesting conclusions that come from simply tallying the answers to categorical, opinion-based questions. Against the unspoken rules of data viz, some even benefit from being displayed on the outcast of plots, the poor pie graph:
<p align="center">
    <img width="100%" src="Images/AI.png">
</p>

When answering the question "Whose responsibility is it, **primarily**, to consider the ramifications of increasingly advanced AI technology?", almost half perceive this responsibility to fall in the lap of the developers themselves, followed by those who believe that either the government or Elon Musks should take the reins of the situation. Almost 10%, however, steer towards full AI anarchy.


Finally, on the subject of the salary gender gap, the following graph shows that the gap, albeit prevalent almost everywhere, varies from country to country. Please observe that the x-axis scale is logarithmic, thus the gap is deceivingly larger than visually represented. The gap is more prevalent in countries such as Russia, Australia and India. Others, such as Brazil, display more gender equality *but* we should also note that outliers i.e. the rich and astronomically rich are always more prevalent for men than women. To conclude, we must not forget that women are severely underrepresented in this survey, so inferences from the outliers alone should be taken with a grain of salt.

<p align="center">
    <img width="100%" src="Images/gendersalary.png">
</p>

After these inspections were done, it was finally time to construct the databank and target the questions from the case itself. I thank the creators of SQLAlchemy for this amazing tool which severely speeds up the process of data ingestion, especially when transporting it from previous Pandas DataFrames.

## Populating the data bank.

The data bank structure is provided in the case documentation and can be seen in the figure below:

<p align="center">
    <img width="100%" src="Images/databank_structure.png">
</p>


Queries were made directly in MySQL workbench after ingestion, as seen in this example:

<p align="center">
    <img width="100%" src="Images/query.png">
</p>

Below are the questions and their respective answers for the AME-Digital case.

-- What is the quantatity of respondents by country?


SELECT Country, COUNT(*) from respondente
JOIN country on country.Country_id = respondente.Country_id
GROUP BY Country
ORDER BY COUNT(*) DESC;

-- How many users from the US prefer Windows OS?


SELECT * from respondente
JOIN country on country.Country_id = respondente.Country_id
JOIN os on os.OS_id = respondente.OS_id
WHERE Country = 'United States' and OperatingSystem = 'Windows';
	-- returns 7635 users

-- What is the salary average for users from Israel who prefer Linux?


SELECT AVG(ConvertedSalary) from respondente
JOIN country on country.Country_id = respondente.Country_id
JOIN os on os.OS_id = respondente.OS_id
WHERE Country = 'Israel' and OperatingSystem = 'Linux-based'  and CSMR != 0;
	-- returns 178 users in total, but only 93 with actual salary input. CSMR mean yields 43487.84135304659 monthly reais
    -- 'CSMR' stands for ConvertedSalaryMonthlyReais
    -- 'ConvertedSalary' is yearly in USD

-- What is the mean and standard deviation for users' salaries who use Slack, for each available size of companies?


SELECT CompanySize,AVG(ConvertedSalary),STDDEV(ConvertedSalary) from respondente
join resp_usa_ferramenta as ruf on ruf.respondent = respondente.respondent
join tool on tool.Tool_id = ruf.Tool_id
join companies on companies.Company_id = respondente.Company_id
where CommunicationTools = 'Slack' and CSMR != 0
GROUP BY CompanySize
ORDER BY AVG(ConvertedSalary);
	-- 29483 users who use slack. Now to group by companysize

-- What is the diference in salary mean for Brazilian respondents who code as a hobby and those for all Brazilian respondents, grouped by OS?


SELECT OperatingSystem,AVG(ConvertedSalary) as media_hobby from respondente
JOIN country on country.Country_id = respondente.Country_id
JOIN os on os.OS_id = respondente.OS_id
where Country = 'Brazil' and Hobby = 1
GROUP BY OperatingSystem;

SELECT OperatingSystem,AVG(ConvertedSalary) as media_geral from respondente
JOIN country on country.Country_id = respondente.Country_id
JOIN os on os.OS_id = respondente.OS_id
where Country = 'Brazil'
GROUP BY OperatingSystem;

-- What are the top 3 technologies more frequently used by developers?


SELECT CommunicationTools,COUNT(CommunicationTools) from respondente
join resp_usa_ferramenta as ruf on ruf.respondent = respondente.respondent
join tool on tool.Tool_id = ruf.Tool_id
join companies on companies.Company_id = respondente.Company_id
GROUP BY CommunicationTools
ORDER BY Count(CommunicationTools) DESC
LIMIT 3;
	-- Slack, Jira, Office

-- What are the top 5 countries in terms of salary?


SELECT Country, AVG(ConvertedSalary) from respondente
JOIN country on country.Country_id = respondente.Country_id
where ConvertedSalary != 0
GROUP BY Country
ORDER BY AVG(ConvertedSalary) DESC;

# Modelling

The case was solved, so it was time to delve deeper. The database provided a good opportunity to implement a predictive model, namely one who could provide a salary estimate
based on respondents' answers to other related questions: company size, if they coded as a hobby, how many years of experience with coding etc.
A first attempt was made with SKLearn, by creating labels and splitting a training and testing dataset:

<p align="center">
    <img width="100%" src="Images/model.png">
</p>

Once the first model ran without errors, I decided to try a second run with the (arguably) much more powerful Pycaret tool, since it already has functions implemented for
comparing a number of models and later evaluating each one. A GradientBoosterRegressor (gbr) model yielded the best results, and indeed the Feature Importance Plot indicated
that the most relevant components were being used to provide better estimates of salary. Still, as indicated by this conspicuous red arrow on the image below, the RMSE
was perhaps a bit too high to be actually useful in salary estimates. If we take the Root Mean Squared Error, in this context, as a proxy to indicate by how much the model deviates
from an accurate answer, then the error itself is as big as an actual salary: over 45k US$/year.

<p align="center">
    <img width="100%" src="Images/model2.png">
</p>

This came as a surprising result, given that the shining star of the whole 2018 Stack Overflow Survey was the curated "Converted Salary" item that normalized every
salary entry to a single standard: yearly salary in US$. The model was already tailored to ignoring outliers and other discrepancies, so this result beckoned a closer look
at the data itself. This investigation highlighted some odd entries, such as the one below, that deviated from an actual full-term salary.

<p align="center">
    <img width="100%" src="Images/model3.png">
</p>

But much more important (and which, in my opinion, shed a final light on the model's behaviour) was the following entry:
<p align="center">
    <img width="100%" src="Images/model4.png">
</p>

The "Converted Salary" column was taking a fairly low salary (3190 monthly Brazilian Reais) and translating it as 1 million US$ per year. Someone, somewhere, while curating
this dataset, apparently forgot that some countries use a comma as a decimal separator, instead of a dot (as is the case for North America). Even worse, since the column was
created using exchange rates from 2018, reconstructing the correct values was not a trivial task. To remove all of these values would significantly decrease the training and
testing populations for the model to run, although it remains as a possible next step in investigating the database.

# Conclusion

The oxford comma is known to be the origin of myriad semantic misinterpretations in the english language and, truthfully, many other languages as well. It was unexpected, 
however, to see this unsuspecting character wreaking this much havoc in numerical values dependent on the written standard of currencies. Just like electrical outlets with
different shapes around the world causing headaches for unprepared tourists, this lack of consensus in format creeped its way into polluting this database.

The modelling exercise still holds didactic value nonetheless, and more importantly the case was solved.The big takeaway? A comma always project two shadows,
and we must be prepared to identify which is the right one, lest the second stabs us right in the back, AFTER you already ran your predictive models on what seemed to be a pristine source of data.

<p align="center">
    <img width="100%" src="Images/ohoh.png">
</p>