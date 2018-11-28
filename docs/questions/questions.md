
## List of Questions regarding the use of the CTC DGM

Questions are grouped into model use and model documentation. Possible clues/answers of where to find answers are listed below Question

### Model use questions

1. The dbase has referential integrity in place. Deleting data (eg stocks) will then result in cascade deletions. If the user relies on csv files then cascade deletion won't happen. Will the dgm function ok if there is orphaned data in the csv file approach?


2. How do we determine which files are required and which ones are not?
  How do we determine which fields are required and which ones are not?

3. What is the coded wire tag tables and how is that table associated with stocks?

4. Do you have to have multiple regional fisheries? i.e. is it possible to have a fisheries occurring in only one region?

5. The model does not produce output if you set the model to run more than 31 years. Why is that? and how do we fix it?

### Model documentation questions

1. We need a document that list all the model equations?
**A:** No, but the manual contain some information to that respect


2. It is common in population dynamics simulation studies for the first few simulated years to be very unstable.  In order to evaluate that, we would need to: run the model with constant mortalities and no recruitment deviations (set error in recruitment curve equal to 0). How do we do that?
**A:**So far the only solution is to run the model setting all variances to low numbers, e.g.: 0.01. The model does stop running if you set the SD to a very small number (e.g. 10e-08). 


3. What are the statistical distributions for all the process and observation error components of the model? -- there might be some 




