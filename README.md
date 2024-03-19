# DocAICortex

## Prerequisite:

### Create Database DOC_AI_DB and Schema DOC_AI_SCHEMA

<img width="329" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/ee65757c-91c8-4b73-be09-e0a08621eb1e">

### Create Warehouse

<img width="1379" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/1813ecb2-eb48-44b1-8cc1-ea627540e30f">


### Create DocumentAI Roles
https://docs.snowflake.com/LIMITEDACCESS/document-ai/roles

## Large Language Model (LLM) Functions (Snowflake Cortex) required-privileges:
https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions#required-privileges

##

## Working with Document AI:

<img width="256" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/4e698108-c195-4ed5-9ca3-fe3781b04b85">


### Add Project
<img width="1664" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/beeafe60-633c-495a-ba18-a54b9097d3b9">
<img width="768" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/0c016cf4-55a7-48d2-b7a9-15e23af87a9b">


### Upload Uber10KPDF
<img width="1675" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/ddc7e30a-2f79-477b-ab3d-ba72a0b60d51">


## Start asking questions
<img width="1686" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/76614ae2-6c77-4efe-a207-51d7ed2d0267">

## Start Training and Publish
<img width="257" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/8ec53811-f0e4-4ddd-b36a-9aac196adc76">




## Create Internal Stage

<img width="1022" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/8da0638a-013c-4807-97bf-bee5b83c2f0a">

## Upload File

<img width="1034" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/b2302f72-72eb-49d4-9a47-3ac439aa1d70">

## RUN ANALYSIS FOR FY2021 10K
```SQL
create or replace TABLE DOC_AI_DB.DOC_AI_SCHEMA.UBER10K_ANALYSIS (
	"Fiscal_Year" VARCHAR,
	"ExtractFrom2022Uber10K" VARCHAR,
	"WhatIsThisAbout" VARCHAR,
	"Year" VARCHAR,
	SWOT VARCHAR,
	"Current Ranking" VARCHAR
);


INSERT into UBER10K_ANALYSIS 
SELECT 
a.c2 as "Fiscal_Year"
,a.c1 as "ExtractFrom2022Uber10K"
, SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'what is the following about '|| a.c1) as "What is this about"
, SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'What is this fiscal year for this report? '|| a.c2) as "Year" 
, SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'Can you give me a SWOT analysis based on the following '|| a.c1) as "SWOT" 
, SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'Where do Uber rank in ride sharing? '|| a.c1) as "Current Ranking"
FROM 
(
select as_varchar(etresultsl2.value) as c2
,as_varchar(etresultsl1.value) as c1 
from (
SELECT UBERTEST2020!PREDICT(
  GET_PRESIGNED_URL(@DOC_TEST_STAGE2, 'Uber2022p125.pdf'),6 ) as results
FROM DIRECTORY(@DOC_TEST_STAGE2)
) et
,LATERAL FLATTEN(et.results:"Competition"[0]) etresultsl1
,LATERAL FLATTEN(et.results:"fiscalyear"[0] ) etresultsl2
where etresultsl2.key='value'
AND etresultsl1.key='value'
) a
;
```
## Note: You'll need to input the current model version for the Predict Function:
<img width="440" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/632fb763-3fa5-49a5-92fd-69b4997126c4">


<img width="1302" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/c8e876cc-690a-4cec-bff6-08c4bd73f217">

## SWOT ANALYSIS
```SQL
SELECT swot  FROM UBER10K_ANALYSIS;
```

## Output

<img width="1331" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/41314aa8-4ac1-4203-9564-c6f76a79db4f">


## Ranking
```SQL
SELECT "Current Ranking"  FROM UBER10K_ANALYSIS;
```

## Output
<img width="1348" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/43660981-b2f1-41e2-be49-513c7c4f434b">


## Snowflake Notebook:
<img width="1686" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/b05c53b9-0671-4afa-a4ca-1e0250e87571">

<img width="1682" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/ef6d6045-9fa9-4e68-8b59-a81184d71468">

<img width="1077" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/59abd92b-7eb6-4044-9d98-8f2ba28455e8">

<img width="1208" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/c605b1da-7685-487d-9b7d-4b3c481eea91">

<img width="1217" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/38e8ef2e-a4ad-4f96-96cc-922330b64f92">

<img width="1082" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/328d6012-4498-4ec8-85a7-88f4348b7de7">











