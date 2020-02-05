将下方文本中的key_id移动到指定位置：
```
<kks_struct_value id="6523" description="" enabled="1" code="A" name="全厂公用系统" key_id="11" />
<kks_struct_value id="6523" description="" enabled="1" code="A" name="全厂公用系统" key_id="12" />
<kks_struct_value id="6523" description="" enabled="1" code="A" name="全厂公用系统" key_id="133" />
<kks_struct_value id="6523" description="" enabled="1" code="A" name="全厂公用系统" key_id="14" />
```
使之变成：
```
<kks_struct_value id="6523" key_id="11" description="" enabled="1" code="A" name="全厂公用系统"  />
<kks_struct_value id="6523" key_id="12" description="" enabled="1" code="A" name="全厂公用系统"  />
<kks_struct_value id="6523" key_id="133" description="" enabled="1" code="A" name="全厂公用系统"  />
<kks_struct_value id="6523" key_id="14" description="" enabled="1" code="A" name="全厂公用系统"  />
```

正则表达式：
```
查询：(<.+)(\sid=\S+)([^>]+)(key_id\S+)(.+)$
替换：$1$2 $4$3$5
```
解释：查询中的每一个括号中的结果为一个分组，共分成5个组，此时key_id属于第4分组，将4和3替换位置即可。