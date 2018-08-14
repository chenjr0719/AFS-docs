## Workspace

![workspace](../_static/images/portal/workspace/default.png)

### Analytics


#### Online Code IDE

In **AFS**, we provide a powerful **Online Code IDE** based on [Jupyter](http://jupyter.org/) to develop your analytic on the cloud with complete **EnSaaS** support.


#### auth_code

The `$auth_code` is an environment variable from **Online Code IDE**, and the purpose of `$auth_code` is authenticating with **AFS** to use **AFS** functions in your analytics.

To check `$auth_code` of **Online Code IDE**, you can use the following snippet:
```
import os
auth_code = os.getenv('auth_code')
print(auth_code)
```
The output:
![](../_static/images/portal/workspace/auth_code.png)


#### Manifest

In **Online Code IDE**, you can define some customize configurations like **memory**, **disk**, or **requirements** for your analytic by declaring a **manifest** at the first cell.

Here is an basic example:
```python
manifest = {
    'memory': 2048,
    'disk_quota': '2048MB',
    'requirements': [
        'pandas'
    ],
    'type': 'API'
}
```
![](../_static/images/portal/workspace/manifest.png)

In this example, **memory** and **disk_quota** are also assigned to 2048MB. If set **memory** or **disk_quota** as **int** type, the default unit is **MB**. Or, use **str** type and you can specify the unit in **M**, **MB**, **G**, or **GB**.

> **Note:** The default value of **disk_quota** is **2048MB** to avoid insufficient disk space when installing modules. If you set **disk_quota** less than 2048MB, the value will be overridden to 2048MB.

The **requirements** are the most important part in analytic develop. As native python develop, when you need some external modules, you can use **requirements.txt** to record all dependencies of your analytic.(more information can be found at [pip docs](https://pip.pypa.io/en/stable/user_guide/#id1)) . Provide a list of **requirements** can obtain the same effect when developing analytic by **AFS**.

The **type** is used to declare this analytic is an **APP** or an **API**. In default, all analytic will be assigned as **APP** type. But if you want your analytic serve as an **API**(and also write in any web framework), you need set **type** to **API** to host your analytic on **WISE-PaaS**.


#### Create analytic with Online Code IDE

1. Click the `CREATE` buttom.
![analytic_create_button](../_static/images/portal/workspace/analytics/create_new_analytic.png)

2. Name your new analytic.
![analytic_naming_dialog](../_static/images/portal/workspace/analytics/create_new_analytic_naming.png)
![online_code_ide_loading](../_static/images/portal/workspace/analytics/online_code_ide_loading.png)
![online_code_ide](../_static/images/portal/workspace/analytics/online_code_ide.png)


#### Install module with [Vendor](vendor.html) in private cloud

In python develop, we can use `pip install $MODULE` to install all required module. But in a private cloud, there is no any external internet resource can be used, including [PyPI](https://pypi.org/).

This restrict force all required modules should provide an offline distribution file in the private cloud when developing in **Online Code IDE** and save the source code to an analytic app.

This section will provide an example to use **Vendor** of AFS to install a module in  **Online Code IDE**. Assume the module is already uploaded to **AFS**, if not, please reference documentation of [Vendor](vendor.html) to upload module.

1. Right-click on the module and copy the url.
    ![copy_module_url](../_static/images/portal/vendor/check_module.png)

2. In **Online Code IDE**, use the following command and paste copied module url to install modules from the vendor:
    ```bash
    ! pip install $MODULE_URL?auth_code=$auth_code
    ```
    ![install_module_from_vendor](../_static/images/portal/workspace/analytics/install_module_from_vendor.png)


### Solutions


#### Create new solution