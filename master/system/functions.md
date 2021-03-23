# 自定义 PQL 系统函数

可以使用 PQL 创建自定义系统函数，以让一些通用的处理逻辑在多个 PQL 中可以共享，自定义系统函数也可以理解为存储过程。由于 PQL 本身的性质，函数只能处理单值，而不是像 SQL 一样，函数可以依次处理集合中的多个值。从这方面讲，PQL 中系统函数在函数本身方面的作用反而小了些。

```sql
FUNCTION @CUSTOM_FUNC_NAME ($a, $b DEFAULT 1)
    BEGIN
        -- statement...

        RETURN $a + $b;
    END
```

自定义系统函数可以分为函数名称、函数参数和函数体三部分，其中函数参数可以为空。详见 [PQL 系统函数](/pql/global-function.md)中的说明。

## 页面说明

上方搜索支持搜索函数名称、所有者用户名或所有者全名，搜索结果始终按钮函数名称正序。在列表中函数体只显示其中一部分，完整的函数体请点击函数名称进入详情页。

---
参考链接

* [PQL 系统函数](/pql/global-function.md)
* [PQL 全局变量](/pql/global-variable.md)
* [自定义 PQL 全局变量](/master/system/variables.md)