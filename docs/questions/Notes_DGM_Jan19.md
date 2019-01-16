# Summary on discussion between the CTC AWG and Li Ding 

The discussion started with a review of the list of questions that the CTC AWG had prepared before this meeting. (See list of questions and Li’s responses below). Additional information was discussed during the meeting. Discussion notes listed in the next section.

## Data related questions: 

List of Questions regarding the use of the CTC DGM
Questions are grouped into model use and model documentation.

### Model use questions

1.  The Access database has referential integrity in place. Deleting data (e.g. stocks) will result in cascade deletions. If the user relies on csv files then cascade deletion won't happen. Will the DGM function ok if there is orphaned data in the csv file approach?

**A (LD):** Deleting data from Access can be achieved by using tools in the database application module. Please see the operation manual. Incomplete data cleaning may lead to a program crash. 

**Group discussion:** whenever changing input files to the DGM, the best approach is to use the database and use the data tool to modify the input and design your own scenarios. Then use the export function to export the csv versions of the files.  The csv versions of the input files can be used to validate values entered as input. 
Before modifying the data base, make sure to save an unchanged copy of the example database in another (safe) directory. 

2.  How do we determine which files are required and which ones are not? How do we determine which fields are required and which ones are not?

**A (LD):** All the input files in the release package and all the fields in the files are needed.

**Group discussion:** Field “Prop Unmarked fish removed” (in the mark selective sheet): given all the variation in the fishery regulations, and the fact that algorithms related to marked fish could not be found,  keeping track of unmarked fish was the solution. It is essentially the allowable harvest rate for unmarked fish in the fishery

3.  What is the coded wire tag table and how is that table associated with stocks?

**A (LD):** As far as I know, the CWT table is only for resampling. As can be seen in the file (SamplingRates.csv),  a sampling rate is specified for each fishery and time (year/period). The sampling module works on catch data generated from DGM, and the catch data are stock specific. The output of the sampling module contains the sampled proportion for each stock in a fishery.
**Group discussion:** The CWT table is no longer needed. 

4.  Do you have to have multiple regional fisheries? i.e. is it possible to have a fisheries occurring in only one region?

**A (LD):** It is possible, but it may need some changes in the code.

**Group discussion:** Question is asking we can have a model with a single AABM or ISBM fishery. Further, how do regions work in the model? How does concept of region relate to concept of fishery, spatially. Just trying to understand concepts. Is there a one to one relationship between regions and fisheries

In principle, having a one region model is possible when single pool option is selected, but  Li is currently working on bug that relates to this question. DGM was designed to mimic the current PST management system.  ‘Region’ is synonymous with the AABM fisheries.  Multiple fisheries operating within the same region are operating on the same pool of fish  (i.e. with the same stock composition.). Fisheries are not acting independently of one another.
One fishery can only be contained in one region. One region might have more than one fishery type. To delete a region, delete all fisheries that occur in that region. Terminal fisheries have their own region. 

5.  Can the DGM be run with a scenario constructed with ISBM fisheries only?

**A (LD):** Yes, the user just needs to edit the CTCFisheries.csv file and related data.

**Group discussion:** Trying to come up w/ simplified scenarios to see how DGM works. Can we just have one or two ISBM fisheries to explore scenarios? Seems as though answer is “yes”, but have to set catch limits in AABM fisheries to 0.
 AABM and ISBM fisheries are separate and independent in the DGM.  At least one AABM fishery is needed, and must be set as the ‘Abundance Driver’ (see the Fishery table)
 Driver fishery definition is the fishery that determines allocation (set the AI). 

6.  Can the DGM be run with a scenario constructed with AABM fisheries only?

**A (LD):** Yes, the same as above, but at least one of the AABM fisheries has to be the abundance index driver.

**Group discussion:** Similar to question 4 and 5, the user just needs to edit the CTCFisheries.csv file and related data. 

7.  The model does not produce output if you set the model to run more than 31 years. Why is that? and how do we fix it?

**A (LD):** It was because the original test data were available only for 31 years, but it can be changed to depend on data availability.

8.  The DGM produces incidental mortalities (drop-off and release mortality).  In DGM output, IM is differentiated by mark status and tag status but not by size relative to size limit regulations that may be specified for a fishery (sublegal size, legal size and extralegal size).  Especially when a scenario includes one or more MSF, IM categories by size seem necessary.  Is it possible to extract IM by size category within the DGM calculations?

**A (LD):** This is possible, but needs code changes.

**Group discussion:** Incidental mortality is not size-specific at this point, the code needs to be changed to allow that. Li was going to show us the location in the code of the Incidental mortality calculations but we ran out of time. 

9.  It is common in population dynamics simulation studies for the first few simulated years to be very unstable. In order to evaluate that, we would need to: run the model with constant mortalities and no recruitment deviations (set error in recruitment curve equal to 0). How do we do that? Is it possible to turn off all error terms in the DGM?  [CTC note: So far the only approach identified is to run the DGM setting all variances to low numbers, e.g.: 0.01. We found that the DGM stops running if you set the SD to a very small number (e.g. 10e-08) or to 0.  Is there something we are doing wrong?  Can you conceive of a way to implement the ability to turn off all inputs associated with a sampling distribution through selecting an option made available in the user interface?

**A (LD):** This is most likely due to the random number generator. I’m not sure about the limit of variance in the generator, but will look into the library. However, a more reliable way to do this is to make code changes so that when the program sees a zero variance, it generates constant values.

**Group discussion:** Question is about the number of simulation years and running a deterministic model in the DGM. 

Tried to turn down SE to near 0, but model crashes after 2 or 3 years when stochasticity is turned off. Might be able to run 0.001. But smaller values will not work.  Need to send Li input files to look at issue. Ask Bob to send control file to Li (already done).  Behavior of DGM was that it would keep going (infinite loop?). Li needs to reproduce on his machine and is currently working on fixing this bug. 
 
Li is currently working on expanding the number of years in the model.
On a related note, If only the first year of parameters/data is populated the model assumes that all quantities are projected into the future with parameters that are constant and equal to the first year of data. 


Model documentation questions
1.  Documentation that lists all the model equations is needed.  Are there equations/algorithms in the DGM that are not yet illustrated in the DGM documentation? All the equations are covered in the report, or relevant references are provided.

2.  What are the statistical distributions for all the process and observation error components of the model? -- there might be some [Note: Gaussian distribution mentioned in the DGM manual but are other distributions used for some variables as well?]Currently, only Gaussian distribution is used. I have also implemented the Kumaraswamy distribution (see the report), but it is not being used at this moment. This issue can be revisited if desired.

3.  Kendra Holt and Brooke Davis identified some issues with the DGM during their translation of the DGM code into R. Were these issues modified or fixed in the DGM? Yes

**Mark retention:** In the mark retention table, even if a fishery is not MSF, the bag limits and exploitation rate limits are still valid (i.e. the columns in the table are still used as valid input regardless of the values of isMSF)
CWT table: The CWT table is not needed to run the DGM model. We can delete that table

**Group survival:** The group survival table is assigned to groups that are composed of different stocks. For all stocks within the same group the survival is correlated following a truncated multivariate normal distribution with correlation coefficient specified by the user. 

**DGM development and code – sharing:**  We mentioned to Li, the intention to host the code on gitlab. Li agreed and we agreed to give him access to the DGM project on gitlab.

