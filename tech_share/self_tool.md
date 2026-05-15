#### 1、模版文本生成

```shell
 C:\Users\guoxi\Desktop\java_shell>java -Dfile.encoding=utf-8 Comander.java "INTO users(name,age,is_delete) VALUES('${param1}','${param2}',0)"  2 "John O'Connor" 25 "Jane Doe" 30
 === 批量处理开始 ===
 模板: INTO users(name,age,is_delete) VALUES('${param1}','${param2}',0)
 每组参数个数: 2
 参数组数: 2
 ================= 