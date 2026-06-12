# EOV Spec Sheet Transformation Guide (DRAFT)

*This is a very early framework for capturing the process to transform EOV specification sheets into DwC criteria used in OBIS dataset filtering. Process is being co-developed by the OBIS DCG sub-group for D5.4.*

1. AI Prompt to transform speech sheet to DwC  
   1. Validate DwC terms  
2. Transform DwC into machine readable format (linkML)  
   1. Validate LinkML  
3. [Query the test data](https://github.com/iobis/obis-open-data#example-querying-the-dataset-locally-using-r)
   1. Create code for a specific pilot case  
      1. Will need to check things like date → e.g. if no eventDate then check Year/Month/Day  
   2. Deposit in GitHub Repo [https://github.com/iobis/eov-data-management](https://github.com/iobis/eov-data-management)
4. QC check the list of datasets
