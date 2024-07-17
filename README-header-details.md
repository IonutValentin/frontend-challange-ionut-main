## Header Section Details

- **Title** "Alignment Overview"


- **Field "Total exposure"** 
  - A monetary amount
  - Example "10.000 EUR"
  - Semantics: This is the sum of all loans, i.e. all money that was lend from the banks to other business partners
  - Json: Can be calculated from various places, you choose
    - entry in `$.metadata.grossCarryingAmountTotal`
    - sum of `$.productListWithAlignme nt[0..*].product.grossCarryingAmount`

 
- **Field "Eligable amount"**
  - A monetary amount
  - Example "9.000 EUR"
  - Semantics: This is the fraction of the exposure that falls under the EU Taxonomy law. E.g. 1.000 EUR were lend to very small companies that do not fall under the EU Taxonomy regulation
  - Json: Can be calculated from various places, you choose
    - entry in `$.metadata.eligabilityTotal`
    - sum of `$.productListWithAlignment[0..*].alignmentKPIList[0..*].eligibilityValue`
    - sum of `$.productListWithAlignment[0..*].alignmentKPIList[0..*].dataPoints[0..*].eligableValue`


- **Field "Aligned amount"**
  - A monetary amount
  - Example "4.000 EUR"
  - Semantics: This is the fraction of exposure for which EU Tax regulation applies and for which it is proven that "it is green" (complies with EU Tax regulation)
  - Json: Can be calculated from various places, you choose
    - entry in `$.metadata.alignmentTotal`
    - sum of `$.productListWithAlignment[0..*].alignmentKPIList[0..*].alignmentValue`
    - sum of `$.productListWithAlignment[0..*].alignmentKPIList[0..*].dataPoints[0..*].alignmentValue`