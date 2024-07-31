
# MRR Optimized Credit System

This readme will provide additional technical guidance based on recent workshops related to the credit system in context of Broker Workbench and Company Workbench products.  Note, this type of exercise will likely be required again for Underwriter Workbench, Shape and other X-Workbench products. 



## The Primary Scenario

(1) Some broker user(s) have provided access to the Brokerage’s commercial book data, to which our BWB product performs Alpha anlaysis. 

(2) The analysis output of Alpha creates a universe of commercial opportunities (a list of companies that have been priced).

**NOTE:** Step 2 is the end of the 'Alpha' free trial, and requires a BWB enterprise subscription to continue onto step 3. 

(3) That universe or segments of that universe, from within BWB, can be ‘batch quoted’ in the markets to which they have appointments.

(4) This creates audiences that can be selected and ‘activated’ for a campaign. This, from a technical point of view, takes information from that quoteID and propagates that information into BWB and CWB for the purposes of now engaging the customer.  In the case of FRP, this is what also triggers actions and data to be shared with OneFRP.  To activate information into a campaign requires 1 digital profile credit per 1 company.  The credit is instantly consumed.  

(5) Once the company has been activated for campaign, that company can be re-quoted as many times as necessary by the broker using BWB, and all new information available via CWB (i.e., from the customer) will automatically flow through to BWB to use in any re-quoting activities.  Each BWB functionality or feature action the broker takes post activation is using the BWB subscription access to those features, so no additional credits are required for quoting purposes.  

(6) When the broker is ready to bind, 1 digital placement credit is required and will be applied or purchased at the point of bind.  This credit is also considered to be instantly consumed.



Below is a chart that illustrates the above:

![image](https://github.com/user-attachments/assets/63343250-c172-4955-9f0b-c37f2cd80277)


## Credit Consumption Dataframe

Using the chart above, we can now establish an information model that can be dynamically populated from BWB user experiences. 

**Campaign ID:** this ID gets assigned when a user in BWB defines a campaign.  A campaign should include a unique name, very likely the 'month' of the renewal focus, and a timeline in order to create.  

**Campaign Create Date:** when a campaign is saved for the first time, the date of creation is logged cannot be edited.  

**Campaign Start Date:** the user can define when the campaign will start and this can be changed as often or as much as the user wants.

**Campaign End Date:** the user can define when the campaign will end and this can be changed as often or as much as the user wants.

**Credit ID:** this is a generated unique ID that defines a credit that was consumed for a given company ID by the Broker ID at a specific date and time. 

**Credit Type:** this defines the class of credit.  Right now there are only three types of credits that we will be leveraging (digital profile, placement, risk selection).  There will be more credit types in the future. 

**Company ID:** this is a generated unique ID that get assigned to a unique company (end customer). 

**Broker UID:** this is a generated unique ID that get assigned to a unique broker workbench user. 

**Brokerage:** this is a generated unique ID that get assigned to a unique brokerage company (i.e., FRP). 

**Credit Consumption Date:** This is the date referenced in the illustration above, which is the date that a company was 'activated' for a campaign.  This occurs when a customer is taken from the universe and added to a campaign. 

**Investment Override:** At the end of each month, a process should occur, where the sales and accounting ops team have an opportunity to apply an 'investment override' to the credits that were consumed within that reporting period.  This can be used for a myriad of reasons, but as we are negotaiting pre-purchases, renewals or deliverying on very specific business cases, this user experience will enable the teams above to deliver on commitments and deals. An investment override being applied (i.e., value == 1) results in the credit price being set $0 prior to the payload being sent to account ops for reporting.  Lastly, while Sales ultimately manages this allocation or reconciliation process, a workflow for approval should occur that approves the overall balances of investment override versus submitted for payment/rev recognition. 

**Revenue:** This is price that has been agreed to for a given partner (i.e., FRP) applied to the credit that was consumed.  To facilitate, there should be a central pricing shcedule that is maintained by Sales and Accounting Ops. 

All of the data elements above have been defined in a mock dataframe, that is aligned to the use case provided in the illustration.  That dataframe is available in this repo and is called 'FRPcreditCampaignAllocation.csv'


## MRR Analysis ##

Using the dataframe defined above, we can then do additional functions to that data, which then creates a new set of data elements.  Below is a new dataframe and the elements of that dataframe defined.  Note, the MRR method and calculations were pulled from multiple SaaS accoutning references.

**Brokerage:** is the distinct brokerage values from df

**MRR Reporting Period:** is the distinct MRR Reporting Period values from df

**Digital Profile Credit Revenue Per User:** calculates the sum of ‘Revenue’  where ‘Credit Type’ == ‘Digital Profile’ divided by the distinct count of ‘Broker UID’ grouped by ‘MRR Reporting Period’

**Placement Credit Revenue Per User:** calculates the sum of ‘Revenue’  where ‘Credit Type’ == ‘Placement’ divided by the distinct count of ‘Broker UID’ grouped by ‘MRR Reporting Period’

**Total MRR Per User:** calculates the sum of ‘Revenue’  divided by the distinct count of ‘Broker UID’ grouped by ‘MRR Reporting Period’

**Monthly Active User Count:** is the count of distinct ‘Broker UID’ grouped by MRR Reporting Period

**MRR:** is ‘Total MRR Per User’ * ‘Monthly Active User Count’

The mock data from the above dataframe is available in the repo and is called 'mrr.csv'.  The Jupyter notebook called 'MRR Calculation.ipynb' applies all of the logic described above to the 'FRPcreditCampaignAllocation.csv' and produces the 'mrr.csv' output. 


