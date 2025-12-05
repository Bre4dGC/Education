система решения ошибок при компиляции

исходник:
```brc
1 | func main()
2 | {
3 |     var name: str = "bread 
4 |     print(name 
5 | }
```

вывод:
```
3 |    var name: str = "bread
  |                          ^ Unclosed string literal
[ERROR] 1: main.brc at 1:27

4 |    print(name
  |              ^ Expected parenthesis  
[ERROR] 2: main.brc at 1:15

Do you want to fix these problems?
(y)es, (n)o, (N)umber: 1
```

решение:
```brc
1 | func main()
2 | {
3 |     var name: str = "bread" # solved: Unclosed string literal
4 |     print(name
5 | }
```