---
date: "2024-06-02 10:54:17"
title: Test post
comments: false
tags: 
- C
- MySQL
categories:
- 数据库
---

### Hello Hugo


```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <mysql/mysql.h>

int main() {
    int ret = 0;

    MYSQL mysql;
    MYSQL* connect = NULL;
    connect = mysql_init(&mysql);
    if (connect == NULL) {
        ret = mysql_errno(&mysql);
        printf("mysql_init error, %s\n", mysql_error(&mysql));
        return ret;
    }
    printf("mysql_init ok...\n");

    connect = mysql_real_connect(connect, "localhost", "shizhongqi", "shizhongqi", "test", 0, NULL, 0);
    if (connect == NULL) {
        ret = mysql_errno(&mysql);
        printf("mysql_real_connect error, err is: %s\n", mysql_error(&mysql));
        return ret;
    }
    printf("mysql_real_connect ok...\n");
    
    const char* query = "select * from emp";
    ret = mysql_query(connect, query);
    if (ret != 0) {
        printf("mysql_query error\n");
        return ret;
    }

    MYSQL_RES* result = mysql_store_result(&mysql);
    if (result == NULL) {
        printf("mysql_store_result error\n");
        return -1;
    }

    int field_num = mysql_field_count(&mysql);
    //表头
    MYSQL_FIELD* fields = mysql_fetch_fields(result);
    int i = 0;
    printf("------------------------------------------\n");
    for (i = 0; i < field_num; i++) {
        printf("%s \t", fields[i].name);
    }
    printf("\n------------------------------------------\n");

    //表内容
    MYSQL_ROW row = NULL;
    while (row = mysql_fetch_row(result)) {
        for (i = 0; i < field_num; i++) {
            printf("%s \t", row[i]);
        }
        printf("\n");
    }

    mysql_free_result(result);//释放内存
    mysql_close(connect);
    printf("mysql_close...\n");
    return 0;
}

```
