# Side Effect of Removing the Hidden Space

Before AFS v1.2.26, there was a space that storaged the Jupyter and Node-RED two applications. The information of the space wasn't shown in the Management Portal, so called "hidden space". Therefore, users can't know the quota of resource had been used. In order to declare the resource quota, the hidden space has been removed. All of apps are moved to the user's space, and they are listed in the "Application List", currently.

* To avoid the duplicated apps' name, the naming rules are revised as follows:
    - Jupyter => code-ide-{instance[0:8]}, {instance_id}-jupyter.{domain} => code-ide-{instance[0:8]}.{domain}
    - Node-RED => flow-ide-{instance[0:8]}, {instance_id}-node-red.{domain} => flow-ide-{instance[0:8]}.{domain}
    - {name}-dev => analytic-{name}-dev-{instance[0:8]}, {instance_id}-{name}-dev.{domain} => analytic-{name}-dev-{instance[0:8]}.{domain}

* After removing the hidden space, the apps are listed in the Management Portal. The users can remove any apps by themselves, but removing some specific apps will damage the AFS service instance. The list of apps as follows are the dependency of AFS, please DON'T remove them:
	- code-ide-xxxxxxxx
	- flow-ide-xxxxxxxx   

* Currently, the users can remove the apps in the Management Portal. After removing the apps will cause that the status of apps can't be shown correctly in the AFS Portal.    

* In the Online Code IDE, the existing notebooks can not be saved after editing.   
	- Because of the AFS service instance used by the notebook to be edited is subscribed before removing the hidden space (before AFS v1.2.26 version). After removing the hidden space, the name and url of the online code IDE apps which are added by the users are changed (e.g., the name of apps is modified by "analytic-{name}-dev-{instance[0:8]}"). In the previous version of the service instance, when users edit the notebook, the corresponding name and url would not be found for SAVE.
	- Users are supposed to re-subscribe a new AFS service instance and move the existing notebooks to it.


