- Ex 1
```
create 'notifications','attributes','metrics'
```

- Ex 2
```
put 'notifications',1, 'attributes:type','promotion'
put 'notifications',1, 'attributes:text','"Hi there! Buy this thing"'
```

```
put 'notifications',2, 'attributes:for_user','Chaz'
put 'notifications',2, 'attributes:type','Comment'
```

- Ex 3
```
put 'notifications',2, 'metrics:open',0
put 'notifications',2, 'metrics:open',1
```

- Ex 4
```
get 'notifications',2
get 'notifications',2,'metrics:open'
get 'notifications',2,'metrics:open','attributes:type'
```

- Ex 5
```
scan 'notifications',{COLUMNS => ['attributes:type'],LIMIT => 1,STARTROW => "2"}
```

- Ex 6
```
delete 'notifications',2, 'attributes:for_user','Chaz'
```

- Ex 7
```
disable 'notifications'
drop 'notifications'
```


#- People Example
```
create 'people', 'd'
```

```
put 'people', 'jmhsieh', 'd:hair', 'black',
```


```
get 'people', 'jmhsieh'
```

```
scan 'people'
```


```
put 'people', 'enis', 'd:hair', 'brown'
put 'people', 'stack', 'd:hair', 'none'
put 'people', 'busbey', 'd:beard', 'scruffy'
put 'people', 'apurtell', 'd:beard', 'grey'
```


```
scan 'people'
```


```
get 'people', 'stack'
```

```
get 'people', 'kathleen'
```


```
scan 'people', { STARTROW => 'kevin'}
```


```
put 'people', 'jmhsieh', 'd:role-hbase', 'pmc'
put 'people', 'stack', 'd:role-hbase', 'pmc'
put 'people', 'stack', 'd:role-phoenix', 'mentor'
put 'people', 'apurtell', 'd:role-hbase', 'chair'
```

```
scan 'people'
```


```
delete 'people', 'stack', 'd:hair'
```

```
put 'people', 'busbey', 'd:beard', 'trim'
```

```
scan 'people'
```


```
deleteall 'people', 'jmhsieh'
```

```
get 'people' , 'jmhsieh'
```
