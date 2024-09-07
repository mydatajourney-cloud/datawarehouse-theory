# datawarehouse-theory and data modeling 
## Overview
Đây là kiến thức mình nhặt được trong quá trình học khoá học udemy về data warehouse. 
## Table of Contents

1. [Quy tắc khi xây dựng datawarehouse](#installation)
2. [Usage](#usage)
3. [Features](#features)
4. [Contributing](#contributing)
5. [License](#license)
6. [Getting Started](#getting-started)
7. [Configuration](#configuration)
8. [Examples](#examples)
9. [Troubleshooting](#troubleshooting)
10. [Contact](#contact)

## Installation
# How to build DW ?

- Interagrated: DW là một môi trường tích hợp, nói cách khác nó lưu trữ dữ liệu từ nhiều ngồn khác nhau
- Subject oriented: Các dữ liệu lưu trữ trong dw phải được sắp xếp theo các chủ đề, dù cho nó từ nhiều nguồn và các hệ thống khác nhau
- Time variant: Các dữ liệu trong DW phải được lưu dữ ở các khoảng thời gian khác nhau chứ không chỉ là hiện tại .
- Non-volatile: DW không được chịu ảnh hưởng bởi các khoảng cập nhật dữ liệu, sau mỗi lần “refresh”  dw sẽ được cập nhật dữ liệu theo các khối.
- ⇒ Hỗ trợ cho việc phân tích và ra quyết định dựa trên dữ liệu.

Tại sao phải xây dựng DW → Hỗ trợ việc phân tích và ra quyết định dựa trên dữ liệu → tại sao ?

- Quan sát dữ liệu ở quá khứ
- Quan sát dữ liệu ở hiện tại
- Quan sát dữ liệu ở tương lai ( dữ liệu mà ta tin là đúng ví dụ AI)
- Quan sát dữ liệu unknown

DW có thể build trên các: 

- Relational database
- Cubes (multi dimensional database
