## 使用回歸演算法預測數值

流程為將歷史資料拆分為訓練表和驗證表

- 訓練表 - 訓練演算法模組

- 驗證表 - 驗證模組準確性


1. 建立訓練表(ML_TRAIN)

將歷史表select出70%的資料當作訓練表

```
INSERT INTO ML_TRAIN(ID, xxx, xxx,xxx,xxx)
SELECT ID, xxx, xxx,xxx FROM xxx WHERE ROWNUM <= (SELECT COUNT(*)/100*70 FROM xxx);

PL/SQL procedure successfully completed.
---------------------------
Table CUSTOMERS360 created.
---------------------------
```

2. GLM Model Settings table

```
%sql
CREATE TABLE glmr_sh_sample_settings (
  setting_name  VARCHAR2(30),
  setting_value VARCHAR2(4000));
```
此張table內的資料為設定演算法參數的值

3. 設定Automated Data Preparation and GLM Model Feature Selection

```
%script
BEGIN
/* Populate settings table  \*/
  INSERT INTO glmr_sh_sample_settings (setting_name, setting_value) VALUES
    (dbms_data_mining.algo_name, dbms_data_mining.algo_generalized_linear_model);
  -- output row diagnostic statistics into a table named GLMC_SH_SAMPLE_DIAG  
  INSERT INTO  glmr_sh_sample_settings (setting_name, setting_value) VALUES
    (dbms_data_mining.glms_diagnostics_table_name, 'GLMR_SH_SAMPLE_DIAG');  
  INSERT INTO  glmr_sh_sample_settings (setting_name, setting_value) VALUES
    (dbms_data_mining.prep_auto, dbms_data_mining.prep_auto_on);  
  -- turn on feature selection
  INSERT INTO  glmr_sh_sample_settings (setting_name, setting_value) VALUES  
    (dbms_data_mining.glms_ftr_selection,
    dbms_data_mining.glms_ftr_selection_enable);
  -- turn on feature generation
  INSERT INTO  glmr_sh_sample_settings (setting_name, setting_value) VALUES
    (dbms_data_mining.glms_ftr_generation,
    dbms_data_mining.glms_ftr_generation_enable);
END;

commit;

```

4. 建立模型

```
declare
  v_xlst dbms_data_mining_transform.TRANSFORM_LIST;

BEGIN
  DBMS_DATA_MINING.CREATE_MODEL(
    model_name          => 'GLMR_SH_Regr_sample',
    mining_function     => dbms_data_mining.regression,
    data_table_name     => 'ML_TRAIN',
    case_id_column_name => 'ID',  →須為Primary key
    target_column_name  => 'XXX', →需要預測的欄位
    settings_table_name => 'glmr_sh_sample_settings',
    xform_list          => v_xlst);
END;
```

5. show 出GLM Model 設定

```
SELECT setting_name, setting_value
  FROM user_mining_model_settings
 WHERE model_name = 'GLMR_SH_REGR_SAMPLE'
ORDER BY setting_name;
```

6. 建立驗證表(ML_TEST)

```
CREATE OR REPLACE VIEW ML_TEST AS SELECT * FROM xxx MINUS SELECT * FROM ML_TRAIN';
```

7. Select 出預測數值

```
SELECT ID,
       PREDICTION(GLMR_SH_Regr_sample USING *) pr,
       PREDICTION_BOUNDS(GLMR_SH_Regr_sample USING *).lower pl,
       PREDICTION_BOUNDS(GLMR_SH_Regr_sample USING *).upper pu
  FROM ML_TEST
 WHERE ID < 100010
 ORDER BY CUST_ID;

```

DONE!
