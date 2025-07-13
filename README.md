# Indian-Political-Analysis-Related-to-Tax-Scheme-in-India
Designed and implemented an advanced-level MySQL-based analytical database project to study the impact of various Indian tax schemes (GST, Income Tax, etc.) on public sentiment, political party performance, and revenue outcomes. The project involved data modeling, relationship mapping, and advanced SQL queries for multi-dimensional analysis.

Skills Demonstrated:
Relational Database Design (Normalization, ERD)

Complex SQL Querying (JOINs, Aggregations, Subqueries, Window Functions)

Data Analysis & Reporting

Problem-Solving Using Structured Data

Domain Understanding: Public Policy, Taxation, Indian Politics

Use This Project to Showcase:
SQL proficiency in interviews or portfolio

Real-world data analysis on social impact and public policy

Ability to design and execute end-to-end database projects

Interest in analytical roles in governance, finance, policy, or research domains.



-- 🔑 Primary Key (PK) and Foreign Key (FK) Summary:


| Table                | Primary Key | Foreign Keys                               |
| -------------------- | ----------- | ------------------------------------------ |
| PoliticalParties     | PartyID     | —                                          |
| TaxSchemes           | SchemeID    | IntroducedBy → PoliticalParties(PartyID)   |
| StateWiseOpinions    | OpinionID   | SchemeID → TaxSchemes(SchemeID)            |
| VotingTrends         | TrendID     | WinningPartyID → PoliticalParties(PartyID) |
| SchemeImplementation | RegionID    | SchemeID → TaxSchemes(SchemeID)            |
| PublicFeedback       | FeedbackID  | SchemeID → TaxSchemes(SchemeID)            |
| TaxRevenue           | RevenueID   | SchemeID → TaxSchemes(SchemeID)            |



--  ER Diagram (Text Representation)
+---------------------+        +------------------+        +----------------------+
|  PoliticalParties   |<-------|    TaxSchemes    |<-------|  StateWiseOpinions   |
|---------------------|        |------------------|        |----------------------|
| PartyID (PK)        |        | SchemeID (PK)    |        | OpinionID (PK)       |
| PartyName           |        | SchemeName       |        | State                |
| Leader              |        | IntroducedBy (FK)|--------| SchemeID (FK)        |
| FoundedYear         |        | SchemeType       |        | SupportPercent       |
| Ideology            |        | LaunchYear       |        | OppositionPercent    |
| NationalParty       |        | TaxRate          |        | Year                 |
+---------------------+        +------------------+        +----------------------+

         ▲                                 ▲                              ▲
         |                                 |                              |
         |                                 |                              |
         |                                 |                              |
         |         +------------------+    |        +--------------------+
         |         | VotingTrends     |    |        | SchemeImplementation
         |         |------------------|    |        |--------------------+
         +-------->| TrendID (PK)     |    +------->| RegionID (PK)       |
                   | State            |             | State               |
                   | Year             |             | SchemeID (FK)       |
                   | WinningPartyID(FK)|            | ImplementationStatus|
                   | MarginPercent    |             | ComplianceRate      |
                   | VoterTurnout     |             +--------------------+
                   +------------------+                        ▲
                                                               |
                                     +------------------+      |
                                     |  PublicFeedback   |<-----+
                                     |------------------|
                                     | FeedbackID (PK)  |
                                     | State            |
                                     | SchemeID (FK)    |
                                     | Sentiment        |
                                     | Comment          |
                                     | Year             |
                                     +------------------+

                                     +------------------+
                                     |   TaxRevenue     |
                                     |------------------|
                                     | RevenueID (PK)   |
                                     | Year             |
                                     | SchemeID (FK)    |
                                     | State            |
                                     | RevenueCollected |
                                     +------------------+




---- CREATE DATABASE IF NOT EXISTS IndianPoliticalTaxAnalysis;
USE IndianPoliticalTaxAnalysis;

--  Create Tables and Relationships
-- 1 PoliticalParties
CREATE TABLE PoliticalParties (
    PartyID INT PRIMARY KEY,
    PartyName VARCHAR(100),
    Leader VARCHAR(100),
    FoundedYear INT,
    Ideology VARCHAR(100),
    NationalParty BOOLEAN
);

