# Datawarehouse Theory and Data Modeling

## Overview

Đây là kiến thức mình nhặt được trong quá trình nghiên cứu các khoá học về data modeling và data warehouse. **Table of Contents** sẽ được viết bằng tiếng Anh cho dễ tra cứu thêm và nội dung sẽ được viết bằng tiếng Việt.

## Table of Contents

1. [How to Build Data Warehouse?](#how-to-build-data-warehouse)
2. [Data Warehouse vs Data Mart](#data-warehouse-vs-data-mart)
3. [Data Warehouse Tree](#data-warehouse-tree)
4. [Data Warehouse Architecture](#data-warehouse-architecture)
5. [Types of ETL](#types-of-etl)
6. [Data Transformation Principle](#data-transformation-principle)
7. [Facts, Facts Table, Dimension, Dimensional Table](#facts-facts-table-dimension-dimensional-table)
8. [Principle of Additivity](#principle-of-additivity)
9. [Star vs Snow Flake Schema, Key Types](#star-vs-snow-flake-schema-key-types)
10. [Fact Table Types, Fact Table Government](#fact-table-types-fact-table-government)

## How to Build Data Warehouse?

- **Integrated**: DW là một môi trường tích hợp, nói cách khác nó lưu trữ dữ liệu từ nhiều nguồn khác nhau.
- **Subject Oriented**: Các dữ liệu lưu trữ trong DW phải được sắp xếp theo các chủ đề, dù cho nó từ nhiều nguồn và các hệ thống khác nhau.
- **Time Variant**: Các dữ liệu trong DW phải được lưu trữ ở các khoảng thời gian khác nhau chứ không chỉ là hiện tại.
- **Non-Volatile**: DW không bị ảnh hưởng bởi các khoảng cập nhật dữ liệu, sau mỗi lần “refresh” DW sẽ được cập nhật dữ liệu theo các khối.

  ⇒ Hỗ trợ cho việc phân tích và ra quyết định dựa trên dữ liệu.

Tại sao phải xây dựng DW? → Hỗ trợ việc phân tích và ra quyết định dựa trên dữ liệu → Tại sao?

- Quan sát dữ liệu ở quá khứ
- Quan sát dữ liệu ở hiện tại
- Quan sát dữ liệu ở tương lai (dữ liệu mà ta tin là đúng ví dụ AI)
- Quan sát dữ liệu unknown

DW có thể build trên các:

- Relational Database
- Cubes (Multi-Dimensional Database)

## Data Warehouse vs Data Mart

- **Data Warehouse** và **Data Mart** independent khá là giống nhau.
- **Data Mart Dependent** có thể áp dụng trong những trường hợp khi dữ liệu từ Data Warehouse đa dạng và khá nhiều chủ đề cần được chia nhỏ ra thành các Data Mart. Ví dụ: Phòng marketing chỉ có thể lấy dữ liệu từ Data Mart phù hợp so với phòng sale.

## Data Warehouse Tree

![Data Warehouse Tree](https://github.com/user-attachments/assets/c47e900a-7594-4c8a-b019-67fc9d9520d9)

- **EDW (Enterprise Data Warehouse)**: Old model.
- **DATA LAKES**: Dành cho các công ty cần quản lý dữ liệu lớn.

  ⇒ **One Stop Shopping**: Dữ liệu từ các nguồn đều tập trung vào 1 DW.

  ⇒ **Chịu ảnh hưởng bởi việc thay đổi dữ liệu trong DW** (bad).

![Data Warehouse Tree](https://github.com/user-attachments/assets/f6645751-2885-490a-8484-576c1e197bc9)

- **Depend DMS**: Dữ liệu từ Data Warehouse chạy dọc xuống các Data Marts.
- **Front-end DMS**: Dùng khi một phần dữ liệu từ các Data Marts được đưa xuống Data Warehouse cho những phân tích khác.
- **DW Bus**: Cũ và nên là phương án cuối vì nhiều DMs quá sẽ không tốt.
- **Federated EDW**: Cũ và nên là phương án cuối vì nhiều DMs quá sẽ không tốt.

  ⇒ **Decomposition**: Dữ liệu được chia nhỏ thành các thành phần, tránh ảnh hưởng bởi việc thay đổi DW.

## Data Warehouse Architecture

**Có 2 lớp ở trong Data Warehouse**:

- **Staging Layer**: Lưu trữ các dữ liệu lấy từ nguồn với điều kiện là các dữ liệu phải giống với nguồn và ít bị can thiệp.
  
  - Nếu có nhiều nguồn từ nhiều application khác nhau = có nhiều bảng staging tương ứng.
  - Nếu có nhiều nguồn nhưng cùng một application = có gộp các bảng staging tương ứng.

  **Có 2 loại Staging Layer**:

  - **Duy trì**: Dữ liệu ở staging sẽ không bị xoá khi mà dữ liệu đã đi vào User Access Layer.
    
    ⇒ Tốn rất nhiều storage để giữ lại dữ liệu.
    
    ⇒ Có thể không kiểm soát được quyền truy cập người nào truy cập được dữ liệu ở staging layer.

  - **Không duy trì**: Dữ liệu ở staging sẽ không được giữ lại khi mà dữ liệu đã đi vào User Access Layer.
    
    ⇒ Tốn ít storage hơn nhưng nếu dữ liệu ở User Access Layer có vấn đề (bị hư) hoặc cần phải xây dựng lại Data Warehouse thì không có dữ liệu có sẵn.
    
    ⇒ Ngoài ra lưu dữ liệu ở staging layer còn giúp cho việc kiểm tra chất lượng dữ liệu.

- **User Access Layer**: (Chưa có nội dung mô tả chi tiết trong README)

## Types of ETL

Có 2 loại ETL: **ETL Initial** và **ETL Incremental**.

- **ETL Initial**: Loại ETL này có nghĩa là ETL khởi đầu, tức là dữ liệu đi từ source vào staging layer, biến đổi và vào user access layer để xây dựng Data Warehouse lần đầu tiên.
  
  - Được thực hiện khi Data Warehouse bắt đầu chạy lần đầu.
  - Thực hiện lại ETL Initial để xây dựng lại Data Warehouse từ đầu khi mà Data Warehouse cũ gặp sự cố.
  - Chỉ các dữ liệu liên quan mới đi vào Data Warehouse.

- **ETL Incremental**: Loại ETL này có nghĩa là ETL tăng dần, tức là dữ liệu đi từ source vào staging layer, biến đổi và thêm vào user access layer khi mà trước đó user access layer đã có dữ liệu từ ETL Initial.
  
  - Được thực hiện khi Data Warehouse đã có dữ liệu.
  - Thực hiện ETL Incremental để thêm dữ liệu và thay đổi dữ liệu hoặc xoá dữ liệu.

  ⇒ Chúng ta sử dụng ETL Incremental nhằm mục đích giúp cho Data Warehouse luôn được cập nhật. Lưu ý rằng dữ liệu Data Warehouse sẽ là dữ liệu tĩnh và non-volatile tức là trong khi các BI đang thực hiện phân tích dữ liệu thì Data Warehouse không thể bị thay đổi.

  - Có 4 loại ETL Incremental: **Append (Thêm)**, **Inplace Update (Thay đổi tại vị trí)**, **Complete-Replacement (Thay đổi hoàn toàn)**, **Rolling Update (Thêm vào dữ liệu và xoá dữ liệu cũ)**.

  ⇒ Tuy nhiên, **Append** và **Inplace Update** được sử dụng phổ biến hơn.

- **Mix and Match of ETL Incremental**: Data Warehouse sẽ được cập nhật dữ liệu từ nhiều nguồn khác nhau. Trong những nguồn khác nhau, dữ liệu sẽ được cập nhật theo giờ, theo ngày hoặc theo tuần tuỳ theo thiết kế và yêu cầu.

## Data Transformation Principle

- **Principle of Unification**: Dữ liệu phải được thống nhất với nhau khi được đưa vào dimensional table về mặt ngữ nghĩa và kích thước của các loại dữ liệu (char, int…).

  ⇒ Ví dụ: Có 2 cột đều mang ý nghĩa là cấp bậc của các giảng viên, cột 1 có các giá trị lần lượt là giáo sư, tiến sĩ, thạc sĩ và cột 2 có các giá trị lần lượt là GS, TS, Ths. Ta có thể hợp dữ liệu lại sao cho các cột cấp bậc có dữ liệu nhất quán trong dimension table. Cột cấp bậc sẽ được đổi thành: GS, TS, Ths cho nhất quán với cột 2.

- **Principle of De-Duplication**: Dữ liệu không được trùng lặp khi đưa vào master dimensional table.

  ⇒ Ví dụ: Một sinh viên A có thể đăng ký 2 môn học ở 2 khoa khác nhau chính vì thế dữ liệu của sinh viên A sẽ đều nằm ở 2 bảng (tương ứng 2 khoa). Tuy nhiên master dimensional table chỉ yêu thông tin sinh viên A chính vì thế ta phải loại bỏ dữ liệu trùng lặp trước khi đưa vào.

- **Principle of Vertical Slicing**: Các cột mà chúng ta nghĩ sẽ không cần thiết cho việc phân tích sẽ được loại bỏ.

- **Principle of Horizontal Slicing**: Các dòng mà chúng ta thấy rằng sẽ không cần thiết hoặc bị lỗi sẽ được lọc hoặc correct lại.

  ⇒ Ví dụ: Có rất nhiều phòng ban trong công ty và mỗi phòng ban có thể có một Data Mart. Ta chỉ muốn bỏ dữ liệu marketing vào Data Mart marketing nên ta sẽ lọc những phòng ban khác ra.

## Facts, Facts Table, Dimension, Dimensional Table

- **Facts**:
  - Thuộc dạng số và có thể đếm được.
  - Là những giá trị đo lường (metric).
  - Là những giá trị đo đếm (measurements). Ví dụ: tiền lương, số năm…

- **Facts Table**: Là nơi chứa những Facts.

- **Dimension**: Là bối cảnh (chủ đề) của những Facts.

  ⇒ Ví dụ: Phòng ban, giảng viên, học sinh…

- **Dimensional Table**: Bảng lưu trữ các Dimension.

## Principle of Additivity

Có 3 loại additivity: **Additivity**, **Non-Additivity**, và **Semi-Additivity**.

- **Additivity**: Áp dụng khi ta muốn tính tổng của một giá trị nào đó.

  ⇒ Ví dụ: Ta có một bảng danh sách học phí của trường từ năm 2015-2018 gồm id, tên, số tiền mà cô/anh ấy phải trả cho một năm học, năm học. Additivity được áp dụng theo cột khi ta muốn tính **tổng tiền** mà trường nhận được từ các sinh viên. Ngoài ra, Additivity được áp dụng theo dòng khi ta muốn biết **tổng tiền** mà một học sinh A phải đóng từ năm (2015-2017).

- **Non-Additivity**: Áp dụng để nhắc rằng các số như phần trăm, điểm trung bình, tính trung bình không được phép cộng.

- **Semi-Additivity**: Áp dụng cho periodic snapshot, dạng này có nghĩa là value có thể hoặc không có thể tính tổng một giá trị nào đó.

## Star vs Snow Flake Schema, Key Types

- **Star Schema**: Tất cả level của một mô hình cây sẽ được quy thành 1 dimension table. Cần sử dụng join ít vì không có nhiều dimension tables. Các bảng liên kết nhau theo các khoá chính (primary key) và khoá ngoại (foreign key).

- **Snow Flake Schema**: Các level của mô hình cây sẽ tương ứng với số dimension tables. Cần sử dụng join nhiều hơn vì phải chia mô hình cây thành các dimension tables và sử dụng khoá và khoá ngoại sẽ phức tạp hơn.

**Có 4 loại key để biểu diễn mối quan hệ dữ liệu**:

- **Primary Key**: Là dữ liệu định danh của bảng (ID).
- **Foreign Key**: Là khoá chính của bảng khác.
- **Natural Key**: Là dữ liệu định danh thường đi vào từ source thay vì được tạo ra từ hệ thống.
- **Surrogate Key**: Là dữ liệu định danh thường được tạo ra từ hệ thống và không mang ý nghĩa bên ngoài như natural key.

## Fact Table Types, Fact Table Government

Có 4 loại Fact Table:

- **Transaction**: Ghi lại những facts trong transaction.
- **Periodic Snapshot**: Ghi lại những measurements theo một thời gian lặp lại nhất định.
- **Accumulating Snapshot**: Ghi lại những business processes.
- **Factless**: Ghi lại sự hiện diện của một event.

**Transaction Fact Table**: Là loại bảng rất quan trọng trong Data Warehouse.

  - Để tạo được một bảng Fact, ta cần có “measure” và “context measure”.

  ⇒ Ví dụ: Fact Table của ta tên `Học_Phí_fact`. Measure của nó sẽ là `số_tiền`, context measure sẽ là `Key ID` của học sinh và `Key` của ngày thanh toán.

**Periodic Snapshot Table**: Đối với những use case cần phải theo dõi sự thay đổi của những measurements, ta có thể tách periodic snapshot table và transaction table để giúp việc phân tích dễ dàng hơn.

  ⇒ Ví dụ: Theo dõi mức chi tiêu của khách hàng sử dụng thẻ theo tuần.

**Fact Table Rules**:

- Các Facts phải có cùng mức độ chi tiết và dimension thì mới có thể đưa vào cùng một bảng Fact.
- Các Facts phải có thời điểm xảy ra với nhau thì mới có thể đưa vào cùng một bảng Fact.

  ⇒ Ví dụ: Nếu cả 2 Fact đều về Billing (tiền học và tiền hoạt động ngoại khoá), thì cả 2 Fact này đều nằm trong cùng một bảng Fact.
