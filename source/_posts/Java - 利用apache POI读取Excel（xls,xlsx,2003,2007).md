---
title: Java - 利用apache POI读取Excel（xls,xlsx,2003,2007)
keywords: [excel,java excel]
date: 2018/1/13 0:38
tags: [poi]
categories: java
---
一个简单的工具类，更多java小练习：https://github.com/hisenyuan/IDEAPractice
# 一、添加依赖
```
    <!-- POI start -->
    <dependency>
      <groupId>org.apache.poi</groupId>
      <artifactId>poi</artifactId>
      <version>3.9</version>
    </dependency>
    <dependency>
      <groupId>org.apache.poi</groupId>
      <artifactId>poi-ooxml</artifactId>
      <version>3.9</version>
    </dependency>
    <dependency>
      <groupId>commons-collections</groupId>
      <artifactId>commons-collections</artifactId>
      <version>3.2.2</version>
    </dependency>
    <!-- POI end -->
```
# 二、测试类
```
package com.hisen.jars.poi;

import java.io.File;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

/**
 * @author hisenyuan
 * @time 2018/1/12 17:43
 * @description 测试读取
 */
public class TestPoiExcelUtil {

  public static void main(String[] args) {
    File file = new File("C:\\work\\document\\银行信息.xlsx");
    try {
      // 每一个excelData为一行数据（存放在数组）
      List<String[]> excelData = POIExcelUtil.readExcel(file);
      for (String[] data:excelData) {
        System.out.println(Arrays.toString(data));
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

}
```
# 三、工具类代码
完整类：<a href="https://github.com/hisenyuan/IDEAPractice/blob/master/src/main/java/com/hisen/jars/poi/POIExcelUtil.java" target="_blank">POIExcelUtil.java</a>
```
package com.hisen.jars.poi;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;
import org.apache.log4j.Logger;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

/**
 * @author hisenyuan
 * @time 2018/1/12 16:35
 * @description 利用POI读取excel表格
 */
public class POIExcelUtil {

  private static Logger logger = Logger.getLogger(POIExcelUtil.class);
  private final static String XLS = "xls";
  private final static String XLSX = "xlsx";

  public static List<String[]> readExcel(File file) throws IOException {
    // 检查文件
    checkFile(file);
    Workbook workBook = getWorkBook(file);
    // 返回对象,每行作为一个数组，放在集合返回
    ArrayList<String[]> rowList = new ArrayList<>();
    if (null != workBook) {
      for (int sheetNum = 0; sheetNum < workBook.getNumberOfSheets(); sheetNum++) {
        // 获得当前sheet工作表
        Sheet sheet = workBook.getSheetAt(sheetNum);
        if (sheet == null) {
          continue;
        }
        // 获得当前sheet的开始行
        int firstRowNum = sheet.getFirstRowNum();
        // 获得当前sheet的结束行
        int lastRowNum = sheet.getLastRowNum();
        // 循环所有行(第一行为标题)
        for (int rowNum = firstRowNum; rowNum < lastRowNum; rowNum++) {
          // 获得当前行
          Row row = sheet.getRow(rowNum);
          if (row == null) {
            continue;
          }
          // 获得当前行开始的列
          short firstCellNum = row.getFirstCellNum();
          // 获得当前行的列数
          int lastCellNum = row.getPhysicalNumberOfCells();
          String[] cells = new String[row.getPhysicalNumberOfCells()];
          // 循环当前行
          for (int cellNum = firstCellNum; cellNum < lastCellNum; cellNum++) {
            Cell cell = row.getCell(cellNum);
            cells[cellNum] = getCellValue(cell);
          }
          rowList.add(cells);
        }
      }
    }
    return rowList;
  }
}
```