# Example for Upload the Analytics to Catalog

The section will instroduce how to upload a Analytic Node to Catalog. A sample code of decision tree model is employed. Users could upload the sample by following the steps.

## Create iii_Decision_Tree_Model

1. Subscribe an AFS service instance, and enter the AFS portal. 
2. Create a new Online Code IDE, named "iii_Decision_Tree_Model".     
    **Note**: "iii" is the company name. "Decision_Tree_Model" is project name, and the first character of each word is capitalized.

3. Edit "iii_Decision_Tree_Model" in the Online Code IDE, please refer the [sample code](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/src/iii_DecisionTree.ipynb).

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

4. Click **File** in the left corner -> **Download as** -> **Notebook(.ipynb)**. Download the file in .ipynb format.

5. Check the file name, "iii_Decision_Tree_Model.ipynb".


## Create a Repository in Gitlab

1. The link of [GitLab](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog).
2. Click the link and connect to Gitlab.
3. Click **Standard** login, and enter the account and password.
4. Click the **New Project** in the right of page to create a new repository as the screenshot.
   
    ![](https://i.imgur.com/WKdD6H5.png)
   
5. Enter the information of the repository.
   
    ![](https://i.imgur.com/3MlNqXp.png)
   
   Step 1: Enter the repository name, and it must be same with ipynb file name. In the example, please enter "iii_Decision_Tree_Model".
   Step 2: Please select **Private**.
   Step 3: Check the entered information, then click **Create project** to create the repository.
   
6. After step 5 is complete, there are some information that show as the screenshot.
   
    ![](https://i.imgur.com/96ufuVe.png)
   
7. Download Git from the [offical website](https://git-scm.com/).
   
8. After installing the Git, please startup the **Git Bash**. The app window is like the following screenshot.
    
    ![](https://i.imgur.com/wYmnMBc.png)
   
9. Please execute the commands that showed in step 6.

10. After the step 9 is complete, there is a folder in the Desktop which is named "iii_Decision_Tree_Model". (Because the currently folder is **Desktop** when executing the commands in step 6, the iii_Decision_Tree_Model is generated in Desktop.)

11. Double click the folder, then create two folders and two files as follows. 
    * img folder      
    * src folder
    * README.md
    * version.json

    ![](https://i.imgur.com/MuJTfWu.png)

    The purpose of the folder and files:
    * img folder: Storage the icon of company, and named by "company_icon".
        ![](https://i.imgur.com/nmJwqj4.png)
    
    * src folder: Storage the "iii_Decision_Tree_Model.ipynb".
        **Note:** The file name should be same with the repository name.

    * README.md: The purpose and description of the node, please refer the [example](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/README.md).
    
    * version.json: The information of the node, please refer the [example](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog/iii_DecisionTree/blob/master/version.json).
    
12. After the steps 10 and 11 are complete, you could push the file in git bash. Please follow the steps:

    (1) Check the files which will be pushed.    
        ```
        git status    
        ```
    (2) Add the file which will be pushed.    
        ```
        git add img/
        git add src/
        git add README.md
        git add version.json
        ```
    (3) Add the description for commiting.    
        ```
        git commit -m "The description of the update."    
        ```
    (4) Push the files.    
        ```
        git push origin master
        ```
    (5) Tag for the pushing.    
        ```
        git tag -a 1_0_0 -m 'version 1_0_0'
        ```
    (6) Push the tag.    
        ```
        git push origin 1_0_0
        ```
13. When completing all steps, the repository "iii_Decision_Tree_Model" is shown in [GitLab](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS_AFS_Catalog).

