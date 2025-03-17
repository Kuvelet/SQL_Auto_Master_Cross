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

| CrossName        | CrossingNumber                                                | CompanyPart# |
|------------------|--------------------------------------------------------------|--------------|
| CrossbrandType_1 | 1a399853c67162cbce50d1b6ed1878ca0271e6af813e4aa832cd1c4986c96acb | PartID_1     |
| CrossbrandType_1 | e75eff849dd8ebb894a773634d8f3b9a2cedcce27df09eeed75a868c2265b256 | PartID_1     |
| CrossbrandType_11| 399084d8f947b8b2358fac366dc666ff38b6687ebd647c8458ca4c1df3f7a941 | PartID_1     |
| CrossbrandType_1 | 1a379cd377f7e946f9773281950f9018883ed51f156c493a5f8ee93fd21af4da | PartID_1     |
| CrossbrandType_1 | 4a63622c3655ec284482d12ee2aa747540620814a9d01d6847c81450a5782645 | PartID_1     |
| CrossbrandType_1 | ca65b19218527c70680da0b8825c34494b162c709391c96f5aacc9416edecd23 | PartID_1     |




