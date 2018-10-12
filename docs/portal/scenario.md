

## SCENARIO 1. AFS Workspaces - Analytics
### Pre-condition
1. Use SSO Tenant/Developer to login Management Portal, and subscribe the Influxdb service instance. (Please refer [Management Portal User Manual](https://portal-technical-stage.wise-paas.com/doc/document-portal.html#ManagementPortal-1).)
	a. Subscribe the service and name influxdb_dt
	![](../_static/images/portal/scenario/1-1.png)
	![](../_static/images/portal/scenario/1-2.png)

	b. Create the "Service Key", and get the connecting information of influxdb (e.g., database, host, passsword...).
	![](../_static/images/portal/scenario/1-3.png)
	![](../_static/images/portal/scenario/1-4.png)
	![](../_static/images/portal/scenario/1-5.png)

2. Subscribe the AFS service instance from Management Portal, and it's named by afs_training. When it shows **create succeeded**, the AFS service instance is created.

3. Click **afs_training** to enter the AFS.
	![](../_static/images/portal/scenario/1-6.png)

4. Create a new Analytic, Firehose, to upload the training data to database.
	a. Create a new Analytic, and it's named by rnn_firehose.
		![](../_static/images/portal/scenario/1-7.png)
		![](../_static/images/portal/scenario/1-8.png)
		![](../_static/images/portal/scenario/1-9.png)

	b. Copy the [sample code](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/rnn_firehose-dev.md)to the rnn_firehose, and the code must be divided by cell.
	Note: The **Cell** is like as follows:
	![](../_static/images/portal/scenario/1-10.png)
	
	c. Enter the connecting information to the rnn_firehose.
	![](../_static/images/portal/scenario/1-11.png)
	![](../_static/images/portal/scenario/1-12.png)

	d. Execute each ecll.

	e. Click the icon in the left side to save it, and click `SAVE` to upload the Analytic App.
	![](../_static/images/portal/scenario/1-13.png)

	f. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/1-14.png)

### Regular operation
1. Create a new Analytic, and it's named by rnn_model.
	![](../_static/images/portal/scenario/1-15.png)
	![](../_static/images/portal/scenario/1-16.png)
	![](../_static/images/portal/scenario/1-17.png)

2. Copy the [sample code](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/rnn_model-dev.md) to rnn-model which is created in last step.

3. Install the scikit-learn package, please copy the command form the [link](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/install_package1), and paste the code in a new cell as the follows. After executing the cell, delete it.
	![](../_static/images/portal/scenario/1-18.png)

	![](../_static/images/portal/scenario/1-19.png)

4. Enter the connecting information of influxdb.
	![](../_static/images/portal/scenario/1-20.png)

5. Execute all of cells.

6. Click the icon in the left side to save it, and click `SAVE` to upload the Analytic App.
	![](../_static/images/portal/scenario/1-21.png)

7. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/1-22.png)

8. Click **Models** in the manu, the model repository which is named by "rnn_model.h5" is created.
	![](../_static/images/portal/scenario/1-23.png)

9. Click "rnn_model.h5", we can see the accurancy and loss of the trained model.
	![](../_static/images/portal/scenario/1-24.png)


## SCENARIO 2. AFS Workspaces - Solutions
Create Online Flow IDE in the **AFS Workspaces - Solutions**, and train the **Desicion Tree** model. After training the model, use the OTA to deliver the model to the edge device.

### Pre-condition
1. Create the Decision Tree node in the Online Flow IDE
	a. Create a new Aanlytic, and it's named by training_dt_model. About the detail process, please refer the Pre-condition Step 4.b in the Scenario 1.

	b. Copy the [sample code](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/training_dt_model-dev.md) to training_dt_model, and the code must be divided by cell.

	c. Pick the second cell, and click `Run` to execute it.
	![](../_static/images/portal/scenario/2-1.png)

	d. Click the icon in the left side to save it, and click `SAVE` to upload the Analytic App.
	![](../_static/images/portal/scenario/2-2.png)

	e. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/2-3.png)

	Note: After the processing, the training_dt_model node is generated in the Online Flow IDE.

