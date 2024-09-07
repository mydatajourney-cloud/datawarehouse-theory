# Datawarehouse-theory and data modeling

## Overview

Đây là kiến thức mình nhặt được trong quá trình nghiên cứu các khoá học về data modeling và data warehouse.

## Table of Contents

1. [How to build data warehouse ?](#how-to-build-data-warehouse)
2. [Data warehouse vs data mart](#data-warehouse-vs-data-mart)
3. [Data warehouse tree](#data-warehouse-tree)
4. [Data warehouse architecture](#data-warehouse-architecture)
5. [Types of ETL](#types-of-etl)
6. [Data transformation principle](#data-transformation-principle)
7. [Facts, Facts Table, Dimension, Dimensional table](#facts-facts-table-dimension-dimensional-table)
8. [Principle of additivity](#principle-of-additivity)
9. [Star vs Snow Flake schema, key types](#star-vs-snow-flake-schema-key-types)
10. [Fact table types, Fact table government](#fact-table-types-fact-table-government)

## How to build data warehouse ?

- Integrated: DW là một môi trường tích hợp, nói cách khác nó lưu trữ dữ liệu từ nhiều nguồn khác nhau
- Subject oriented: Các dữ liệu lưu trữ trong DW phải được sắp xếp theo các chủ đề, dù cho nó từ nhiều nguồn và các hệ thống khác nhau
- Time variant: Các dữ liệu trong DW phải được lưu trữ ở các khoảng thời gian khác nhau chứ không chỉ là hiện tại.
- Non-volatile: DW không được chịu ảnh hưởng bởi các khoảng cập nhật dữ liệu, sau mỗi lần “refresh” DW sẽ được cập nhật dữ liệu theo các khối.
- ⇒ Hỗ trợ cho việc phân tích và ra quyết định dựa trên dữ liệu.

Tại sao phải xây dựng DW → Hỗ trợ việc phân tích và ra quyết định dựa trên dữ liệu → tại sao?

- Quan sát dữ liệu ở quá khứ
- Quan sát dữ liệu ở hiện tại
- Quan sát dữ liệu ở tương lai (dữ liệu mà ta tin là đúng ví dụ AI)
- Quan sát dữ liệu unknown

DW có thể build trên các:

- Relational database
- Cubes (multi-dimensional database)

## Data warehouse vs data mart

- Datawarehouse và datamart independent khá là giống nhau
- Datamart dependent có thể áp dụng trong những trường hợp khi dữ liệu từ datawarehouse đa dạng và khá nhiều chủ đề cần được chia nhỏ ra thành các datamart. Ví dụ phòng marketing chỉ có thể lấy dữ liệu từ datamart phù hợp so với phòng sale.

## Data warehouse tree
![image](https://github.com/user-attachments/assets/c47e900a-7594-4c8a-b019-67fc9d9520d9)

EDW: Enterprise
