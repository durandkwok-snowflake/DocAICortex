# DocAICortex

## Prerequisite:
https://docs.snowflake.com/LIMITEDACCESS/document-ai/roles

## Select Document AI

<img width="256" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/4e698108-c195-4ed5-9ca3-fe3781b04b85">


## Add Project
<img width="1664" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/beeafe60-633c-495a-ba18-a54b9097d3b9">
<img width="768" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/0c016cf4-55a7-48d2-b7a9-15e23af87a9b">


## Upload Uber10KPDF
<img width="1675" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/ddc7e30a-2f79-477b-ab3d-ba72a0b60d51">
Start asking questions
<img width="1686" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/76614ae2-6c77-4efe-a207-51d7ed2d0267">

## Create Database DOC_AI_DB and Schema DOC_AI_SCHEMA

<img width="329" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/ee65757c-91c8-4b73-be09-e0a08621eb1e">


## Create Internal Stage

<img width="1022" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/8da0638a-013c-4807-97bf-bee5b83c2f0a">

## Upload File

<img width="1034" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/b2302f72-72eb-49d4-9a47-3ac439aa1d70">

## RUN ANALYSIS FOR FY2021 10K
```SQL
SELECT a.c1 as "Extract From 2022Uber10K"
,a.c2 as "Fiscal Year"
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

<img width="1302" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/c8e876cc-690a-4cec-bff6-08c4bd73f217">

## SWOT ANALYSIS

<img width="1605" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/5114dae8-e5be-4010-8311-8a73e0cd890a">

## Output

<img width="1608" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/af46f222-1578-4838-8994-af57dee55073">

## Ranking

<img width="1608" alt="image" src="https://github.com/durandkwok-snowflake/DocAICortex/assets/109616231/0d587cff-df81-4a96-a770-e860ddee0e3b">






