📕 **Fork from [TechCPS](https://github.com/Techcps/GSP)**

Machine learning process:
Visit the documentations : https://cloud.google.com/bigquery/docs/linear-regression-tutorial?hl=fr

```sql
--1). Creation de model
CREATE OR REPLACE MODEL
  `bq-webmarketing.test_dataset.ml_test` OPTIONS(model_type = 'linear_reg',
    input_label_cols = ['fare_amount'] ) AS
SELECT
  *
FROM
  `data-to-insights.taxi.tlc_yellow_trips_2018_sample`;


--2). Evaluation de Model
SELECT
  *
FROM
  ML.EVALUATE(MODEL `bq-webmarketing.test_dataset.ml_test`);


--3). Prediction du Model
SELECT
  *
FROM
  ML.PREDICT(Model `bq-webmarketing.test_dataset.ml_test`,
    (SELECT
      *
    FROM
      `data-to-insights.taxi.tlc_yellow_trips_2018_sample`
    WHERE
      passenger_count>=1));

--4). Expliquer les résultats des prédictions
SELECT
  *
FROM
  ML.EXPLAIN_PREDICT(Model `bq-webmarketing.test_dataset.ml_test`,
    (SELECT
      *
    FROM
      `data-to-insights.taxi.tlc_yellow_trips_2018_sample`
    WHERE
      passenger_count>=1));

--5). Expliquer globalement le modèle (expliquer le model lors de la creation du model)
CREATE OR REPLACE MODEL
  `bq-webmarketing.test_dataset.ml_test` OPTIONS(model_type = 'linear_reg',
    input_label_cols = ['fare_amount'],
    enable_global_explain = TRUE) AS
SELECT
  *
FROM
  `data-to-insights.taxi.tlc_yellow_trips_2018_sample`;

-- explication du model
select * from ML.GLOBAL_EXPLAIN(MODEL`bq-webmarketing.test_dataset.ml_test`);
```





