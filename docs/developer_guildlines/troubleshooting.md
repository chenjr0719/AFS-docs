# Troubleshooting

## Jupyter Kernel Die

1. Memory GC issue
修改前，每呼叫一次API，就會佔用512MB的記憶體。
GET /test
memory_str = ' ' * 512000000 * 1
print("OK")
修改後，在回傳結果前，將變數刪除。
GET /test
memory_str = ' ' * 512000000 * 1
del memory_str
print("OK")

2. Disk full issue


3. Others

## Side Effect of Removing the Hidden Space
* 在WISE-PaaS AFS Online code IDE針對已存在的notebook進行編輯後無法存檔   
經盤查後，了解欲編輯的notebook所使用的AFS service instance是在刪除hidden space之前(AFS v1.2.26之前的版本)訂閱。在刪除hidden space後，使用者所新增的online code IDE的名稱與url都有所更動(e.g., analytic-{name}-dev-{instance[0:8]})，因此用戶在舊版本的service instance編輯notebook後，會找不到相對應的名稱與url進行SAVE，導致此問題狀況。
此為刪除hidden space之後的side effect，建議使用者重新訂閱一新的AFS service instance，並將現有的notebook程式搬移至新訂閱的service instance繼續使用，即可避免此問題。

* In the WISE-PaaS AFS Online code IDE, the existing notebooks can not SAVE after editing.
After the check, understand that the AFS service instance used by the notebook to be edited is subscribed before the hidden space (AFS v1.2.26 version). After deleting the hidden space, the name and url of the online code IDE added by the user are changed (eg, analytic-{name}-dev-{instance[0:8]}), so the user is in the old version. After the service instance edits the notebook, it will not find the corresponding name and url for SAVE, which will cause the problem.
This is the side effect after deleting the hidden space. Users are advised to re-subscribe to a new AFS service instance and move the existing notebook program to the service instance of the new subscription to continue to use it.

* 從Management Portal將使用者建立的app刪除後，會導致AFS Portal無法正確列出Analytic List的status
而這些行為有些會直接導致AFS操作不能。AFS應該如何應對這些來至使用者的破壞性操作需要被討論。


## Others
* 文件远小于2GB, 发生超出空间错误（StorageDataError: BotoClientError: Out of space for destination file）
Root cause: 去查看afs-axa裡的Jupyter，發現Disk空間已是2G of 2G，導致使用Boto client去Blob store取檔案時，拋出例外訊息。
Solution: AFS 1.2.26版之後所訂閱的AFS Instance會將Jupyter及Node RED部署至訂閱AFS Instance當下的Space裡，用戶可透過CF CLI去取得當前App的Disk使用量(目前Management Portal只顯示App的Disk大小無法顯示用量)，若空間不足可透過CLI命令去App裡刪除或是重啟App。

查看目前App所使用的Disk用量: cf app APP_NAME
進入到App裡: cf ssh APP_NAME
重啟App: cf restart APP_NAME


