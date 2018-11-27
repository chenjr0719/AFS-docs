
## 建立 iii_Decision_Tree_Model
1. 成功訂閱AFS並成功開啟  
2. 創建一個新的 Online Code IDE ,iii_Decision_Tree_Model  
* **Note**: iii 為 company 名稱 , DecisiontreeModel 為 project 名稱並且開頭必須大寫

3. 在Online Code IDE編輯iii_Decision_Tree_Model ,   iii_Decision_Tree_Model 的 ipynb檔可參考以下連結：
[ipynb連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/src/iii_DecisionTree.ipynb)
iii_Decision_Tree_Model 的內容參考如下：
```python =
manifest = {
    'memory': 1024,
    'disk_quota': 2048,
    'buildpack': 'python_buildpack',
    'requirements': [
        'numpy',
        'pandas',
        'scikit-learn',
        'influxdb',
        'requests',
        'scipy',
        'urllib3',
        'afs'
    ],
    'type': 'API'
}
```
```python =
from afs import config_handler
from pandas import DataFrame
import json

cfg = config_handler()
cfg.set_param('criterion', type='string', required=True, default="gini")
cfg.set_param('random_state', type='string', required=True, default="2")
cfg.set_param('max_depth', type='string', required=True, default="3")
cfg.set_param('K_fold', type='integer', required=True, default=10)

cfg.set_param('model_name', type='string', required=True, default="model.pkl")
cfg.set_features(True)
cfg.set_column('data')
cfg.summary()

```
```python =
from sklearn import tree
from sklearn.model_selection import train_test_split
from sklearn import metrics
from sklearn.externals import joblib
from afs import models
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import LabelBinarizer
from sklearn.preprocessing import OneHotEncoder
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import GridSearchCV

import pandas as pd
import numpy as np
import json
import requests
```
```python =
def grid(data , target , parameters_dt , cv):
    clf = tree.DecisionTreeClassifier()
    grid = GridSearchCV(estimator = clf, param_grid = parameters_dt, cv = cv, 
                        scoring = 'accuracy')
    grid.fit(data,target)
    best_accuracy = grid.best_score_
    best_params = grid.best_params_
    del clf,grid
    import gc
    gc.collect()
    return best_accuracy,best_params
```
```python =
def training_model(data , target ,best_params , best_accuracy ,model_name):
    clf = tree.DecisionTreeClassifier(**best_params)
    clf = clf.fit(data, target)
    #save model
    joblib.dump(clf , model_name)
    client = models()
    client.upload_model(model_name, accuracy=best_accuracy, loss=0.0, tags=dict(machine='dt'))
    del clf,client
    import gc
    gc.collect()
    return model_name

```
```python =
# POST /

# Set flow architecture, REQUEST is the request including body and headers from client

# from afs import config_handler
# cfg = config_handler()
cfg.set_kernel_gateway(REQUEST)
# Get the parameter from node-red setting


criterion = str(cfg.get_param('criterion'))
random_state = str(cfg.get_param('random_state'))
max_depth = str(cfg.get_param('max_depth'))
cv = cfg.get_param('K_fold')


model_name = str(cfg.get_param('model_name'))
select_feature = cfg.get_features_selected()
data_column_name = cfg.get_features_numerical()
target2 = cfg.get_features_target()

labels_column_name = [x for x in select_feature if x not in data_column_name]
labels_column_name = [x for x in labels_column_name if x not in target2]

if(labels_column_name==[]):
    labels_column_name=["No"]

a1=["time"]
labels_column_name = [x for x in labels_column_name if x not in a1]

if "All" in labels_column_name:
    labels_column_name.remove("All")

if(data_column_name==[]):
    data_column_name=["No"]

criterion = criterion.split(",")
random_state = random_state.split(",")
max_depth = max_depth.split(",")

random_state =list(map(int, random_state))
max_depth = list(map(int, max_depth))

parameters_dt = {"criterion" : criterion , "random_state" : random_state , "max_depth" : max_depth}



# Get the data from request, and transform to DataFrame Type
df = cfg.get_data()
df = pd.DataFrame(df)

target = np.array(df.loc[:,[target2]])

if (data_column_name[0]=="All"):
    all_df_column = [df.columns[i] for i in range(len(df.columns))]
    if (labels_column_name[0]!="No"):
        for i in range(len(labels_column_name)):
            all_df_column.remove(labels_column_name[i])
        all_df_column.remove(target2)
    if (labels_column_name[0]=="No"):
        all_df_column.remove(target2)
    data = np.array(df.loc[:,all_df_column])

elif (data_column_name[0]=="No"):
    data = np.array([]).reshape(df.shape[0],0)
    if (labels_column_name[0]!="No"):
        for i in labels_column_name:   
            if ((False in map((lambda x: type(x) == str), df[i]))==False):
                label2 = LabelBinarizer().fit_transform(df[i])
                data = np.hstack((data,label2))
            if ((False in map((lambda x: type(x) == int), df[i]))==False):
                target9 = OneHotEncoder( sparse=False ).fit_transform(df[i].values.reshape(-1,1))
                data = np.hstack((data,target9))

else:    
    data = np.array(df.loc[:,data_column_name])
    if (labels_column_name[0]!="No"):
        for i in labels_column_name:   
            if ((False in map((lambda x: type(x) == str), df[i]))==False):
                label2 = LabelBinarizer().fit_transform(df[i])
                data = np.hstack((data,label2))
            if ((False in map((lambda x: type(x) == int), df[i]))==False):
                target9 = OneHotEncoder( sparse=False ).fit_transform(df[i].values.reshape(-1,1))
                data = np.hstack((data,target9))

best_accuracy,best_params = grid(data , target , parameters_dt , cv)
result = training_model(data , target ,best_params , best_accuracy ,model_name)
result = str(result)

df2 = pd.DataFrame([result], columns=['model_name'])
# df_dict = df2.to_dict()  


# # Send the result to next node, and result is  DataFrame Type

ret = cfg.next_node(df2, debug=False) 

# The printing is the API response.

import gc
del criterion,random_state,max_depth,cv,model_name,select_feature,data_column_name
del target2,labels_column_name,a1,parameters_dt,df,
del best_accuracy,best_params,result,target,data,df2
gc.collect()

print(json.dumps(ret))

```
4. 點選左上角的 **File** -> **Download as** -> **Notebook(.ipynb)** 下載成 ipynb 的檔案格式

