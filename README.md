# CampaignContributionCode
**This repo contains code which will take data from Google Civic API and Maplight API and output a data structure containing aggregations of campaign contributions(min, max, avg, total) for each political party**

**Link for Data Source:**
* Google_Civic_API: https://www.googleapis.com/civicinfo/v2/representatives
* Maplight_API: https://maplight.org/data_guide/contribution-search-api-documentation/

**DataAnalysis_Scope**
* For demo purpose, I've only shown 2020 $contribution amount for the representatives for whom data can be found in Maplight API and google civic API.
* Code can easily be readjusted to incorporate historical $contributionAmount data as per the requirement.

**Code_Logic/Procedure**
* We'll have to join google civic API dataframe and Maplight API data on the basis of candidate names to get candidate names and their party name from Google API data and corresponding $_transactionAmount from Maplight data.
* Once data is combined in panda dataframe, we'll use min(), max(), mean(), sum() to get the 'Min_$ContributionAmount', 'Max_$ContributionAmount', 'Average_$ContributionAmount', 'Total_$ContributionAmount' from combined dataframe and we'll group it by political party name.

**Data_Limitations & Issues**
* Althogh Google Civic API is publicly available dataset, it has limited data about candidates. 
  * One of the two primary endpoints of the Google Civic Information API returns data about elections (the other returns   elected representatives). 
    * Elections information is in two parts: 
      * The elections query returns a list of all upcoming elections and optionally takes an address as a parameter to narrow the results. And during certain elections, the voter info query lets you look up polling places, early vote and drop off locations, data on candidates and referendums, and other election information.

* **Issue#1: Limited Data:** Since we need only candidate names and their political party names for demo purpose, we'll use google representatives data API.

* ***How to improve:** To get more accurate data on candidate names, contribution amount, their political party name, etc.; Ballotpedia provides robust dataset via API for a small fee.*

* **Issue#2: CandidateName mismatch:** When we're passing candidate names from google API to Maplight API URL to query $contribution amount, We're seeing some blank rows because of name mismatch, *such as Donald J. Trump is the candidate name in Google representative API while it Donald Trump in Maplight API.*

```
link = "https://api.maplight.org/maplight-api/fec/contributions?candidate_name=" +str(name[i].split()[0]) + "%20" + str(name[i].split()[-1])
data = requests.get(link).json()
``` 
* ***How to improve:** we can use regex to resolve this issue but for this demo purpose, I've not incorporated regex logic.*

* **Issue#3: TransactionAmount Discrepency:** Few rows contains negative $amount which I've displayed in output. I've corrected it by using ```abs()``` function in ```updatedQueryData()``` udf to convert all negative values to absolute $ values.

* **Issue#4: Party Name Mismatch:** Certain candidates have more than one party name assigned to them for a given year. In order to avoid this discrepancy for this demo, I've put logic by googling correct party for the respective candiates. In real-time enviroment, we'll have to use more reliable and robust data and then we can write logic to validate this information.

**How to run/test CNN_DemoCode.ipynb notebook**
* We'll need to set-up jupyter notebook and anaconda environment to run/test this notebook.