2. Subscribe the Influxdb_Query node in the Online Flow IDE.

	a. In the **Catalog**, we can subscribe the **Influxdb_Query** node in the Analytic category. Please refer the screeshots as follows:
	![](../_static/images/portal/scenario/2-4.png)
	![](../_static/images/portal/scenario/2-5.png)

	b. The **Influxdb_Query analytic** is listed in the **Analytic List** when it's subscribed successfully.

	c. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/2-6.png)

	Note: After the processing, the Influxdb_Query node is generated in the Online Flow IDE.

3. Subscribe the OTA node in the Online Flow IDE.
	a. In the **Catalog**, we can subscribe the **OTA** node in the Analytic category. Please refer the screeshots as follows:
	![](../_static/images/portal/scenario/2-7.png)
	![](../_static/images/portal/scenario/2-8.png)

	b. The **OTA analytic** is listed in the **Analytic List** when it's subscribed successfully.
	
	c. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/2-9.png)
	
	Note: After the processing, the OTA node is generated in the Online Flow IDE.

4. Create a new Analytic, Firehose, to upload the training data to database.
	
	a. Create a new Analytic, and it's named by data_to_influxdb, and copy the [sample code]( http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/data_to_influxdb-dev.md)
	![](../_static/images/portal/scenario/2-10.png) to it.

	b. Click `Run` to execute the cell, and it will print **success** when the cell is executed successfully.
	
	c. Click the icon in the left side to save it, and click `SAVE` to upload the Analytic App.
	![](../_static/images/portal/scenario/2-11.png)

	d. Wait a minute, the status of the Analytic will change to **Running**, and go to next step.
	![](../_static/images/portal/scenario/2-12.png)