INSERT INTO PoliticalParties (PartyID, PartyName, Leader, FoundedYear, Ideology, NationalParty) VALUES
(1, 'Bharatiya Janata Party', 'Narendra Modi', 1980, 'Right-wing', true),
(2, 'Indian National Congress', 'Mallikarjun Kharge', 1885, 'Center-left', true),
(3, 'Aam Aadmi Party', 'Arvind Kejriwal', 2012, 'Centrist', false),
(4, 'Party_4', 'Leader_4', 1953, 'Regional', false),
(5, 'Party_5', 'Leader_5', 1982, 'Regional', false),
(6, 'Party_6', 'Leader_6', 1980, 'Regional', false),
(7, 'Party_7', 'Leader_7', 2000, 'Regional', false),
(8, 'Party_8', 'Leader_8', 1977, 'Regional', false),
(9, 'Party_9', 'Leader_9', 2009, 'Regional', false),
(10, 'Party_10', 'Leader_10', 1966, 'Regional', false),
(11, 'Party_11', 'Leader_11', 1982, 'Regional', false),
(12, 'Party_12', 'Leader_12', 1951, 'Regional', false),
(13, 'Party_13', 'Leader_13', 2015, 'Regional', false),
(14, 'Party_14', 'Leader_14', 1978, 'Regional', false),
(15, 'Party_15', 'Leader_15', 2012, 'Regional', false),
(16, 'Party_16', 'Leader_16', 1962, 'Regional', false),
(17, 'Party_17', 'Leader_17', 1977, 'Regional', false),
(18, 'Party_18', 'Leader_18', 1952, 'Regional', false),
(19, 'Party_19', 'Leader_19', 1980, 'Regional', false),
(20, 'Party_20', 'Leader_20', 1986, 'Regional', false),
(21, 'Party_21', 'Leader_21', 1972, 'Regional', false),
(22, 'Party_22', 'Leader_22', 2007, 'Regional', false),
(23, 'Party_23', 'Leader_23', 1960, 'Regional', false),
(24, 'Party_24', 'Leader_24', 1986, 'Regional', false),
(25, 'Party_25', 'Leader_25', 1981, 'Regional', false),
(26, 'Party_26', 'Leader_26', 1970, 'Regional', false),
(27, 'Party_27', 'Leader_27', 1995, 'Regional', false),
(28, 'Party_28', 'Leader_28', 1996, 'Regional', false),
(29, 'Party_29', 'Leader_29', 1975, 'Regional', false),
(30, 'Party_30', 'Leader_30', 1971, 'Regional', false);


-- 2 TaxSchemes
CREATE TABLE TaxSchemes (
    SchemeID INT PRIMARY KEY,
    SchemeName VARCHAR(100),
    IntroducedBy INT,
    SchemeType VARCHAR(50),
    LaunchYear INT,
    TaxRate FLOAT,
    FOREIGN KEY (IntroducedBy) REFERENCES PoliticalParties(PartyID)
);

