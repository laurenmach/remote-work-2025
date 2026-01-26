# Regression Analysis of Remote Work Participation
This is my 2025 Masters in Data Science thesis for Western Governor's University, _Regression Analysis of Remote Work Participation_. The presentation is designed for a non-technical audience and goes through the research process, including:

    Background and problem
    Research question and hypothesis
    Data source
    Population selection process
    Comparison of target population to full dataset
    Linear regression results and findings
    Limitations
    Suggestions for further research
    Implications for business

## Executive Summary 

Since 2019, businesses’ approach to telework has shifted dramatically. Conversations about whether to allow telework (either in a fully remote or hybrid posture) often center on team dynamics, job requirements, competitor policy, or company culture. Less considered is how telework options change the makeup of the labor pool itself. When deciding to allow telework, business leaders may assume a team is made of the same people whether they are at home or in the office. But is that true? The hypothesis of this study is that the availability of telework (including hybrid work) increases key populations’ participation in the workforce. In other words, for these populations, telework policies don’t just impact where they work, they impact how much or even whether they are participating in the workforce at all.  

The _American Time Use Survey_ (ATUS) dataset is collected and maintained by the US Bureau of Labor Statistics (BLS) as part of the US Census (2024). Data from the 2003-2023 atuscps file is used comprising 298,841 observations and 92 variables. Observations are from 2019-2023 surveys. The variables include participant identifiers, demographics, work data, and personal information (Appendix A). Data preparation included the following steps: 

    Importing desired columns from atuscps file and dropping pre-2019 rows 
    Converting continuous variables for use in population selection (such as household size and age) into continuous variables based on distribution visualization 
    Maintaining a ‘people’ dataset with identifier columns that can be used to subset participants (e.g. all 2019 participants or all identified population participants) without having to load the full dataset 
    Renaming telework columns for clarity and creating combined “TELEALL” column 
    Creating calculated employment status field for population selection based on both current employment (full/part-time/unemployed) and desired employment to identify underemployed individuals 
    Creating calculated CORR field showing whether a participant’s employment and telework statuses are the same (counting underemployment with unemployment) 
    One hot encoding and sorting variable dimensions based on CORR using both MCA and groupby to select target populations (Appendix B) 
    Identifying variable dimensions that are >1 standard deviation above the mean (top16) or in the top quartile (top25) based on CORR value counts 
    Creating calculated field RANK to quickly separate top16 and top25 populations 

