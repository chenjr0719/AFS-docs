# Inference Engine Install Automatically in Edge Device

Previously, a introduction of **Inference Engine**, it's a Python runtime program on Docker. We can install it manually step by step. However, for the industial application, there are many edge devices (e.g., perhapes 10, 100, or 1000 devices) work online at the same time. In the section, we introduce how to install the Inference Engine automatically in many edge devices.

## Pre-condition
* The OS of edge devices must be the **Windows 10 Pro** or higher version.
* The edge devices must be installed the **RMM Agent (v-1.0.16)**, and registed in RMM Server.
    

## Update the Python Package via the whl file in a On-Premises environment


![image](../_static/images/inference_engine/01_package.png)


![image](../_static/images/inference_engine/02_step1.png)


![image](../_static/images/inference_engine/03.png)


![image](../_static/images/inference_engine/04_package_path.png)


![image](../_static/images/inference_engine/05_login_RMM.png)


![image](../_static/images/inference_engine/06_ota_package.png)


![image](../_static/images/inference_engine/07_upload.png)


![image](../_static/images/inference_engine/08_choose_file.png)


![image](../_static/images/inference_engine/09_upload_progress.png)


![image](../_static/images/inference_engine/10_ota_upgrade.png)


![image](../_static/images/inference_engine/11_upgrade_package.png)


![image](../_static/images/inference_engine/12_upgrade_progress.png)


![image](../_static/images/inference_engine/13_install01.png)


![image](../_static/images/inference_engine/14_install02.png)