INSERT INTO TaxSchemes (SchemeID, SchemeName, IntroducedBy, SchemeType, LaunchYear, TaxRate) VALUES
(1, 'Scheme_1', 21, 'Indirect', 2003, 21.1),
(2, 'Scheme_2', 23, 'Direct', 2002, 11.6),
(3, 'Scheme_3', 5, 'Indirect', 2006, 10.0),
(4, 'Scheme_4', 1, 'Direct', 2000, 5.7),
(5, 'Scheme_5', 16, 'Indirect', 2014, 23.5),
(6, 'Scheme_6', 3, 'Direct', 2010, 19.2),
(7, 'Scheme_7', 7, 'Indirect', 2013, 22.6),
(8, 'Scheme_8', 25, 'Direct', 2023, 12.5),
(9, 'Scheme_9', 17, 'Indirect', 2001, 21.0),
(10, 'Scheme_10', 16, 'Direct', 2023, 18.6),
(11, 'Scheme_11', 17, 'Indirect', 2008, 1.6),
(12, 'Scheme_12', 1, 'Direct', 2020, 9.2),
(13, 'Scheme_13', 3, 'Indirect', 2022, 11.8),
(14, 'Scheme_14', 16, 'Direct', 2020, 23.1),
(15, 'Scheme_15', 20, 'Indirect', 2003, 7.3),
(16, 'Scheme_16', 15, 'Direct', 2014, 3.6),
(17, 'Scheme_17', 28, 'Indirect', 2017, 20.2),
(18, 'Scheme_18', 28, 'Direct', 2001, 15.7),
(19, 'Scheme_19', 3, 'Indirect', 2004, 26.7),
(20, 'Scheme_20', 29, 'Direct', 2001, 20.8),
(21, 'Scheme_21', 30, 'Indirect', 2007, 9.8),
(22, 'Scheme_22', 13, 'Direct', 2004, 21.7),
(23, 'Scheme_23', 8, 'Indirect', 2023, 7.0),
(24, 'Scheme_24', 7, 'Direct', 2018, 13.5),
(25, 'Scheme_25', 21, 'Indirect', 2020, 24.2),
(26, 'Scheme_26', 4, 'Direct', 2004, 8.3),
(27, 'Scheme_27', 1, 'Indirect', 2011, 18.9),
(28, 'Scheme_28', 7, 'Direct', 2012, 16.5),
(29, 'Scheme_29', 24, 'Indirect', 2015, 26.7),
(30, 'Scheme_30', 24, 'Direct', 2009, 25.4);

-- 3 StateWiseOpinions
CREATE TABLE StateWiseOpinions (
    OpinionID INT PRIMARY KEY,
    State VARCHAR(50),
    SchemeID INT,
    SupportPercent FLOAT,
    OppositionPercent FLOAT,
    Year INT,
    FOREIGN KEY (SchemeID) REFERENCES TaxSchemes(SchemeID)
);

INSERT INTO StateWiseOpinions (OpinionID, State, SchemeID, SupportPercent, OppositionPercent, Year) VALUES
(1, 'Gujarat', 3, 81.3, 12.4, 2021),
(2, 'Tamil Nadu', 21, 59.7, 25.4, 2023),
(3, 'Kerala', 18, 44.4, 21.4, 2020),
(4, 'Karnataka', 6, 56.8, 32.3, 2023),
(5, 'Delhi', 28, 59.3, 33.2, 2022),
(6, 'Uttar Pradesh', 27, 88.5, 64.7, 2017),
(7, 'West Bengal', 1, 51.4, 57.8, 2016),
(8, 'Rajasthan', 4, 50.5, 39.7, 2019),
(9, 'Punjab', 30, 88.5, 12.5, 2023),
(10, 'Bihar', 19, 34.5, 53.0, 2020),
(11, 'Madhya Pradesh', 1, 68.4, 63.6, 2022),
(12, 'Haryana', 10, 84.6, 47.2, 2021),
(13, 'Chhattisgarh', 5, 80.3, 17.5, 2016),
(14, 'Odisha', 8, 82.5, 50.1, 2023),
(15, 'Assam', 22, 85.6, 40.1, 2022),
(16, 'Goa', 1, 59.3, 43.6, 2023),
(17, 'Jharkhand', 3, 41.4, 38.0, 2021),
(18, 'Telangana', 1, 72.1, 49.5, 2020),
(19, 'Andhra Pradesh', 5, 34.9, 31.6, 2016),
(20, 'Tripura', 22, 44.6, 23.9, 2015),
(21, 'Meghalaya', 28, 78.7, 43.6, 2015),
(22, 'Manipur', 4, 82.3, 35.7, 2020),
(23, 'Nagaland', 25, 55.9, 40.8, 2018),
(24, 'Sikkim', 26, 63.6, 33.4, 2020),
(25, 'Mizoram', 22, 32.2, 28.8, 2021),
(26, 'Arunachal Pradesh', 21, 42.6, 59.2, 2017),
(27, 'Ladakh', 1, 48.7, 10.6, 2019),
(28, 'Jammu & Kashmir', 17, 76.5, 27.0, 2018),
(29, 'Himachal Pradesh', 8, 69.9, 24.6, 2015),
(30, 'Maharashtra', 12, 79.2, 24.5, 2017);