5. Setup the **RMM** device, include (1)install the **RMM Agent** in the edge device; (2)register the device; and (3)create a storage for RMM. Please refer the [document](https://portal-technical-stage.wise-paas.com/doc/document-portal.html#RMM-4).

### Regular operation
1. Create a new Analytic, and it's named by training_decisiontree.
	![](../_static/images/portal/scenario/2-13.png)
	![](../_static/images/portal/scenario/2-14.png)
	![](../_static/images/portal/scenario/2-15.png)
	![](../_static/images/portal/scenario/2-16.png)

2. Pull the SSO Setting node from the list in the left side. Then, enter the Username and Password
in it.

3. Pull the Influxdb_Query node from the list in the left side. Then, select the influxdb_dt and the service key that we have created. Therefore, enter `select * from machine` to the Query Command.
	![](../_static/images/portal/scenario/2-17.png)

4. Pull the training_dt_model node from the list in the left side, and setup the parameters.
	![](../_static/images/portal/scenario/2-18.png)

	* criterion: Can't be empty. enter *gini* or *entropy*, separated by commas, there must be no spaces between parameters and commas.
	* random_state、max_depth: Enter the integer only. If want to optimize the parameters, we can fill in multiple sets of parameters in the random_state and max_depth fields as shown above. The parameters must be separated by commas. There must be no blank between the parameters and the comma.
	* K-fold: Enter the times for cross validation, and it must be an interger and bigger than one.
	* model_name: Name the trained model, must like *.pkl (e.g., model.pkl).
	![](../_static/images/portal/scenario/2-19.png)
	![](../_static/images/portal/scenario/2-20.png)

	* Select Features: Select which fields are to be put into the model for training (can be multiple select). In the field, please select the fields KW_EQUIPMENT, KW_FAN, KW_SUMMARY, PRESSURE_OUTPUT, STATUS_FAN, VOLTAGE_INPUT, and EVENT.
	* Select Numerical Features: Pick out the fields selected by select_features, which are the numeric fields (can be multiple selected but not fully selected, or not selected). Please select KW_EQUIPMENT, KW_FAN, KW_SUMMARY, PRESSURE_OUTPUT, STATUS_FAN, VOLTAGE_INPUT in the field.
	* Select Target Feature: Select the target of training. Please select EVENT in the field.
	* Map Column: The value of this field is the JSON Key value (can't be changed).
	
5. Pull the OTA node from the list in the left side, and setup the parameters. Select the edge device and storage that were setuped in *Pre-condition*.
	![](../_static/images/portal/scenario/2-21.png)

6. Connect the Influxdb_Firehose node to the training_dt_model node, then connect the training_dt_model node to the OTA node, click the `Deploy` button in the upper right corner, and click the `Save` button to save the Solution.
	![](../_static/images/portal/scenario/2-22.png)

7. Create a new Solution Task.
	a. Click Tasks from the left menu, create a new Task named training_decisiontree_task, and press the Next button.
	![](../_static/images/portal/scenario/2-23.png)
	![](../_static/images/portal/scenario/2-24.png)

	b. Select the Solution Type, Solution Instance select training_decisiontree, and click `Next`.
	![](../_static/images/portal/scenario/2-25.png)
	![](../_static/images/portal/scenario/2-26.png)

	c. About the Trigger Type, select Interval, and the Interval Type selects Minutes, the Interval fills in "1". Then, click `Create`.
	![](../_static/images/portal/scenario/2-27.png)
	![](../_static/images/portal/scenario/2-28.png)

8. Click training_decisiontree_task to enter **Task** to see the results.
	![](../_static/images/portal/scenario/2-29.png)

9. Wait a minute, the Task will start executing. After the execution is successful, the status will be displayed as succeded. If it does not appear after 1 minute, please press f5 to re-form the page.
	![](../_static/images/portal/scenario/2-30.png)

## SCENARIO 3. Inference Engine

## Pre-condition
* The OS of edge devices must be the **Windows 10 Pro** or higher version.
* The edge devices must be installed the **RMM Agent (v-1.0.16)**, and registed in RMM Server.
* Get the application of packaging (OTAPackager-1.0.5.exe).
* Download the files for package as follows: 
   * Docker installer. [[Download](https://store.docker.com/editions/community/docker-ce-desktop-windows)]

   * Three .bat files (include install_docker.bat, start_docker.bat, start_inference.bat). [[Download](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/tree/master/inference_engine/auto_install_docker)]

   * SSL credential (registry.cert). [[Download](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/tree/master/inference_engine/auto_install_docker)]
* Setup for login automatically after rebooting, please refer the [page](http://z88487561.pixnet.net/blog/post/47069245-%5Bwindows%5D-win-10-%E9%96%8B%E6%A9%9F-%E5%85%8D%E5%AF%86%E7%A2%BC-%E8%87%AA%E5%8B%95%E7%99%BB%E5%85%A5-%E5%B0%8F%E6%8A%80%E5%B7%A7).

## Start to Install Inference Engine

1. Use the OTApackager APP to package the required files.
	
	a. The required files.
	![image](../_static/images/inference_engine/01_package.png)

	b. Edit "install_docker.bat", the file path should be modified to matching the path in the edge device.
	![image](../_static/images/inference_engine/02_1.png)

	c. Enter the Package Tyep, Package Version, then select the path for saving the package file.
	![image](../_static/images/inference_engine/02_2_step1.png)

    d. Select **install_docker.bat** to be the "Deploy File".
    ![image](../_static/images/inference_engine/03.png)

    e. Select the folder for saving the package file.
    ![image](../_static/images/inference_engine/04_package_path.png)

2. Login to **RMM Portal**, and upload the package file.
	
	a. Login to **RMM Portal**.
	![image](../_static/images/inference_engine/05_login_RMM.png)

	b. Click OTA Package.
	![image](../_static/images/inference_engine/06_ota_package.png)

	c. Click "Upload".
	![image](../_static/images/inference_engine/07_upload.png)

	d. Select the package file for uploading.
	![image](../_static/images/inference_engine/08_choose_file.png)

	e. Wait a second, when the progress bar goes to 100%, the uploaded file is shown in the list.
	![image](../_static/images/inference_engine/09_upload_progress.png)

3. Send the uploaded file to the edge device for installing automatically.
	
	a. Click "OTA" and "Upgrade". Then, select the device to be installed.
	![image](../_static/images/inference_engine/10_ota_upgrade.png)

	b. Selcet the package which want to **Upgrade**.
	![image](../_static/images/inference_engine/11_upgrade_package.png)

	c. When the progressing bar goes to 100%, the edge device downloaded the package file completely, and start to install it.
	![image](../_static/images/inference_engine/12_upgrade_progress.png)

4. Before installing the package, the edge device restart once. The **Docker** in the edge device starts automatically, and the inference engine runs.

	a. The screenshot shows when the installation is running.
	![image](../_static/images/inference_engine/13_install01.png)

	b. In the screenshot, it shows the required images are downloading.
	![image](../_static/images/inference_engine/14_install02.png)

Finally, an edge device has been installed the inference engine automatically. Therefore, if there are many edge devices need to install the inference engine, we just need pick mutiple devices in **Step 3.**, and they will be installed completely.

5. Now, we can use the model which is trained in Scenario 2. to inference.
	
	a. Confirm that the model is trained successfully in Scenario 2., and devivered to edge device by OTA.

	b. Download the anaconda (with python 3.6), and install it in the edge device. [[Download](https://www.anaconda.com/download/)]
	![image](../_static/images/inference_engine/15.png)
	
	c. Start the **Jupyter Notebook** from application.
	![image](../_static/images/inference_engine/16.png)
	
	d. Download the [firehose](https://github.com/minikai/to_inference_engine_firehose_demo_0904) for testing the inference engine.
	d. Download the firehose program for testing the inference engine.
	![image](../_static/images/inference_engine/17.png)

	e. Click the Upload button at the top right to upload ex_config.ini, firehose.ipynb, and testing_data.csv to jupyter.
	![image](../_static/images/inference_engine/18.png)

	f. Click and modify ex_config.ini, and add "http://127.0.0.1:7500/predict" after "url=". Then, save the file.
	![image](../_static/images/inference_engine/19.png)

	g. Open the firehose.ipynb just uploaded on jupyter and click `Run` to execute.
	![image](../_static/images/inference_engine/20.png)

	h. Login to inference_engine, and see the prediction results.
		(1) Execute ```$ cmd``` to open the command window
		(2) Execute ```$ docker exec -it inference bash```
		(3) To check if the model is normally dispatched into the inference engine, we can execute ```$ ls /root/inference_engine/inference_engine/``` to see the model.pkl exists or not.
		(4) Execute ```$ cat /root/inference_engine/inference_engine/predict_result.txt``` to check if the predicted value continues to increase, if the representative is successful.
		![image](../_static/images/inference_engine/21.png)

## SCENARIO 4. AFS Vender
The development process can be done offline through Vendor.

1. The module can be installed offline through Vendor. About more deatil, please refer to [documents](https://afs-docs.readthedocs.io/en/latest/portal/workspace.html#install-module-with-vendor-in-private-cloud).

2. About how to manage the Vendor, including module upload, download, and delete the package, please refer to [documents](https://afs-docs.readthedocs.io/en/latest/portal/vendor.html).

## SCENARIO 5. AFS Task

### Create a task
The deatil steps are included in Scenario 2., please refer to Step 8-10 of Scenario 2.

### Create multiple tasks
1. Download “multiple_task_example.csv” to make the list of tasks.
	a. Click the CREATE button in the upper right corner of the Task page, click the button in the upper right corner of the pop-up window and click Create multiple task.

	b. Click the link to download csv example.

	c. Copy the [sample](http://advgitlab.eastasia.cloudapp.azure.com/EI-PaaS-SampleCode/analytics_framework_service/blob/master/notebook/02_support_vector_machine/multiple_task_example.csv) to a text editor and name the file multi_task.csv.

	Note: About the detail to edit the csv file, please refer to section 4.3 of the [spec document](https://docs.google.com/document/d/1FtTis-vThtgimmgG0DyWrc3X3V4_2L6OJrya5P5ipiw/edit).

2. Select the csv file (please select the csv file created by 1.c above) and click `CREATE` to create the tasks.

## SCENARIO 6. AFS Model
AFS Model shows the performance of the model training.
It has introduced in Step 8. to Step 9. of SCENARIO 1.

