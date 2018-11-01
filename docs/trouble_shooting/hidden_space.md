# Side Effect of Removing the Hidden Space

# Side Effect of Removing the Hidden Space
* To avoid the duplicated APPs' name, the naming rules are revised as follows:
    - Jupyter => code-ide-{instance[0:8]}, {instance_id}-jupyter.wise-paas.com => code-ide-{instance[0:8]}.wise-paas.com
    - Node-Red => flow-ide-{instance[0:8]}, {instance_id}-node-red.wise-paas.com => flow-ide-{instance[0:8]}.wise-paas.com
    - {name}-dev => analytic-{name}-dev-{instance[0:8]}, {instance_id}-{name}-dev.wise-paas.com => analytic-{name}-dev-{instance[0:8]}.wise-paas.com

* After removing the hidden space, the APPs are listed in the management portal. The users can remove any APPs by themself, but removing some specific APPs will damage the AFS service instance. The list of APPs as follows are the dependency of AFS, please DON'T remove them:
	- code-ide-xxxxxxxx
	- flow-ide-xxxxxxxx   

* Currently, the users can remove the APPs in the management portal. After removing the APPs will cause that the status of APPs can't be shown correctly in the AFS Portal.    

* In the WISE-PaaS AFS Online code IDE, the existing notebooks can not SAVE after editing.   
	- Because of the AFS service instance used by the notebook to be edited is subscribed before removing the hidden space (before AFS v1.2.26 version). After removing the hidden space, the name and url of the online code IDE APPs which are added by the users are changed (e.g., the name of APPs is modified by "analytic-{name}-dev-{instance[0:8]}"). In the previous version of the service instance, when users edit the notebook, the corresponding name and url would not be found for SAVE.
	- Users are supposed to re-subscribe a new AFS service instance and move the existing notebooks to it.