-- 4 VotingTrends
CREATE TABLE VotingTrends (
    TrendID INT PRIMARY KEY,
    State VARCHAR(50),
    Year INT,
    WinningPartyID INT,
    MarginPercent FLOAT,
    VoterTurnout FLOAT,
    FOREIGN KEY (WinningPartyID) REFERENCES PoliticalParties(PartyID)
);

INSERT INTO VotingTrends (TrendID, State, Year, WinningPartyID, MarginPercent, VoterTurnout) VALUES
(1, 'Gujarat', 2020, 30, 57.2, 53.9),
(2, 'Tamil Nadu', 2015, 3, 35.9, 55.4),
(3, 'Kerala', 2018, 25, 37.8, 69.8),
(4, 'Karnataka', 2019, 8, 47.5, 73.3),
(5, 'Delhi', 2017, 13, 42.2, 65.5),
(6, 'Uttar Pradesh', 2014, 30, 48.5, 76.9),
(7, 'West Bengal', 2015, 5, 38.9, 61.0),
(8, 'Rajasthan', 2023, 14, 50.9, 58.8),
(9, 'Punjab', 2015, 24, 60.0, 67.0),
(10, 'Bihar', 2022, 4, 55.3, 69.1),
(11, 'Madhya Pradesh', 2016, 24, 57.7, 70.4),
(12, 'Haryana', 2014, 13, 49.3, 70.0),
(13, 'Chhattisgarh', 2019, 19, 55.4, 57.4),
(14, 'Odisha', 2020, 11, 39.4, 57.7),
(15, 'Assam', 2017, 22, 59.1, 79.7),
(16, 'Goa', 2021, 6, 52.4, 54.3),
(17, 'Jharkhand', 2016, 18, 44.2, 63.1),
(18, 'Telangana', 2021, 27, 30.9, 76.2),
(19, 'Andhra Pradesh', 2021, 26, 52.8, 71.3),
(20, 'Tripura', 2022, 25, 39.2, 62.0),
(21, 'Meghalaya', 2014, 20, 45.4, 71.7),
(22, 'Manipur', 2017, 26, 37.0, 51.9),
(23, 'Nagaland', 2024, 14, 48.4, 63.3),
(24, 'Sikkim', 2021, 21, 35.0, 67.0),
(25, 'Mizoram', 2019, 3, 36.5, 70.2),
(26, 'Arunachal Pradesh', 2017, 29, 69.7, 61.3),
(27, 'Ladakh', 2019, 8, 50.2, 76.6),
(28, 'Jammu & Kashmir', 2017, 5, 66.8, 71.5),
(29, 'Himachal Pradesh', 2022, 12, 69.5, 75.5),
(30, 'Maharashtra', 2021, 4, 41.1, 68.1);


-- 5 SchemeImplementation
CREATE TABLE SchemeImplementation (
    RegionID INT PRIMARY KEY,
    State VARCHAR(50),
    SchemeID INT,
    ImplementationStatus VARCHAR(50),
    ComplianceRate FLOAT,
    FOREIGN KEY (SchemeID) REFERENCES TaxSchemes(SchemeID)
);

