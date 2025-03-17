# SQL Approach to Developing a Master Cross for Auto Part Catalogs

### Table of Contents

-[Project Overview](#project-overview)

-[Data Sources](#data-sources)

-[Tools](#tools)

-[Data Context & Preparation](#data-context--preparation)

-[Transformation](#transformation)

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

#### Data Restructuring Plan for Enhanced RFQ Processing

The objective is to restructure the data by creating dedicated columns for each unique **CrossName**, ensuring a more organized and accessible format. This will enhance the efficiency of querying and retrieving information, particularly for the sales team handling customer **RFQs (Requests for Quotations)**.

#### Key Steps in Data Restructuring:

1. **Dedicated Columns for CrossNames:**

   - Each unique **CrossName** will be represented as a separate column.
   - For each row, all possible combinations of values derived from the dataset will be generated.

2. **Handling Missing CrossNames:**

   - If a **CrossName** is not available for a particular entry, it will be represented as **null** to maintain data integrity.

3. **Capturing Multiple Combinations:**

   - In cases where multiple **CrossNames** exist for the same brand, each unique combination will be represented in separate rows.
   - This ensures every possible permutation is captured, providing a comprehensive view of the data.

4. **Addressing Multiple Crosses for Single Part Numbers:**

   - There are instances where each **CrossName** may have multiple crosses for a single part number. This occurs when manufacturers make minor changes to a part due to inefficiencies, repeated returns, or chronic problems.
   - When such changes happen, a part number supersedes to another number but still corresponds to the same car and submodel.
   - The restructuring will account for these scenarios, ensuring that superseded part numbers and their valid crosses are accurately reflected in the data.

#### Benefits of Restructuring:

- **Improved Data Accessibility:** Streamlined structure for faster and more accurate data retrieval.
- **Enhanced RFQ Response:** Simplified process for identifying relevant cross-references, allowing the sales team to respond to customer inquiries with greater accuracy and speed.
- **Comprehensive Data Representation:** Ensures no valid cross-reference is overlooked, even when multiple crosses or supersessions exist.

This restructuring approach aims to create a robust data framework that supports efficient and effective RFQ processing.


Below is the sample output wanted using the given sample data

| PartID      | OEMCond   | BERU    |   Bougicord | Bremi   | Mecra     |   ValuCraft | ACDelco   |   B_B_Long |   B_B | BoschCond   |   Brecav |   Carol | Champion   |   Delphi |   Doduco |   Facet | IISI     | Ilkerler   |   Intermotor |   Janmor |   Magneti |   Moroso | NGK       |   ProSpark |   Sentech |   SMP |   Tesla |   Unuvar |
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


Below is the stored procedure that I consistently utilize whenever a new part cross needs to be added or when existing cross-references require modification.

This procedure is designed to ensure that every new cross-reference is integrated accurately into the system while maintaining data integrity and consistency. Additionally, it allows for the seamless updating of existing cross data when changes arise, such as supersessions, manufacturer updates, or corrections due to inefficiencies or chronic issues.

By using this structured approach, we can guarantee that the database remains reliable and up-to-date, ensuring the sales team has immediate access to the most current cross-reference information. This enhances our ability to respond to customer inquiries with speed and precision while reducing the risk of data discrepancies.

```sql
USE [MASTER]
GO
/****** Object:  StoredProcedure [dbo].[usp_CreateCrossMaster]    Script Date: 3/17/2025 4:08:27 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER PROCEDURE [dbo].[usp_CreateCrossMaster]
AS
BEGIN
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.CrossMaster_PartID', 'U') IS NOT NULL
    BEGIN
        DROP TABLE dbo.CrossMaster_PartID;
    END;

    -- Define CTEs for each 'Type'
    WITH BERU_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'BERU'
    ),
    Bougicord_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Bougicord'
    ),
    Bremi_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Bremi'
    ),
    Mecra_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Mecra'
    ),
    ValuCraft_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'ValuCraft'
    ),
    ACDelco_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'ACDelco'
    ),
    B_B_Long_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'B&B_Long'
    ),
    B_B_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'B&B'
    ),
    BoschCond_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'BoschCond'
    ),
    Brecav_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Brecav'
    ),
    Carol_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Carol'
    ),
    Champion_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Champion'
    ),
    Delphi_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Delphi'
    ),
    Doduco_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Doduco'
    ),
    Facet_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Facet'
    ),
    IISI_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'IISI'
    ),
    Ilkerler_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Ilkerler'
    ),
    Intermotor_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Intermotor'
    ),
    Janmor_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Janmor'
    ),
    Magneti_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Magneti'
    ),
    Moroso_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Moroso'
    ),
    NGK_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'NGK'
    ),
    ProSpark_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'ProSpark'
    ),
    Sentech_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Sentech'
    ),
    SMP_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'SMP'
    ),
    Tesla_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Tesla'
    ),
    Unuvar_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'Unuvar'
    ),
    OEMCond_CTE AS (
        SELECT PartID, Part_Number
        FROM [dbo].[CrossMaster_Raw]
        WHERE Type = 'OEMCond'
    )

    -- Insert the final result into CrossMaster_PartID table
    SELECT
        COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID,
                 B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID,
                 Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID,
                 Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID, Sentech_CTE.PartID,
                 SMP_CTE.PartID, Tesla_CTE.PartID, Unuvar_CTE.PartID, OEMCond_CTE.PartID) AS PartID,
        BERU_CTE.Part_Number AS BERU,
        Bougicord_CTE.Part_Number AS Bougicord,
        Bremi_CTE.Part_Number AS Bremi,
        Mecra_CTE.Part_Number AS Mecra,
        ValuCraft_CTE.Part_Number AS ValuCraft,
        ACDelco_CTE.Part_Number AS ACDelco,
        B_B_Long_CTE.Part_Number AS B_B_Long,
        B_B_CTE.Part_Number AS B_B,
        BoschCond_CTE.Part_Number AS BoschCond,
        Brecav_CTE.Part_Number AS Brecav,
        Carol_CTE.Part_Number AS Carol,
        Champion_CTE.Part_Number AS Champion,
        Delphi_CTE.Part_Number AS Delphi,
        Doduco_CTE.Part_Number AS Doduco,
        Facet_CTE.Part_Number AS Facet,
        IISI_CTE.Part_Number AS IISI,
        Ilkerler_CTE.Part_Number AS Ilkerler,
        Intermotor_CTE.Part_Number AS Intermotor,
        Janmor_CTE.Part_Number AS Janmor,
        Magneti_CTE.Part_Number AS Magneti,
        Moroso_CTE.Part_Number AS Moroso,
        NGK_CTE.Part_Number AS NGK,
        ProSpark_CTE.Part_Number AS ProSpark,
        Sentech_CTE.Part_Number AS Sentech,
        SMP_CTE.Part_Number AS SMP,
        Tesla_CTE.Part_Number AS Tesla,
        Unuvar_CTE.Part_Number AS Unuvar,
        OEMCond_CTE.Part_Number AS OEMCond
    INTO dbo.CrossMaster_PartID
    FROM
        BERU_CTE
    FULL OUTER JOIN Bougicord_CTE ON BERU_CTE.PartID = Bougicord_CTE.PartID
    FULL OUTER JOIN Bremi_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID) = Bremi_CTE.PartID
    FULL OUTER JOIN Mecra_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID) = Mecra_CTE.PartID
    FULL OUTER JOIN ValuCraft_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID) = ValuCraft_CTE.PartID
    FULL OUTER JOIN ACDelco_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID) = ACDelco_CTE.PartID
    FULL OUTER JOIN B_B_Long_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID) = B_B_Long_CTE.PartID
    FULL OUTER JOIN B_B_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID) = B_B_CTE.PartID
    FULL OUTER JOIN BoschCond_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID) = BoschCond_CTE.PartID
    FULL OUTER JOIN Brecav_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID) = Brecav_CTE.PartID
    FULL OUTER JOIN Carol_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID) = Carol_CTE.PartID
    FULL OUTER JOIN Champion_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID) = Champion_CTE.PartID
    FULL OUTER JOIN Delphi_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID) = Delphi_CTE.PartID
    FULL OUTER JOIN Doduco_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID) = Doduco_CTE.PartID
    FULL OUTER JOIN Facet_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID) = Facet_CTE.PartID
    FULL OUTER JOIN IISI_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID) = IISI_CTE.PartID
    FULL OUTER JOIN Ilkerler_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID) = Ilkerler_CTE.PartID
    FULL OUTER JOIN Intermotor_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID) = Intermotor_CTE.PartID
    FULL OUTER JOIN Janmor_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID) = Janmor_CTE.PartID
    FULL OUTER JOIN Magneti_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID) = Magneti_CTE.PartID
    FULL OUTER JOIN Moroso_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID) = Moroso_CTE.PartID
    FULL OUTER JOIN NGK_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID) = NGK_CTE.PartID
    FULL OUTER JOIN ProSpark_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID) = ProSpark_CTE.PartID
    FULL OUTER JOIN Sentech_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID) = Sentech_CTE.PartID
    FULL OUTER JOIN SMP_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID, Sentech_CTE.PartID) = SMP_CTE.PartID
    FULL OUTER JOIN Tesla_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID, Sentech_CTE.PartID, SMP_CTE.PartID) = Tesla_CTE.PartID
    FULL OUTER JOIN Unuvar_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID, Sentech_CTE.PartID, SMP_CTE.PartID, Tesla_CTE.PartID) = Unuvar_CTE.PartID
    FULL OUTER JOIN OEMCond_CTE ON COALESCE(BERU_CTE.PartID, Bougicord_CTE.PartID, Bremi_CTE.PartID, Mecra_CTE.PartID, ValuCraft_CTE.PartID, ACDelco_CTE.PartID, B_B_Long_CTE.PartID, B_B_CTE.PartID, BoschCond_CTE.PartID, Brecav_CTE.PartID, Carol_CTE.PartID, Champion_CTE.PartID, Delphi_CTE.PartID, Doduco_CTE.PartID, Facet_CTE.PartID, IISI_CTE.PartID, Ilkerler_CTE.PartID, Intermotor_CTE.PartID, Janmor_CTE.PartID, Magneti_CTE.PartID, Moroso_CTE.PartID, NGK_CTE.PartID, ProSpark_CTE.PartID, Sentech_CTE.PartID, SMP_CTE.PartID, Tesla_CTE.PartID, Unuvar_CTE.PartID) = OEMCond_CTE.PartID;

END;
```







