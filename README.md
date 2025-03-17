# SQL Approach to Developing a Master Cross for Auto Part Catalogs

### Table of Contents

-[Project Overview](#project-overview)

-[Data Sources](#data-sources)

-[Tools](#tools)

-[Data Context & Preparation](#data-context--preparation)

-[Transformation](#transformation)

-[Results & Findings](#results--findings)

-[Next Steps](#next-steps)

### Project Overview

In the automotive parts industry, especially for manufacturers and sellers, accurate part identification is crucial. With numerous car makes, models, and submodels—each potentially varying by region—ensuring the correct part match is essential. OEM (Original Equipment Manufacturer) numbers serve as key references for identifying exact parts and are widely used when creating aftermarket alternatives. When selling aftermarket parts, customers often reference OEM numbers or, in some cases, part numbers from competing aftermarket brands when requesting quotes. Therefore, maintaining a comprehensive catalog that includes all available cross-references from original manufacturers and competitors is vital for any aftermarket seller. This cross-referencing system, often compiled into a "Master Cross," enhances catalog accuracy, improves customer service, and simplifies the quoting process. In this study, I will demonstrate how to create an effective Master Cross for an aftermarket automotive manufacturer, providing a valuable resource to share with clients. Given that major industry players manage catalogs with 15,000 to 35,000 unique parts, building an organized and detailed Master Cross is essential for maintaining a competitive edge.

### Data Sources

The Auto_Cross_Data_Anonymous CSV file contains over 150,000 rows of data. While this dataset is based on real-life examples, all information has been anonymized to ensure confidentiality.

### Tools

- Microsoft SQL Server

### Data Context & Preparation

The provided dataset consists of three primary columns: `CrossName`, `CrossingNumber`, and `CompanyPart#`. The `CrossName` column categorizes the type of cross-branding associated with each part, while the `CrossingNumber` is to be a unique alphanumeric identifier,  representing a hashed or encrypted value for each cross-reference. The `CompanyPart#` originally contained specific part numbers associated with a company but has now been anonymized to generic identifiers in the format of PartID_1, PartID_2, and so on. This anonymization helps protect sensitive product information while maintaining the uniqueness of each part for analysis or processing purposes.

***Import the data as a flat file into SQL Server to initiate the transformation process.**

### Transformation

Here’s an overview of the raw dataset.

| CompanyPart#     | Crossing_Number | CrossName      |
|---------|-------------|-----------|
| PartID_1 | DRL451     | ACDelco   |
| PartID_1 | ZEF1066    | Beru      |
| PartID_1 | ZEF1076    | Beru      |
| PartID_1 | ZEF787     | Beru      |
| PartID_1 | 4158       | Bougicord |
| PartID_1 | 600/217    | Bremi     |
| PartID_1 | LS99/190   | Champion  |
| PartID_1 | 7683       | Doduco    |
| PartID_1 | 4.8616     | Facet     |
| PartID_1 | BK931530   | IISI      |
| PartID_1 | FP-1265    | Ilkerler  |
| PartID_1 | CCO.74240  | Mecra     |
| PartID_1 | 300890787  | OEMCond   |
| PartID_1 | 77776810   | OEMCond   |
| PartID_1 | 7776810    | OEMCond   |
| PartID_1 | 776810     | OEMCond   |
| PartID_1 | 1810       | Unuvar    |
| PartID_2 | 600/458    | Bremi     |
| PartID_2 | 7674       | Doduco    |
| PartID_2 | 4.9367     | Facet     |
| PartID_2 | BK931540   | IISI      |
| PartID_2 | FP-1283    | Ilkerler  |
| PartID_2 | CCO.74420  | Mecra     |
| PartID_2 | 46427497   | OEMCond   |
| PartID_2 | 46563068   | OEMCond   |
| PartID_2 | 4655306    | OEMCond   |
| PartID_2 | 1497       | Unuvar    |
| PartID_3 | ZEF1201    | Beru      |
| PartID_3 | 4206       | Bougicord |
| PartID_3 | 600/428    | Bremi     |
| PartID_3 | 7826       | Doduco    |
| PartID_3 | 4.9505     | Facet     |
| PartID_3 | BK931550   | IISI      |
| PartID_3 | FP-1285    | Ilkerler  |
| PartID_3 | CCO.74410  | Mecra     |
| PartID_3 | RC-FT1203  | NGK       |
| PartID_3 | 46474814E  | OEMCond   |
| PartID_3 | 46425912   | OEMCond   |
| PartID_3 | 46474814   | OEMCond   |
| PartID_3 | 46743086   | OEMCond   |
| PartID_3 | 1814       | Unuvar    |


> **Note:** This table is a representative sample and does not include all records from the actual dataset. Data have been modified for confidentiality.

My plan is to restructure this data by creating dedicated columns for each unique **CrossName**, ensuring a more organized and accessible format. For each row, I will generate all possible combinations of values derived from the dataset. 

- If a **CrossName** is not available for a particular entry, it will be represented as **null** to maintain data integrity.  
- In cases where multiple **CrossNames** exist for the same brand, each unique combination will be represented in separate rows. This approach ensures that every possible permutation is captured, providing a comprehensive view of the data. 

By restructuring the data in this way, the **sales team** will be able to efficiently query and retrieve information from customer **RFQs (Requests for Quotations)**. This will streamline the process of identifying the relevant cross-references and enhance the team's ability to respond to customer inquiries with greater accuracy and speed.

Below is the sample output wanted using the given sample data

| GTB      | OEMCond   | BERU    |   Bougicord | Bremi   | Mecra     |   ValuCraft | ACDelco   |   B_B_Long |   B_B | BoschCond   |   Brecav |   Carol | Champion   |   Delphi |   Doduco |   Facet | IISI     | Ilkerler   |   Intermotor |   Janmor |   Magneti |   Moroso | NGK       |   ProSpark |   Sentech |   SMP |   Tesla |   Unuvar |
|:---------|:----------|:--------|------------:|:--------|:----------|------------:|:----------|-----------:|------:|:------------|---------:|--------:|:-----------|---------:|---------:|--------:|:---------|:-----------|-------------:|---------:|----------:|---------:|:----------|-----------:|----------:|------:|--------:|---------:|
| PartID_1 | 300890787 | ZEF787  |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 77776810  | ZEF787  |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 7776810   | ZEF787  |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 776810    | ZEF787  |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 300890787 | ZEF1066 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 77776810  | ZEF1066 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 7776810   | ZEF1066 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 776810    | ZEF1066 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 300890787 | ZEF1076 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 77776810  | ZEF1076 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 7776810   | ZEF1076 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_1 | 776810    | ZEF1076 |        4158 | 600/217 | CCO.74240 |         nan | DRL451    |        nan |   nan | nan         |      nan |     nan | LS99/190   |      nan |     7683 |  4.8616 | BK931530 | FP-1265    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1810 |
| PartID_2 | 46427497  | nan     |         nan | 600/458 | CCO.74420 |         nan | nan       |        nan |   nan | nan         |      nan |     nan | nan        |      nan |     7674 |  4.9367 | BK931540 | FP-1283    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1497 |
| PartID_2 | 46563068  | nan     |         nan | 600/458 | CCO.74420 |         nan | nan       |        nan |   nan | nan         |      nan |     nan | nan        |      nan |     7674 |  4.9367 | BK931540 | FP-1283    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1497 |
| PartID_2 | 4655306   | nan     |         nan | 600/458 | CCO.74420 |         nan | nan       |        nan |   nan | nan         |      nan |     nan | nan        |      nan |     7674 |  4.9367 | BK931540 | FP-1283    |          nan |      nan |       nan |      nan | nan       |        nan |       nan |   nan |     nan |     1497 |
| PartID_3 | 46474814E | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | 986357181   |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46425912  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | 986357181   |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46474814  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | 986357181   |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46743086  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | 986357181   |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46474814E | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | B181        |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46425912  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | B181        |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46474814  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | B181        |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |
| PartID_3 | 46743086  | ZEF1201 |        4206 | 600/428 | CCO.74410 |         nan | nan       |        nan |   nan | B181        |      nan |     nan | nan        |      nan |     7826 |  4.9505 | BK931550 | FP-1285    |          nan |      nan |       nan |      nan | RC-FT1203 |        nan |       nan |   nan |     nan |     1814 |

> **Note:** This table is a representative sample and does not include all records from the actual dataset. Data have been modified for confidentiality.