INSERT INTO SchemeImplementation (RegionID, State, SchemeID, ImplementationStatus, ComplianceRate) VALUES
(1, 'Gujarat', 28, 'Medium', 52.7),
(2, 'Tamil Nadu', 21, 'Medium', 81.8),
(3, 'Kerala', 12, 'Medium', 84.3),
(4, 'Karnataka', 21, 'High', 40.4),
(5, 'Delhi', 16, 'High', 62.1),
(6, 'Uttar Pradesh', 8, 'High', 88.5),
(7, 'West Bengal', 2, 'Low', 88.6),
(8, 'Rajasthan', 18, 'Medium', 46.3),
(9, 'Punjab', 25, 'High', 75.7),
(10, 'Bihar', 2, 'Low', 94.5),
(11, 'Madhya Pradesh', 16, 'High', 51.0),
(12, 'Haryana', 18, 'Low', 64.0),
(13, 'Chhattisgarh', 17, 'High', 71.2),
(14, 'Odisha', 2, 'High', 49.9),
(15, 'Assam', 7, 'High', 91.7),
(16, 'Goa', 25, 'Low', 88.0),
(17, 'Jharkhand', 1, 'Low', 91.8),
(18, 'Telangana', 19, 'Medium', 82.3),
(19, 'Andhra Pradesh', 6, 'High', 72.8),
(20, 'Tripura', 15, 'High', 45.6),
(21, 'Meghalaya', 30, 'Low', 57.1),
(22, 'Manipur', 21, 'High', 68.2),
(23, 'Nagaland', 29, 'High', 74.3),
(24, 'Sikkim', 27, 'Low', 62.6),
(25, 'Mizoram', 17, 'High', 63.1),
(26, 'Arunachal Pradesh', 9, 'Medium', 73.5),
(27, 'Ladakh', 26, 'High', 93.1),
(28, 'Jammu & Kashmir', 24, 'Medium', 57.7),
(29, 'Himachal Pradesh', 1, 'Low', 52.3),
(30, 'Maharashtra', 4, 'High', 53.0);


-- 6 PublicFeedback
CREATE TABLE PublicFeedback (
    FeedbackID INT PRIMARY KEY,
    State VARCHAR(50),
    SchemeID INT,
    Sentiment VARCHAR(10), -- Values: 'Positive', 'Neutral', 'Negative'
    Comment TEXT,
    Year INT,
    FOREIGN KEY (SchemeID) REFERENCES TaxSchemes(SchemeID)
);

INSERT INTO PublicFeedback (FeedbackID, State, SchemeID, Sentiment, Comment, Year) VALUES
(1, 'Gujarat', 27, 'Neutral', 'Comment about scheme 1', 2018),
(2, 'Tamil Nadu', 3, 'Neutral', 'Comment about scheme 2', 2018),
(3, 'Kerala', 29, 'Neutral', 'Comment about scheme 3', 2022),
(4, 'Karnataka', 21, 'Positive', 'Comment about scheme 4', 2015),
(5, 'Delhi', 30, 'Positive', 'Comment about scheme 5', 2022),
(6, 'Uttar Pradesh', 18, 'Positive', 'Comment about scheme 6', 2021),
(7, 'West Bengal', 14, 'Negative', 'Comment about scheme 7', 2015),
(8, 'Rajasthan', 1, 'Positive', 'Comment about scheme 8', 2016),
(9, 'Punjab', 10, 'Positive', 'Comment about scheme 9', 2023),
(10, 'Bihar', 30, 'Negative', 'Comment about scheme 10', 2018),
(11, 'Madhya Pradesh', 23, 'Negative', 'Comment about scheme 11', 2016),
(12, 'Haryana', 8, 'Negative', 'Comment about scheme 12', 2016),
(13, 'Chhattisgarh', 5, 'Negative', 'Comment about scheme 13', 2019),
(14, 'Odisha', 27, 'Positive', 'Comment about scheme 14', 2015),
(15, 'Assam', 5, 'Negative', 'Comment about scheme 15', 2019),
(16, 'Goa', 12, 'Negative', 'Comment about scheme 16', 2015),
(17, 'Jharkhand', 4, 'Negative', 'Comment about scheme 17', 2018),
(18, 'Telangana', 20, 'Positive', 'Comment about scheme 18', 2023),
(19, 'Andhra Pradesh', 15, 'Negative', 'Comment about scheme 19', 2018),
(20, 'Tripura', 8, 'Negative', 'Comment about scheme 20', 2018),
(21, 'Meghalaya', 1, 'Negative', 'Comment about scheme 21', 2016),
(22, 'Manipur', 15, 'Neutral', 'Comment about scheme 22', 2022),
(23, 'Nagaland', 25, 'Negative', 'Comment about scheme 23', 2023),
(24, 'Sikkim', 11, 'Neutral', 'Comment about scheme 24', 2023),
(25, 'Mizoram', 20, 'Negative', 'Comment about scheme 25', 2023),
(26, 'Arunachal Pradesh', 12, 'Neutral', 'Comment about scheme 26', 2022),
(27, 'Ladakh', 27, 'Neutral', 'Comment about scheme 27', 2018),
(28, 'Jammu & Kashmir', 17, 'Negative', 'Comment about scheme 28', 2017),
(29, 'Himachal Pradesh', 23, 'Neutral', 'Comment about scheme 29', 2020),
(30, 'Maharashtra', 26, 'Negative', 'Comment about scheme 30', 2018);


