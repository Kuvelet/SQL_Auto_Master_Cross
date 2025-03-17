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

Here's a snapshot of the raw data. I plan to restructure it by creating columns for each unique CrossName and generating rows for every possible combination of values from the dataset.

| CompanyPart# | CrossName         | CrossingNumber |
|-------------|------------------|----------------|
| PartID_1    | OEMcond          | 1a399853c      |
| PartID_1    | OEMcond          | e75eff849      |
| PartID_1    | MOOG | 399084d8f      |
| PartID_1    | OEMcond          | 1a379cd37      |
| PartID_1    | OEMcond          | 4a63622c3      |
| PartID_1    | OEMcond          | 1a399853c      |
| PartID_1    | OEMcond          | ca65b1921      |
| PartID_1    | OEMcond          | c0c572051      |
| PartID_1    | OEMcond          | e75eff849      |
| PartID_1    | CrossbrandType_14 | e7ffc5be7      |
| PartID_1    | OEMcond          | 73b32d5a8      |
| PartID_2    | OEMcond          | 4a9efa64f      |
| PartID_2    | CrossbrandType_2  | 313b64e58      |
| PartID_2    | MOOG | 21ea446a4      |
| PartID_2    | OEMcond          | 9535ddf65      |
| PartID_2    | OEMcond          | aad951cac      |
| PartID_2    | OEMcond          | 4a9efa64f      |
| PartID_2    | OEMcond          | 714ba50e4      |
| PartID_2    | OEMcond          | c9ab5492c      |
| PartID_2    | OEMcond          | e733a8698      |
| PartID_2    | CrossbrandType_14 | c04c54e6a      |
| PartID_2    | OEMcond          | dbc852d44      |
| PartID_2    | OEMcond          | 83efa47d3      |
| PartID_2    | OEMcond          | e55c262cc      |
| PartID_2    | CrossbrandType_38 | dba70e8e7      |
| PartID_3    | OEMcond          | 40236b986      |
| PartID_3    | CrossbrandType_2  | fc6a76133      |
| PartID_3    | CrossbrandType_5  | 6e50b8a21      |
| PartID_3    | CrossbrandType_11 | 189942055      |
| PartID_3    | OEMcond          | f4f124f70      |
| PartID_3    | OEMcond          | 40236b986      |
| PartID_3    | OEMcond          | 891efb4c6      |
| PartID_3    | OEMcond          | dba70e8e7      |
| PartID_3    | CrossbrandType_14 | 011f238fe      |
| PartID_3    | OEMcond          | cd658b895      |
| PartID_3    | OEMcond          | 6591b474a      |
| PartID_3    | OEMcond          | 694cfcbfd      |
| PartID_3    | CrossbrandType_38 | 714ba50e4      |

---

| CompanyPart# | CrossbrandType_11 | CrossbrandType_14 | CrossbrandType_2 | CrossbrandType_5 | OEMcond                                       |
|--------------|------------------|-------------------|------------------|------------------|----------------------------------------------|
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | 1a399853c                                    |
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | e75eff849                                    |
| PartID_1     | 399084d8f         | e7ffc5be7         | NaN              | NaN              | 1a379cd37                                    |
| PartID_2     | 21ea446a4         | c04c54e6a         | 313b64e58        | NaN              | 4a9efa64f                                    |
| PartID_3     | 189942055         | NaN               | fc6a76133        | 6e50b8a21        | 40236b986                                    |
| PartID_4     | 7d848cc58         | NaN               | 8bade43b4        | 5901f87bf        | 8f47b0f9a                                    |


