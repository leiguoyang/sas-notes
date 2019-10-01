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

## Naming convention

### Column name
Column names can be

- 1-32 characters
- must start with a letter or underscore and continue with letters, numbers or underscores.

### Library name
Library name

- 最多8个字符
- starts with letters or underscore
- continue with letters, numbers or underscores

## Statement
SAS programs consist of DATA and PROC steps, and each step consists of statements.

```
data ...;
  other statements;
run;
```

```
proc ...;
  other statements;
run;
```

Global statements are outside steps.

```
title ...;
```

```
options ...;
```

```
libname ...;
```
## Library
创建一个library reference.

```sas
libname libref engine path;
```

例如

```sas
libname sales 'c/user/data';
```

默认的engine是base。

用library读取excel文件。

```sas
libname NP xlsx '~/EPG194/data/np_info.xlsx';
```

`np_info.xlsx`这个文件的一个个worksheet就会变成`NP`里的一个个对应的data set，或者说SAS table.

## Importing data

导入CSV文件。

```
proc import datafile='path_to_file' dbms=csv out=library_name.data_set_name <replace>;
<guessingrows=n>;
run;
```

导入excel(xlsx)文件。

```
proc import datafile="path_to_file" dbms=xlsx out=library_name.data_set_name <replace>;
<sheet=worksheet_name>;
run;
```

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

## Plot

Plot可以显示两个或多个variables之间的关系。

```
proc plot data=data_set_name;
  plot vertical*horizontal;
run;
```

其中`vertical`是其中一个variable, 而`horizontal`是另外一个variable。如

```
proc plot data=data_set_name;
  plot price*date;
run;
```


详细使用请参考[Plotting one set of variables](https://documentation.sas.com/?cdcId=pgmsascdc&cdcVersion=9.4_3.4&docsetId=basess&docsetTarget=p1ebornamhs8z0n1vao2wlbfiwqb.htm&locale=zh-CN)

## Comment

A multi-line comment.

```
/* This is a comment */
data data_set_name;
  set ...;
run;
```

A single line comment.

```
* This is a commment;
```