-- 7 TaxRevenue
CREATE TABLE TaxRevenue (
    RevenueID INT PRIMARY KEY,
    Year INT,
    SchemeID INT,
    State VARCHAR(50),
    RevenueCollected BIGINT,
    FOREIGN KEY (SchemeID) REFERENCES TaxSchemes(SchemeID)
);

INSERT INTO TaxRevenue (RevenueID, Year, SchemeID, State, RevenueCollected) VALUES
(1, 2015, 23, 'Gujarat', 155070109),
(2, 2019, 12, 'Tamil Nadu', 1478068592),
(3, 2023, 4, 'Kerala', 914462931),
(4, 2016, 3, 'Karnataka', 4317430226),
(5, 2021, 30, 'Delhi', 1235145207),
(6, 2019, 4, 'Uttar Pradesh', 4787658174),
(7, 2021, 4, 'West Bengal', 1882878924),
(8, 2019, 22, 'Rajasthan', 3074131963),
(9, 2017, 3, 'Punjab', 2804915294),
(10, 2022, 6, 'Bihar', 2315086935),
(11, 2017, 28, 'Madhya Pradesh', 1499209291),
(12, 2016, 16, 'Haryana', 3328926217),
(13, 2019, 29, 'Chhattisgarh', 2385371840),
(14, 2016, 5, 'Odisha', 3171045580),
(15, 2019, 2, 'Assam', 4079262188),
(16, 2020, 9, 'Goa', 1767838728),
(17, 2019, 30, 'Jharkhand', 3657404543),
(18, 2021, 10, 'Telangana', 4748942108),
(19, 2015, 18, 'Andhra Pradesh', 537983300),
(20, 2016, 12, 'Tripura', 3069554829),
(21, 2015, 17, 'Meghalaya', 4214889871),
(22, 2017, 1, 'Manipur', 2436830392),
(23, 2022, 23, 'Nagaland', 631291957),
(24, 2022, 19, 'Sikkim', 191549239),
(25, 2021, 12, 'Mizoram', 2632192612),
(26, 2020, 2, 'Arunachal Pradesh', 3256571501),
(27, 2022, 29, 'Ladakh', 872156125),
(28, 2015, 29, 'Jammu & Kashmir', 1289891493),
(29, 2019, 12, 'Himachal Pradesh', 1835026425),
(30, 2017, 9, 'Maharashtra', 278708797);

-- QUERIES

-- 1. List all political parties
SELECT * FROM PoliticalParties;

-- 2. Show all tax schemes introduced after 2015
SELECT * FROM TaxSchemes WHERE LaunchYear > 2015;

-- 3. Find public feedback from Maharashtra
SELECT * FROM PublicFeedback WHERE State = 'Maharashtra';

-- 4. Get all schemes with a tax rate over 18%
SELECT * FROM TaxSchemes WHERE TaxRate > 18;

-- 5. Show voting trends in Delhi
SELECT * FROM VotingTrends WHERE State = 'Delhi';

-- 6. Count how many schemes each party introduced
SELECT IntroducedBy, COUNT(*) AS TotalSchemes FROM TaxSchemes GROUP BY IntroducedBy;

-- 7. List states with high implementation compliance (>80%)
SELECT State FROM SchemeImplementation WHERE ComplianceRate > 80;

-- 8. Average support percent per scheme
SELECT SchemeID, AVG(SupportPercent) AS AvgSupport FROM StateWiseOpinions GROUP BY SchemeID;

