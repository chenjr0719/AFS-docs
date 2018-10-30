# Troubleshooting

## Jupyter Kernel Die
修改前，每呼叫一次API，就會佔用512MB的記憶體。
GET /test
memory_str = ' ' * 512000000 * 1
print("OK")
修改後，在回傳結果前，將變數刪除。
GET /test
memory_str = ' ' * 512000000 * 1
del memory_str
print("OK")