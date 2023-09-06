# 源代码来源

## 基于lcov实现的增量代码UT覆盖率检查

https://www.cnblogs.com/wahaha02/p/5733755.html

## 源代码的原有说明

### 背景

配合CppUTest单元测试框架，lcov提供了一套比较完整的工程工具来对UT覆盖率进行度量。但对有些团队来说，历史负担太重，大量的遗留代码没有相应的UT。在这种情况下，对新增代码进行覆盖率检查，可能对团队来说是一种可行性较强的措施。在此目标基础上，并提出如下需求：

1. 利用现有的lcov资源；
1. 可以对指定git cmmit提交的代码进行UT覆盖率检查；
1. 可以指定需要UT覆盖率检查的软件模块、文件；
1. 可以设置UT覆盖率阈值；
1. 检查结果可视化展示，有良好的用户体验；

为实现如上需求，开发了一个ut_incremental_check.py 工具。

### 使用说明

ut_incremental_check.py有4个参数：

<since>..<until>：指定git commit SHA范围

<monitor_c_files>：指定需要关注的文件或目录列表，此参数要符合json数据格式

<lcov_dir>：lcov生成的目标文件目录

<threshold>：对新增代码UT覆盖率的下限要求。取值范围在(0,1]范围。

总体的工作流程见如下help说明。

```
$ ./ut_incremental_check.py

PURPOSE:
calculate UT coverage of git commits' new code

USAGE:
./ut_incremental_check.py <since>..<until> <monitor_c_files> <lcov_dir> <threshold>
example:
./ut_incremental_check.py "227b032..79196ba" '["source/soda/sp/lssp/i2c-v2/ksource"]' "coverage" 0.6

WORK PROCESS:
get changed file list between <since> and <until> , filter by <monitor_c_files> options;
get changed lines per changed file;
based on <lcov_dir>, search .gcov.html per file, and get uncover lines;
create report file:ut_incremental_check_report.html and check <threshold> (cover lines/new lines).

UT:
./ut_incremental_check.py ut
```


