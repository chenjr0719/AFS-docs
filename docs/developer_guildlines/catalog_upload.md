# Upload the Analytics to Catalog

AFS provides the users/developers can upload their analytics to the catalog. For other users, if they find the analytics in the catalog are suitable for solving their problems, they can subcribe the analytics from catalog.    

In the section, you could learn how to uploading the analytics to catalog.    

1. The developers could upload the code of analytics to **Gitlab**.
2. Before uploading, the naming rules you should know.
3. Finally, the structure of gitlab repository is introduced.

## Naming rule
### Gitlab Repository

* Naming rule of Repository Name

	- The name shoule be start from **company name**. The first character of each word is capitalized, and connect by the bottom line betwwen words.
	- Example: company1_Split_Img, company2_Svm, company2_Wave_Transform.

* Local Gitlab

	- The repository name can't be duplicated (includes the node name) in local gitlab of company.
		
		- Each developer can push the code to repository directly.
		- Example: /developer1/advantech_Split_Img, /developer2/advantech_Svm, /developer1/advantech_Svm.

	- An account has the permission to clone all of repository is required.

### Node

Naming rule of Node (HTML, JS)

* \{company\}_\{Repository_Name\}
	- \\{company\\}: The host name of company email address (in lowercase).
	- \{Repository_Name\}: The name must be same with the repository in the Gitlab. The first character of each word is capitalized, and connect by the bottom line betwwen words.	
* Example: company_Split_Img, company_Svm.

## Gitlab Repository Structure
### Native Python Version

* \{gitlab\}/\{Node_Project\}
	* /src: Save the source code.
		- /vendor
		- Procfile
		- main.py: The main program.
		- manifest.yml: The app name must be same with node_project name.
		- node_config.html: The HTML of node in Node-RED. The node name must be same with node_project name.
		- node_config.js: The js of node in Node-RED. The node name must be same with node_project name.
		- requirements.txt
		- runtime.txt
	* README.md: The spec and description of node, includes the items as follows:
		- Node_Name: The analytic name.
		- Version: The version of node (x.x.x).
		- Description: The description of the node, must be in English, and less than 300 words.
		- Publish time: The date of publish (yyyy-mm-dd).
		- Input data format: The format of input data (e.g., Request headers, Request, body) that get from the previous node.
		- Output data format: The format of output data (e.g., Request headers, Request, body) that send to the next node.
		- Response Content: Response body, status code.
		- Others: The other items(e.g., api interface...etc.)    
	* version.json: The record and version of node.
		- node_version: node version(must be same with "README.md").
		- node_type: native_python.

* A description (less than 50 words) which shows in the catalog index page is required.

### Jupyter Version

* \{gitlab\}/\{Node_Project\}
	* /src: Save the source code.
		- /vendor
		- Procfile
		- \{company\}_\{Node_Project\}.ipynb: name the jupyter notebook by "\{company\}_\{Node_Project\}" (withoyt "-dev").
		- requirements.txt
		- runtime.txt
	* README.md: The spec and description of node, includes the items as follows:
		- Node_Name: The analytic name.
		- Version: The version of node (x.x.x).
		- Description: The description of the node, must be in English, and less than 300 words.
		- Publish time: The date of publish (yyyy-mm-dd).
		- Input data format: The format of input data (e.g., Request headers, Request, body) that get from the previous node.
		- Output data format: The format of output data (e.g., Request headers, Request, body) that send to the next node.
		- Response Content: Response body, status code.
		- Others: The other items(e.g., api interface...etc.)     
	* version.json: The record and version of node.
		- node_version: node version(must be same with "README.md").
		- node_type: native_python

* Jupyter的/src，在AFS portal中，進入每個analytic的頁面，在網址後加上「/download」，即可將notebook載下來解壓縮取得以下相關檔案。
* The folder "/src" of Jupyter is in AFS portal, 

* A description (less than 50 words) which shows in the catalog index page is required.