5. 確認檔名為 iii_Decision_Tree_Model.ipynb


## 在研華的 GitLab 創建新的 Repository

1. 研華 GitLab 連結如下：
   [GitLab連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog)
   
2. 點選連結進入研華GitLab
3. 右側的帳號登入請點選 **Standard** , 並輸入帳號與密碼完成登入
4. 點選右側的 **New Project** 建立新的 Repository
   如下圖：
   
   ![](https://i.imgur.com/WKdD6H5.png)
   
5. 輸入創建新的 Repository所需資訊
   如下圖：
   
   ![](https://i.imgur.com/3MlNqXp.png)
   
   第1步：輸入 Repository 名稱,需與ipynb的檔案同名,請    輸入 iii_Decision_Tree_Model
   
   第2步：請選擇 **Private**
   
   第3步：確認好前兩步資訊沒有填錯,請按 **Create              project** 建立Repository
   
6. 上一步驟執行完畢後會出現下圖：
   下圖僅供參考,實際狀況可能略微不同
   
   ![](https://i.imgur.com/96ufuVe.png)
   
7. 下載 Git , 下方為官網連結：
   [Git官網](https://git-scm.com/)
   請依照相對應的軟硬體設備選擇正確的檔案下載
   
8. Git 下載完成後,請開啟Git Bash
   會出現類似下圖的程式
   ![](https://i.imgur.com/wYmnMBc.png)
   
9. 請在 Git bash 執行第6步驟所出現的資訊,並依照第6步驟    所出現的資訊依序執行

10. 完成第9步驟後 , 會在桌面看到一個資料夾名為         iii_Decision_Tree_Model(因為我在執行第9步驟時的當前資料夾為 Desktop , 所以產生的資料夾位於桌面)

11. 點擊進入資料夾,請在此資料夾建立兩個資料夾兩個檔案如     下：
    (1)img 資料夾
    (2)src 資料夾
    (3)README.md檔案
    (4)version.json檔案
    如下圖：
   ![](https://i.imgur.com/MuJTfWu.png)
   
12. 介紹第11步所建立之資料夾與檔案:
    (1)img : 放公司的圖片檔案,並命名為company_icon
        ![](https://i.imgur.com/nmJwqj4.png)
        圖片連結可參考如下：
        [img 圖片](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/img/company_icon.png)
    
    (2) README.md檔案:描述關於此節點相關資訊
        內容請參考以下連結：
        [README.md 連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/README.md)
    
    (3)version.json檔案:填寫node相關資訊
        內容請參考以下連結：
        [version.json 連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/version.json)
    
    (4)src資料夾裡請建立一個資料夾以及三個檔案和剛剛建立的iii_Decision_Tree_Model.ipynb檔案
    如下圖：
    ![](https://i.imgur.com/qEzmj3K.png)

      a.Procfile 內容可參考以下連結：
      [Procfile 連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/src/Procfile)
      
      b.requirements.txt : 紀錄所需套件
      請參考以下連結：
      [requirements 連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/src/requirements.txt)
      
      c.runtime:執行的python環境
      請參考以下連結：
      [runtime連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/src/runtime.txt)
       
       d.iii_Decision_Tree_Model.ipynb:之前所建立的檔案
       
       e.vendor : 所需套件的安裝檔
       可參考如下圖：
       ![](https://i.imgur.com/rDGBeMO.png)
       詳細所需要的套件請參考以下連結：
       [vendor連結](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/tree/master/src/vendor)

13. 當完成第10~12步驟時請回到git bash進行push的動作
    請依序執行以下步驟：
    (1)git status 查看哪些檔案需要push
    
    (2)git add 加上要push的檔案名
       以此範例為例,請輸入以下指令：
       git add img/
       git add src/
       git add README.md
       git add version.json
    
    (3)git commit -m "update" (update可替換)
    
    (4)git push origin master 將檔案push上去
    
    (5)git tag -a 1_0_0 -m 'version 1_0_0' 替這次的push打tag
      
    (6)git push origin 1_0_0 將tag push上去
    
14. 當以上所有的步驟完成後,可以在[GitLab](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog)看到剛剛所建立 iii_Decision_Tree_Model 的 Repository

