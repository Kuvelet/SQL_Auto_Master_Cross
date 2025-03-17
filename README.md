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

| CompanyPart# | CrossName         | CrossingNumber |
|-------------|------------------|----------------|
| PartID_1    | OEMcond          | 1a399853c      |
| PartID_1    | OEMcond          | e75eff849      |
| PartID_1    | OEMcond          | 1a379cd37      |
| PartID_1    | MOOG | 399084d8f      |
| PartID_1    | Mevotech | e7ffc5be7      |
| PartID_2    | OEMcond          | 4a9efa64f      |
| PartID_2    | MOOG | 21ea446a4      |
| PartID_2    | Mevotech | c04c54e6a      |
| PartID_3    | OEMcond          | 40236b986      |
| PartID_3    | OEMcond          | f4f124f70      |
| PartID_3    | OEMcond          | 40236b986      |
| PartID_3    | MAS  | fc6a76133      |
| PartID_3    | Dorman  | 6e50b8a21      |
| PartID_3    | MOOG | 189942055      |
| PartID_3    | Mevotech | 011f238fe      |

> **Note:** This table is a representative sample and does not include all records from the actual dataset. Data have been modified for confidentiality.

My plan is to restructure this data by creating dedicated columns for each unique **CrossName**, ensuring a more organized and accessible format. For each row, I will generate all possible combinations of values derived from the dataset. 

- If a **CrossName** is not available for a particular entry, it will be represented as **null** to maintain data integrity.  
- In cases where multiple **CrossNames** exist for the same brand, each unique combination will be represented in separate rows. This approach ensures that every possible permutation is captured, providing a comprehensive view of the data. 

By restructuring the data in this way, the **sales team** will be able to efficiently query and retrieve information from customer **RFQs (Requests for Quotations)**. This will streamline the process of identifying the relevant cross-references and enhance the team's ability to respond to customer inquiries with greater accuracy and speed.

Below is the sample output wanted using the given sample data

| CompanyPart# | Moog | Mevotech | Dorman | Mas | OEMcond                                       |
|--------------|------------------|-------------------|------------------|------------------|----------------------------------------------|
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | 1a399853c                                    |
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | e75eff849                                    |
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | 1a379cd37                                    |
| PartID_2     | 21ea446a4         | c04c54e6a         |    NaN     | NaN              | 4a9efa64f                                    |
| PartID_3     | 189942055         | 011f238fe               | 6e50b8a21        | fc6a76133        | f4f124f70                                    |
| PartID_3     | 189942055         | 011f238fe               | 6e50b8a21        | fc6a76133        | 40236b986                                    |
| PartID_3     | 189942055         | 011f238fe               | 6e50b8a21        | fc6a76133        | 40236b986                                    |

> **Note:** This table is a representative sample and does not include all records from the actual dataset. Data have been modified for confidentiality.



