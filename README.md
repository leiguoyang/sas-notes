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

```sas
data ...;
  other statements;
run;
```

```sas
proc ...;
  other statements;
run;
```

Global statements are outside steps.

```sas
title ...;
```

```sas
options ...;
```

```sas
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

```sas
proc import datafile='path_to_file' dbms=csv out=library_name.data_set_name <replace>;
<guessingrows=n>;
run;
```

导入excel(xlsx)文件。

```sas
proc import datafile="path_to_file" dbms=xlsx out=library_name.data_set_name <replace>;
<sheet=worksheet_name>;
run;
```

## Exporting data
```sas
proc export data=library_name.data_set_name
file='path_to_file'
dbms=dbms_name
<replace>;
run;
```

导出成excel文件
```sas
proc export data=sdf.orders
file='&tmpdir.text.xlsx'
dbms=xlsx
replace;
sheet='orders';
run;
```

## Data set
创建一个新的data set.

```sas
data data_set_name;
```

给新的data set赋值。

```sas
data data_set_name;
  set Mylib.Productsales;
```

## Data step

- Subsetting
- Create new columns
- Some complicated logic with `if` flow control or with `do` loop

### Subsetting

Select specific rows with `if` or `where`.

```sas
data result;
  set sashelp.cars;
  if Make = 'Audi';
run;
```

or

```sas
data result;
  set sashelp.cars;
  where Make = 'Audi';
run;
```

Select specific rows with multiple conditions.

```sas
data result;
  set sashelp.cars;
  where Make = 'Audi' and Length > 150;
run;
```

Select specific columns.

```sas
data result;
  set sashelp.cars;
  keep Make Model Length;
run;
```

Select specific rows and columns.

```sas
data result;
  set sashelp.cars;
  keep Make Model Length;
  where Make = 'Audi' and Length > 150;
run;
```

## Macro
### Macro variable
定义一个macro variable

```sas
%let variable_name = variable_value;
```

调用一个macro variable.You refer to the variable by preceding the variable name with an ampersand (&)

```sas
title "Data for &variable_value";
```

### Macro definition

定义一个macro.

```sas
%macro plot(yvar= ,xvar= );
   proc plot;
      plot &yvar*&xvar;
   run;
%mend plot;
```

调用一个macro.

```sas
%plot(yvar=income,xvar=age)
```

### Do statement

```sas
%macro do_sth();
  %do i = 1 %to 10;
    %put Hello;
  %end;
%mend;

%do_sth;
```

## Plot

Plot可以显示两个或多个variables之间的关系。

```sas
proc plot data=data_set_name;
  plot vertical*horizontal;
run;
```

其中`vertical`是其中一个variable, 而`horizontal`是另外一个variable。如

```sas
proc plot data=data_set_name;
  plot price*date;
run;
```


详细使用请参考[Plotting one set of variables](https://documentation.sas.com/?cdcId=pgmsascdc&cdcVersion=9.4_3.4&docsetId=basess&docsetTarget=p1ebornamhs8z0n1vao2wlbfiwqb.htm&locale=zh-CN)

## Comment

A multi-line comment.

```sas
/* This is a comment */
data data_set_name;
  set ...;
run;
```

A single line comment.

```sas
* This is a commment;
```

## SQL

### Creating a table

```
proc sql;
  create table table_name as query_result_set;
```

## Links

- [SAS documentation](https://documentation.sas.com/?cdcId=pgmsascdc&cdcVersion=9.4_3.5&docsetId=lestmtsref&docsetTarget=p1awxgleif5wlen1pja0nrn6yi6i.htm&locale=en)
- [Free SAS training](https://www.sas.com/en_us/training/offers/free-training.html)
- [Base SAS](https://support.sas.com/en/software/base-sas-support.html)
- [Graph](https://documentation.sas.com/?docsetId=graphref&docsetTarget=p15qcl2nzalw4zn1fp6rkrf3n9kn.htm&docsetVersion=9.4&locale=en)
- [Format and informat](https://documentation.sas.com/?docsetId=leforinforref&docsetTarget=p09lpr3kmbh8fen1qepuv6zc1ldd.htm&docsetVersion=9.4&locale=en)
