# house-disbursement-data
Data store of U.S. House of Representatives Expenditure Data
d3.js vizulization using this data at [Hamilton Project](https://github.com/meagdoh/hamilton-project)

## Analyzing Data for Social Good
Building off of my capstone project, Hamilton Project (which was based of the initial research data cleaning from the OpenGov Foundation and Sunlight Foundation, respectively), I carefully analyzed over 600,000 transaction records from the House of Representatives' Statements of Disbursements.

All of the documentation on cleaning the data is stored in this Jupyter notebook [link] while some preliminary visualizations are saved in the README of the repo of the original application (built w. D3 and React)

### Lessons Learned
Data and design principles [link to e-book about data] // how to teach Visualizing Data

### Limitations
Due to the inconsistent data formats, missing values and X, I spent a lot of time in circles around which areas i wanted to focus on. Was I building an application like U.S. spending.gov or [Steve Ballmer's project] for end users to query a clean, consolidated databased OR was there a specific story I could tell using visualizations - taking one more step that the user didn't have to.

I settled a bit on both. The first (querying a database) is currently a manual upload with a few simple rules (that eventually could be in a script)


### Adding new data
Download .csv from House of Disbursements site, save as `CongressSession_Year_QuarterNum` e.g. `114_2016_Q1.csv`

### Rules for cleaning data
1) Confirm Headers Names: [ ORGANIZATION, PROGRAM, SORT_SUBTOTAL_DESCRIPTION, SORT_SEQUENCE, TRANSACTION_DATE, DATA SOURCE, DOCUMENT, VENDOR_NAME, PERFORM_START_DT, PERFORM_END_DT, DESCRIPTION, AMOUNT
]

2) [SORT_SEQUENCE] = [DETAIL] this removes the subtotals and total rows from the data set.

### Tips for Analyzing Data
When looking at amount totals by vendor, limit to TOP 100. There are 33,485 unique `VENDOR_NAME` NOTE: As previously documented, vendors are often listed under more than one name. This is a future opportunity to manually/use fuzzy matching to match and consolidate vendor names.

If you are analyzing personnel data, mark: [SORT_SUBTOTAL_DESCRIPTION] does *not* equal [BENEFITS TO FORMER PERSONNEL; PERSONNEL BENEFITS; PERSONNEL COMPENSATION] this removes the personnel expenses from the data set. For purposes of analyzing vendor expenditures, personnel needs not to be included


#### Key
ORGANIZATION // Office
PROGRAM // General category of expenses
SORT_SUBTOTAL_DESCRIPTION // Specific category of expenses
SORT_SEQUENCE // Total, Subtotal, Detail
TRANSACTION_DATE
DATA SOURCE
DOCUMENT
VENDOR_NAME // Name of vendor. NOTE: Employee name is stored in VENDOR_NAME when PERSONNEL_COMPENSATION
PERFORM_START_DT
PERFORM_END_DT
DESCRIPTION
AMOUNT // in USD

### Research Questions

For personnel data:
- How many House staffers have tech-related titles?

For non-personnel data:
- Who are the top 100 vendors in the House over time?
- How has spending categories changed over time?

### Data Findings
- No. of Transactions Analyzed: 635,721

- As noted in past work, `VENDOR_NAME` `Unknown` is the biggest category of spending. The `DATA_SOURCE` for all `Vendor` equals `unknown` originate from the `GL` (General Ledger). And the highest transaction category is `DC TELECOM TOLLS`_($11,014,427)_

- `VENDOR NAME` equals `CITIBANK GOV CARD SERVICE` _($18,606,367)_ is the top vendor after `NULL`. Assumption: Tech purchases are made on P-cards but the research shows these cards are used mostly for `COMMERCIAL TRANSPORTATION`.

- When analyzing total expenditures by `DESCRIPTION` we find the top category fall into `TECHNOLOGY SERVICE CONTRACTS` _($37,712,578)_. In this category, there are the well-known technology vendors like *iConstituent* and *Fireside21*, but there are a few top vendors who you may not know:
-- MINERAL GAP DATA CENTER, _$1.9M_
-- COMPROBASE INC., _$1.5M_
-- ADVANCE DIGITAL SYSTEMS INC., _$1.5M_
For a full list, run query: [add query]

- Research often cites limited technology staff. A search for `DESCRIPTION` (in this case, defined as staff title) contains `TECH` OR `SYS ADMIN` results in approximately _200 staffers_ with these titles ranging from `Tech Policy Advisor` to `System Administrator`, _about X% of all staff total_ (taken from other data source)


### Next steps
- A signal tracker. By having this as a live dashboard, we can build in alerts if specific spending changes dramatically
