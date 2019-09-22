# SAS notes
SAS notes

## Keywords

- `variable` 即column
- `observation` 即row
- `ODS` Output Delivery System

## Data types
SAS软件只有两类变量。

- 字符型
- 数值型

字符型变量

- 可包含任何值
- 用空白表示缺失值
- 最长可达32K

数值型变量

- 默认长度为8，数字值（不论其包含多少位数字）均存储为 8 个字节的浮点数，除非指定其他长度。

## Steps
SAS程序包含两类程序步，DATA步和PROC步。DATA 步通常用于创建或修改 SAS 数据集，但也可用来生成定制报表。

## Data set
创建一个新的data set.

```
data data_set_name;
```

给新的data set赋值。

```
data data_set_name;
  set Mylib.Productsales;
```

## Macro
### Macro variable
定义一个macro variable

```
%let variable_name = variable_value;
```

调用一个macro variable.You refer to the variable by preceding the variable name with an ampersand (&)

```
title "Data for &variable_value";
```

### Macro definition

定义一个macro.

```
%macro plot(yvar= ,xvar= );
   proc plot;
      plot &yvar*&xvar;
   run;
%mend plot;
```

调用一个macro.

```
%plot(yvar=income,xvar=age)
```
