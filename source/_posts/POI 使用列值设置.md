---
title: POI使用—列值设定
tags: [工作总结,poi,column]
toc: true
---

## 背景
在使用POI操作Excel时，往往会有一些特殊的列值设定。这里进行记录。

### 设置列值为下拉选择
有的时候，需要设置某一列的值为固定选项的值，这时候就需要进行数据类值的设定。

```
 //生下拉框
String[] data = XXX;//生成数据
DVConstraint constraint = DVConstraint.createExplicitListConstraint(data);
//设置数据有效性加载在哪个单元格上,四个参数分别是：起始行、终止行、起始列、终止列
CellRangeAddressList regions = new CellRangeAddressList(currentRowIndex, 65535, 5, 5);
//数据有效性对象
HSSFDataValidation data_validation_list = new HSSFDataValidation(regions, constraint);
workbook.getSheetAt(0).addValidationData(data_validation_list);
```

### 设置值内容为文本

```
//设置日期列为文本模式
HSSFCellStyle textStyle = workbook.createCellStyle();
HSSFDataFormat format = workbook.createDataFormat();
textStyle.setDataFormat(format.getFormat("@"));
workbook.getSheetAt(0).setDefaultColumnStyle(3,textStyle);
```