_(Note: source data is not included in this repository as it exceeds github's filesize limits)_

The research question is answered using an Ordinary Least Squares multilinear regression model (Appendix C). Using a 30/70 train/test split on the full dataset, PEHRUSLT (usual hours worked per week at all jobs; 0-150 continuous) is analyzed against rank, year, and telework variables. The model’s RMSE is 10.4 hours and the mean error of the training model’s predictions against the test set is 5.5 hours. According to this model, RANK is statistically significant in predicting hours worked across the dataset, meaning that the target populations do participate in the workforce at different rates than the whole population. 

Duplicate models run on the top16 and top25 (inclusive) subsets reveal that for the targeted populations, telework is a statistically significant predictor of hours worked, with TELECOVID (Apr 2020-Oct 2022) telework rates the strongest positive indicator. The R squared of the models varied from 0.002-0.008, meaning telework access is not the primary reason workforce participation varies across the dataset or subsets, but it has a larger effect on the target populations (TELECOVID_1 +4.6 hours for top16 populations vs.  +1.6 for full dataset).  

A limitation of this study is the type of telework data available. ATUS is representative of the entire United States population, but telework data as a standalone question has only been included since late 2022. Additional data from questions asked about teleworking during and before the COVID-19 pandemic are used in this study, but do not have as many valid responses. More detailed information about telework arrangements (hybrid vs. fully remote, temporary vs. long-term) would provide a more nuanced understanding of how telework impacts workforce participation. 

Suggestions for further study include: 

    The impact of telework on workforce participation by industry or job type 
    Projected candidate pool for a position if posted as telework vs. in-person
    Projected team makeup if telework is allowed vs. in-person only 

Across the board, positive telework status is associated with increased workforce participation at a statistically significant level. This effect is stronger in the key populations, with the largest effect among those who are disabled, White-Black or White-Asian, have a household >7 people, and married couples where at least one person is currently in the Armed Forces.  

These findings have several key implications for businesses. Expanding telework opportunities allows businesses to tap into a labor market that is otherwise limited or unable to access those positions. Fully remote positions allow companies to hire people who don’t live near their physical work location. Telework is more inclusive of disabled populations, which can increase retention, since anyone can become disabled at any time. A company that is prepared and open to flexible working arrangements is much better positioned to retain talent than one with limitations on how and where people work. Finally, many identified populations are underrepresented in corporations. Companies that are trying to be more representative of the populations they serve or incorporate perspectives that are different from the industry mainstream can find a different, more robust candidate pool with a telework position than they would with an in-person one. 

## Appendix A: Variables

### Key:
id-unique identifier\
cat-categorical variable\
cont-continuous variable\
cont>cat-continuous variable converted (binned) into categorical variable\
calculated-calculated variable\
1-used for data cleaning/formatting\
2-used for population selection\
3-used for regression\
4-included for supplemental illustration only

TUCASEID	id	1\
TULINENO	id	1\
HRYEAR4	id	1,2,3\
PEAFNOW	cat	1\
PEMLR	cat	1,2,3\
PEHRWANT	cat	1,2\
PESCHENR	cat	1\
PEDWWNTO	cat	1,2\
TELE2019	cat	1,2,3\
TELECOVID	cat	1,2,3\
TELENOW	cat	1,2,3\
EMPA	calculated	1\
EMPB	calculated	1\
EMPC	calculated	1\
EMPD	calculated	1\
EMPSTAT	calculated	1,2\
TELEALL	calculated	1,2\
CORR	calculated	1,2\
GESTFIPS	cat	2\
GTMETSTA	cat	2\
HEHOUSUT	cat	2\
HETENURE	cat	2\
HRHTYPE	cat	2\
HRNUMHOU_c	cont>cat	2\
HUSPNISH	cat	2\
PEABSRSN	cat	2\
PEAFEVER	cat	2\
PECERT1	cat	2\
PECERT3	cat	2\
PECYC	cat	2\
PEDIPGED	cat	2\
PEDISDRS	cat	2\
PEDISEAR	cat	2\
PEDISEYE	cat	2\
PEDISOUT	cat	2\
PEDISPHY	cat	2\
PEDISREM	cat	2\
PEERNCOV	cat	2\
PEERNHRY	cont	2\
PEGRPROF	cat	2\
PEHSPNON	cat	2\
PEMARITL	cat	2\
PEMJOT	cat	2\
PESEX	cat	2\
PRCITSHP	cat	2\
PRDISFLG	cat	2\
PRERNHLY_c	cont>cat	2\
PRMARSTA	cat	2\
PRNMCHLD_c	cont>cat	2\
PRTAGE_c	cont>cat	2\
PTDTRACE	cat	2\
PUAFEVER	cat	2\
PUBUS1	cat	2\
PEDW4WK	cat	4\
PEDWAVL	cat	4\
PEDWAVR	cat	4\
PEDWLKO	cat	4\
PEDWWK	cat	4\
PEHRACT1	cat	4\
PEHRACT2	cat	4\
PEHRACTT	cat	4\
PEHRAVL	cat	4\
PEHRFTPT	cat	4\
PEHRRSN1	cat	4\
PEHRRSN2	cat	4\
PEHRRSN3	cat	4\
PEHRUSL1	cont	4\
PEHRUSL2	cont	4\
PEHRUSLT	cont	3\
PEJHWKO	cat	4\
PENLFACT	cat	4\
PENLFJH	cat	4\
PENLFRET	cat	4\
PERET1	cat	4\
PRABSREA	cat	4\
PREMP	cat	4\
PREMPHRS	cat	4\
PREMPNOT	cat	4\
PRFTLF	cat	4\
PRHRUSL	cat	4\
PRNLFSCH	cat	4\
PRPTHRS	cat	4\
PRPTREA	cat	4\
PRSJMJ	cat	4\
PRWKSCH	cat	4\
PRWKSTAT	cat	4\
PRWNTJOB	cat	4\
PTCOVID1	cat	1,2\
PTCOVID2	cat	4\
PTCOVR1	cat	1,2\
PTCOVR2	cat	4\
PTCOVR3	cat	1,2\
PTCOVR4	cat	4\
PUABSOT	cat	4\
PUDIS	cat	4\
PUDIS1	cat	4\
PUDIS2	cat	4\
PURETOT	cat	4\
PUWK	cat	4\
rank	calculated	1,3	

## Appendix B: Target Populations

### Top16 – Variables >1 standard deviation above mean in CORR=1 rate

For each: Variable_dimension; % of variable where CORR=1; Description 

PEDISDRS_1 	86.03% 	disability: difficulty dressing or bathing \
PEDISOUT_1 	81.29% 	disability: difficulty running errands or going out alone \
PEDISPHY_1 	76.69% 	disability: serious difficulty walking or going up stairs \
PTDTRACE_6 	72.85% 	white-Black \
PTDTRACE_8 	70.22% 	white-Asian \
PEDISREM_1 	68.69% 	disability: serious cognitive or memory difficulties \
HRHTYPE_2 	67.19% 	husband/wife primary family in Armed Forces \
PRDISFLG_1 	64.84% 	any disability \
HRNUMHOU_c_6 	64.73% 	household size >9 \
PEDISEYE_1 	64.24% 	disability: serious difficulty with vision \
HRNUMHOU_c_5 	63.38% 	household size 7-8 

### Top25 – in addition to above; variables in the top quartile of CORR=1 rate

For each: Variable_dimension; % of variable where CORR=1; Description

HRNUMHOU_c_4 	61.49% 	household size 5-6 \
PTDTRACE_7 	61.31% 	white-Native \
HRHTYPE_4 	59.41% 	female civilian head of household \
PRNMCHLD_c_1 	58.83% 	no children \
HEHOUSUT_5 	56.48% 	mobile home or trailer with no permanent rooms \
GESTFIPS_28 	55.77% 	MS \
HEHOUSUT_6 	54.73% 	mobile home or trailer w/room(s) added \
HETENURE_2 	54.51% 	rent home \
PTDTRACE_2 	54.38% 	Black \
PTDTRACE_3 	54.13% 	Native \
GESTFIPS_56 	54.10% 	WY \
GESTFIPS_2 	54.00% 	AK \
HETENURE_3 	53.99% 	housing not owned and no cash rent paid \
PRCITSHP_1 	53.93% 	US-born citizen \
GESTFIPS_21 	53.91% 	KY \
GESTFIPS_23 	53.85% 	ME \
GESTFIPS_22 	53.72% 	LA \
GESTFIPS_19 	53.49% 	IA \
HRHTYPE_3 	53.43% 	civilian male head of household \
GESTFIPS_5 	53.29% 	AR \
PESEX_1 	53.26% 	male \
GTMETSTA_2 	53.20% 	non-metro area \
GESTFIPS_47 	53.06% 	TN \
PEHSPNON_1 	52.90% 	Hispanic/Latino \
GESTFIPS_40 	52.83% 	OK \
GESTFIPS_20 	52.78% 	KS \
HRNUMHOU_c_3 	52.72% 	household size 3-4 

## Appendix C: OLS Linear Regression Summaries

Screenshots of the Ordinary Least Squares linear regressions are provided below. If you are unable to access the screenshots, see the full data writeup in the thesis file.

![Screenshot of OLS linear regression on 30% train section of the full dataset](/OLS/ols_train.png)
