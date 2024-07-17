
## Loan Table Details

Wireframe

![layout-example.png](layout-example.png)


JSON to Loan Table Mapping:

- **Row** ✅ mandatory
    - Every entry in `$.productListWithAlignment` is one row (this is the container that bundles loan information + esg information)


- **Column "Loan Id"**: ✅ mandatory
    - Show `$.productListWithAlignment[THIS ROW].product.contractId`


- **Column "Partner Name"**: ☑️ optional️
    - Show `$.productListWithAlignment[THIS ROW].product.borrower.name`


- **Column "Exposure"**: ☑️ optional️
    - Show `$.productListWithAlignment[THIS ROW].product.grossCarryingAmount`


- **Column "Aligned & Eligable Total "**: ☑️ optional️
    - Calculate the SUM OF ALL `$.productListWithAlignment[THIS ROW].alignmentKPIList[SUM ALL].eligibilityValue`
    - Calculate the SUM OF ALL `$.productListWithAlignment[THIS ROW].alignmentKPIList[SUM ALL].alignmentValue`
    - Show as "ALIGNED_TOTAL_AMOUNT of ELIGABLE_TOTAL_AMOUNT", Example "3.400.000 of 7.000.000 EUR"


- **Column "CCM Aligned & Eligable"**: ✅ mandatory
    - Calculate the ELIGABLE_CCM_VALUE as
        - The SUM OF `$.productListWithAlignment[THIS ROW].alignmentKPIList[SUM ALL].dataPoints[SUM SOME].eligableValue`
        - But only those where `..dataPoints[].environmentalObjective` is `CCM`
    - Calculate the ALIGNED_CCM_VALUE as
        - The SUM OF `$.productListWithAlignment[THIS ROW].alignmentKPIList[SUM ALL].dataPoints[SUM SOME].alignedValue`
        - But only those where `..dataPoints[].environmentalObjective` is `CCM`
    - Show `ALIGNED_CCM_VALUE of ELIGABLE_CCM_VALUE`, Example ("1.200 of 4.000 EUR")


- **Column "CCA Aligned & Eligable"**: ☑️ optional️
    - Show the same as in _"CCM Aligned & Eligable"_, but the selection criterion is `..dataPoints[].environmentalObjective` is `CCA`





## Datasource Details

The [JSON file loans.json](./public/taxonomy/loans/loans.json) might looks overwhelming, but actually you just need very few parts of it to complete the coding challenge and we provide exact formulas for using the data (see loans table above)

![](datasource-json-overview.png)

Relevant Sections of the JSON file:

* **Metadata**
  * `$.metadata`
  * From here you'll get useful values for the header section
* **Main List of Loans + ESG-Data**
  * `$.productListWithAlignment`
  * This is the main list. One Entry = One Row in your table
  * All other values are just aggregated values from below
* **General Loan data**
  * `$.productListWithAlignment[0..*].product`
  * Here you'll find general information about the loan, e.g. how much money was given to the business partner
* **ESG Datapoints for a Loan**
  * `$.productListWithAlignment[0..*].alignmentKPIList[0..*].dataPoints[0..*]`
  * Here you find the ESG data that must be summed up (see formulas below)