-- 9. Total tax revenue collected by state in 2020
SELECT State, SUM(RevenueCollected) FROM TaxRevenue WHERE Year = 2020 GROUP BY State;

-- 10. Schemes with low public support (<40%)
SELECT * FROM StateWiseOpinions WHERE SupportPercent < 40;

-- 11. Feedback sentiment counts per state
SELECT State, Sentiment, COUNT(*) FROM PublicFeedback GROUP BY State, Sentiment;

-- 12. Join schemes with their introducing party
SELECT t.SchemeName, p.PartyName FROM TaxSchemes t JOIN PoliticalParties p ON t.IntroducedBy = p.PartyID;

-- 13. Voting trend with max margin per state
SELECT * FROM VotingTrends v1 WHERE MarginPercent = (
  SELECT MAX(v2.MarginPercent) FROM VotingTrends v2 WHERE v2.State = v1.State
);

-- 14. Top 5 states by tax revenue
SELECT State, SUM(RevenueCollected) FROM TaxRevenue GROUP BY State ORDER BY SUM(RevenueCollected) DESC LIMIT 5;

-- 15. Feedback positivity ratio per scheme
SELECT SchemeID, SUM(CASE WHEN Sentiment='Positive' THEN 1 ELSE 0 END)*1.0/COUNT(*) AS PosRatio FROM PublicFeedback GROUP BY SchemeID;

-- 16. Count schemes by type
SELECT SchemeType, COUNT(*) FROM TaxSchemes GROUP BY SchemeType;

-- 17. Years with most schemes launched
SELECT LaunchYear, COUNT(*) FROM TaxSchemes GROUP BY LaunchYear ORDER BY COUNT(*) DESC LIMIT 3;

-- 18. Average tax rate per party
SELECT p.PartyName, AVG(t.TaxRate) FROM TaxSchemes t JOIN PoliticalParties p ON t.IntroducedBy = p.PartyID GROUP BY p.PartyName;

-- 19. Schemes with support & opposition per state
SELECT s.SchemeName, o.State, o.SupportPercent, o.OppositionPercent FROM StateWiseOpinions o JOIN TaxSchemes s ON o.SchemeID = s.SchemeID;

-- 20. States with more negative than positive feedback
SELECT State FROM PublicFeedback GROUP BY State HAVING SUM(Sentiment='Negative') > SUM(Sentiment='Positive');

-- 21. Rolling average of tax revenue per scheme
SELECT SchemeID, Year, AVG(RevenueCollected) OVER (PARTITION BY SchemeID ORDER BY Year ROWS 2 PRECEDING) AS RollingAvg FROM TaxRevenue;

-- 22. Compare support before & after 2020
SELECT SchemeID,
  SUM(CASE WHEN Year < 2020 THEN SupportPercent ELSE 0 END) AS SupportBefore2020,
  SUM(CASE WHEN Year >= 2020 THEN SupportPercent ELSE 0 END) AS SupportAfter2020
FROM StateWiseOpinions GROUP BY SchemeID;

-- 23. Party with highest average winning margin
SELECT p.PartyName, AVG(v.MarginPercent) AS AvgMargin FROM VotingTrends v
JOIN PoliticalParties p ON v.WinningPartyID = p.PartyID
GROUP BY p.PartyName ORDER BY AvgMargin DESC LIMIT 1;

-- 24. Top compliant states for GST (SchemeID = 1)
SELECT State, ComplianceRate FROM SchemeImplementation WHERE SchemeID = 1 ORDER BY ComplianceRate DESC LIMIT 5;

-- 25. Schemes with high support but low compliance
SELECT o.SchemeID, o.State FROM StateWiseOpinions o
JOIN SchemeImplementation i ON o.SchemeID = i.SchemeID AND o.State = i.State
WHERE o.SupportPercent > 60 AND i.ComplianceRate < 50;

