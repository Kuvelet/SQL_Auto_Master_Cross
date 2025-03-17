# SQL Approach to Developing a Master Cross for Auto Part Catalogs

### Table of Contents

-[Project Overview](#project-overview)

-[Data Sources](#data-sources)

-[Tools](#tools)

-[Data Context & Preparation](#data-context--preparation)

-[Analysis](#analysis)

-[Results & Findings](#results--findings)

-[Next Steps](#next-steps)

### Project Overview

In the automotive parts industry, especially for manufacturers and sellers, accurate part identification is crucial. With numerous car makes, models, and submodels—each potentially varying by region—ensuring the correct part match is essential. OEM (Original Equipment Manufacturer) numbers serve as key references for identifying exact parts and are widely used when creating aftermarket alternatives. When selling aftermarket parts, customers often reference OEM numbers or, in some cases, part numbers from competing aftermarket brands when requesting quotes. Therefore, maintaining a comprehensive catalog that includes all available cross-references from original manufacturers and competitors is vital for any aftermarket seller. This cross-referencing system, often compiled into a "Master Cross," enhances catalog accuracy, improves customer service, and simplifies the quoting process. In this study, I will demonstrate how to create an effective Master Cross for an aftermarket automotive manufacturer, providing a valuable resource to share with clients. Given that major industry players manage catalogs with 15,000 to 35,000 unique parts, building an organized and detailed Master Cross is essential for maintaining a competitive edge.

### Data Sources

The Auto_Cross_Data_Anonymous CSV file contains over 150,000 rows of data. While this dataset is based on real-life examples, all information has been anonymized to ensure confidentiality.

### Tools

- Microsoft SQL Server

### Data Context & Preparation

The provided dataset consists of three primary columns: `CrossName`, `CrossingNumber`, and `CompanyPart#`. The `CrossName` column categorizes the type of cross-branding associated with each part, while the `CrossingNumber` is to be a unique alphanumeric identifier,  representing a hashed or encrypted value for each cross-reference. The CompanyPart# originally contained specific part numbers associated with a company but has now been anonymized to generic identifiers in the format of PartID_1, PartID_2, and so on. This anonymization helps protect sensitive product information while maintaining the uniqueness of each part for analysis or processing purposes.



