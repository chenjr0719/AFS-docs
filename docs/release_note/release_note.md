# Release Note

## 1.2.26 (2018-10-26)
Features:
* Remove the **Hidden Space**.
* Add the icon of WISE-PaaS into the AFS UI.
* Add the default icon into APP status list.

Bugfixed:
在v1.2.25版本上测试，创建AFS实例失败，返回错误“Your account has been locked because of too many failed attempts to login”
* Fix when it's failure to creating the AFS service intance  
【偶发】场景2的training_decisiontree_task创建后，一直无执行结果
無法設定具有逗號、減號、/n表示法的crontab類型的task


## 1.2.25 (2018-10-19)

### Bugfixes
透過Pipeline部署AFS service broker時即便k8s參數為空(disable)，部屬成功後仍舊會出現k8s的plan可供訂閱

Pipeline部署AFS Portal 和 SB APP，使用帳號AFSAdmin，以及實現加解密並更換AFSAdmin密碼

情境三最后一步，查看predct_result.txt无任何预测值

通过OTA部署Docker_install的包失败
Task 使用command的方式設定失敗
AFS 所用CF Credentials 偶发被锁定(was:AFS Internal Server Error)
無法Edit既存的Analytic，出現route not exist的錯誤訊息
Validate solution failed但是實際上用task trigger此solution卻可順利執行並產出model
無法設定具有逗號、減號、/n表示法的crontab類型的task
training_decisiontree_task在執行約12小時後出現錯誤，暫停重啟無法恢復