-- 26. Parties with schemes having >70% avg support
SELECT p.PartyName, COUNT(*) AS PopularSchemes FROM TaxSchemes t
JOIN StateWiseOpinions o ON t.SchemeID = o.SchemeID
JOIN PoliticalParties p ON t.IntroducedBy = p.PartyID
GROUP BY p.PartyName HAVING AVG(o.SupportPercent) > 70;

-- 27. Sentiment breakdown per scheme
SELECT SchemeID, Sentiment, COUNT(*) FROM PublicFeedback GROUP BY SchemeID, Sentiment;

-- 28. Revenue per scheme per year as % of total
SELECT Year, SchemeID, RevenueCollected,
  RevenueCollected * 100.0 / SUM(RevenueCollected) OVER(PARTITION BY Year) AS PercentShare
FROM TaxRevenue;

-- 29. States with more than 2 low implementations
SELECT State FROM SchemeImplementation WHERE ImplementationStatus = 'Low' GROUP BY State HAVING COUNT(*) > 2;

-- 30. Most common sentiment per state
SELECT State, Sentiment FROM (
  SELECT State, Sentiment, COUNT(*) AS Cnt,
  RANK() OVER (PARTITION BY State ORDER BY COUNT(*) DESC) AS Rnk
  FROM PublicFeedback GROUP BY State, Sentiment
) AS Ranked WHERE Rnk = 1;

-- 31. Correlation-like: voter turnout vs scheme support
SELECT v.State, v.VoterTurnout, AVG(o.SupportPercent) AS AvgSupport
FROM VotingTrends v JOIN StateWiseOpinions o ON v.State = o.State
GROUP BY v.State, v.VoterTurnout;

-- 32. Party-wise total revenue from schemes
SELECT p.PartyName, SUM(r.RevenueCollected) AS TotalRevenue
FROM PoliticalParties p
JOIN TaxSchemes t ON p.PartyID = t.IntroducedBy
JOIN TaxRevenue r ON r.SchemeID = t.SchemeID
GROUP BY p.PartyName;

-- 33. Pre-2000 schemes with >75% compliance
SELECT s.SchemeName, i.State, i.ComplianceRate
FROM TaxSchemes s JOIN SchemeImplementation i ON s.SchemeID = i.SchemeID
WHERE s.LaunchYear < 2000 AND i.ComplianceRate > 75;

-- 34. Avg revenue per scheme across all states
SELECT SchemeID, AVG(RevenueCollected) AS AvgRevenue
FROM TaxRevenue GROUP BY SchemeID;

-- 35. Party-wise number of schemes with 'Indirect' type
SELECT p.PartyName, COUNT(*) AS IndirectSchemes
FROM PoliticalParties p JOIN TaxSchemes t ON p.PartyID = t.IntroducedBy
WHERE t.SchemeType = 'Indirect' GROUP BY p.PartyName;

-- 36. Year-wise voter turnout trend by state
SELECT State, Year, VoterTurnout FROM VotingTrends ORDER BY State, Year;

-- 37. Average sentiment (positive %) per state
SELECT State,
  SUM(CASE WHEN Sentiment = 'Positive' THEN 1 ELSE 0 END) * 100.0 / COUNT(*) AS PositivePercent
FROM PublicFeedback GROUP BY State;

-- 38. Identify underperforming schemes (low support & revenue)
SELECT s.SchemeID, AVG(o.SupportPercent) AS AvgSupport, SUM(r.RevenueCollected) AS TotalRevenue
FROM StateWiseOpinions o
JOIN TaxRevenue r ON o.SchemeID = r.SchemeID
JOIN TaxSchemes s ON s.SchemeID = o.SchemeID
GROUP BY s.SchemeID
HAVING AvgSupport < 50 AND TotalRevenue < 1000000000;

-- 39. Max and Min tax rates per party
SELECT p.PartyName, MAX(t.TaxRate) AS MaxRate, MIN(t.TaxRate) AS MinRate
FROM PoliticalParties p JOIN TaxSchemes t ON p.PartyID = t.IntroducedBy
GROUP BY p.PartyName;

-- 40. Total feedback entries per year
SELECT Year, COUNT(*) AS Feedbacks FROM PublicFeedback GROUP BY Year ORDER BY Year;
