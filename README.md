# Datawarehouse-theory and data modeling

## Overview

Đây là kiến thức mình nhặt được trong quá trình nghiên cứu các khoá học về data modeling và data warehouse. Table of contents sẽ được viết bằng tiếng anh cho dễ tra cứu thêm và nội dung sẽ được viết bằng tiếng việt. 

## Table of Contents

1. [Quy tắc khi xây dựng datawarehouse ?](#how-to-build-data-warehouse)
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

EDW: Enterprise Data warehouse (old)

DATA LAKES: dành cho các công ty cần quản lý dữ liệu lớn.

⇒ One stop shopping: dữ liệu từ các nguồn đều tập trung vào 1 DW

⇒ Chịu ảnh hưởng bởi việc thay đổi dữ liệu trong DW (bad)

![image](https://github.com/user-attachments/assets/f6645751-2885-490a-8484-576c1e197bc9)

Depend DMS: dữ liệu từ data warehouse chạy dọc xuống các data marts

Front-end DMS: dùng khi một phần dữ liệu đi ngược từ các data marts được đưa xuống data warehouse cho những phân tích khác.

DW bus: cũ và nên là phương án cuối vì nhiều DMs quá sẽ không tốt.

Federated EDW: cũ và nên là phương án cuối vì nhiều DMs quá sẽ không tốt.

⇒ Decomposition: dữ liệu được chia nhỏ thành các thành phần, tránh ảnh hưởng bởi việc thay đổi DW.

## Data warehouse architecture

*Có 2 lớp ở trong data warehouse: Staging Layer và User Access Layer*

*Staging layer*: lưu trữ các dữ liệu lấy từ nguồn với điều kiện là các dữ liệu phải giống với nguồn và ít bị can thiệp.

- Nếu có nhiều nguồn từ nhiều application khác nhau = có nhiều bảng staging tương ứng
- Nếu có nhiều nguồn nhưng cùng một application = có gộp các bảng staging tương ứng

*Có 2 loại Staging Layer: duy trì và không duy trì*

- Staging Layer duy trì là dữ liệu ở staging sẽ không bị xoá khi mà dữ liệu đã đi vào User Access Layer.
    
    ⇒ Tốn rất nhiều storage để giữ lại dữ liệu
    
    ⇒ Có thể không kiểm soát được quyền truy cập người nào truy cập được dữ liệu ở staging layer.
    
- Staging Layer không duy trì là dữ liệu ở staging sẽ không được giữ lại khi mà dữ liệu đã đi vào User Access Layer.
    
    ⇒ Tốn ít storage hơn nhưng nếu dữ liệu ở User Access Layer có vấn đề (bị hư) hoặc cần phải xây dựng lại data warehouse thì không có dữ liệu có sẵn.
    
    ⇒ Ngoài ra lưu dữ liệu ở staging layer còn giúp cho việc kiểm tra chất lượng dữ liệu.

*User Access Layer*:

## Types of ETL

Có 2 loại ETL: ETL initial và ETL incremental

*ETL initial*: loại ETL này có nghĩa là ETL khởi đầu, tức là dữ liệu đi từ source đi vào staging layer, biến đổi và vào user access layer để xây dựng data warehouse lần đầu tiên.

- Được thực hiện khi data warehouse bắt đầu chạy lần đầu.
- Thực hiện lại ETL initial để xây dựng lại data warehouse từ đầu, khi mà data warehouse cũ gặp sự cố.
- Chỉ các dữ liệu liên quan mới đi vào data warehouse.

*ETL incremental*: loại ETL này có nghĩa là ETL tăng dần, tức là dữ liệu đi từ source đi vào staging layer, biến đổi và thêm vào user access layer khi mà trước đó user access layer đã có dữ liệu từ ETL initial.

- Được thực hiện khi data warehouse đã có dữ liệu.
- Thực hiện ETL incremental để thêm dữ liệu và thay đổi dữ liệu hoặc xoá dữ liệu.

⇒ Chúng ta sử dụng ETL incremental nhằm mục đích giúp cho data warehouse luôn được cập nhật. Lưu ý rằng dữ liệu data warehouse sẽ là dữ liệu tĩnh và non-volatile tức là trong khi các BI đang thực hiện phân tích dữ liệu thì data warehouse không thể bị thay đổi.

- Có 4 loại ETL incremental: append (thêm), inplace update (thay đổi tại vị trí), complete-replacement (thay đổi hoàn toàn), rolling update (thêm vào dữ liệu và xoá dữ liệu cũ).

⇒ Tuy nhiên, append và inplace-update được sử dụng phổ biến hơn.

*Mix and match of ETL incremental*: data warehouse sẽ được cập nhật dữ liệu từ nhiều nguồn khác nhau. Trong những nguồn khác nhau dữ liệu sẽ được cập nhật theo giờ, theo ngày hoặc theo tuần tuỳ theo thiết kế và yêu cầu.

## Data transformation principle

*Principle of unification*: mang ý nghĩa là dữ liệu phải được thống nhất với nhau khi được đưa vào dimensional table về mặt ngữ nghĩa và kích thước của các loại dữ liệu (char, int…)

⇒ Ví dụ có 2 cột đều mang ý nghĩa là cấp bậc của các giảng viên, cột 1 có các giá trị lần lượt là giáo sư, tiến sĩ, thạc sĩ và cột 2 có các giá trị lần lượt là GS, TS, Ths. Ta có thể hợp dữ liệu lại sao cho các cột cấp bậc có dữ liệu nhất quán trong dimension table. Cột cấp bậc sẽ được đổi thành: GS, TS, Ths cho nhất quán với cột 2.

*Principle of de-duplication*: mang ý nghĩa là dữ liệu không được trùng lặp khi đưa vào master dimensional table.

⇒ Ví dụ một sinh viên A có thể đăng ký 2 môn học ở 2 khoa khác nhau chính vì thế dữ liệu của sinh viên A sẽ đều nằm ở 2 bảng (tương ứng 2 khoa). Tuy nhiên master dimensional table chỉ yêu thông tin sinh viên A chính vì thế ta phải loại bỏ dữ liệu trùng lặp trước khi đưa vào.

*Principle of vertical slicing*: mang ý nghĩa là các cột mà chúng ta nghĩ sẽ không cần thiết cho việc phân tích sẽ được loại bỏ

*Principle of horizontal slicing*: mang ý nghĩa các dòng mà chúng ta thấy rằng sẽ không cần thiết hoặc bị lỗi sẽ được lọc hoặc correct lại.

⇒ Sẽ có những use case khi dữ liệu sẽ được phân ra thành các data marts, mỗi data mart sẽ chỉ có nội dung chuyên sâu khác nhau để phân tích sâu hơn. Vì thế ta cần phải lọc dữ liệu tương ứng với những data marts khác nhau.

⇒ Ví dụ có rất nhiều phòng ban trong công ty và mỗi phòng ban có thể có một data mart. Ta chỉ muốn bỏ dữ liệu marketing vào data mart marketing nên ta sẽ lọc những phòng ban khác ra.

## Facts, Facts Table, Dimension, Dimensional table

*Facts*:

- Thuộc dạng số và có thể đếm được
- Là những giá trị đo lường (metric)
- Là những giá trị đo đếm (measurements). Ví dụ như giá trị trung bình điểm, học sinh có điểm cao nhất lớp..

⇒ Ví dụ: tiền lương, số năm…

*Facts Table*: là nơi chứa những Facts

*Dimension*: là bối cảnh (chủ đề) của những Facts

⇒ Ví dụ như: phòng ban, giảng viên, học sinh…

*Dimensional table*: bảng lưu trữ các Dimension

## Principle of additivity

- Additivity principle là một nguyên lý trong data warehousing mà yêu cầu dữ liệu phải có khả năng cộng dồn, tính toán hoặc tổng hợp được.

## Star vs Snow Flake schema, key types

- **Star Schema**: một loại schema trong đó fact table được liên kết trực tiếp với các dimension tables.
  
- **Snow Flake Schema**: một biến thể của star schema trong đó các dimension tables được chuẩn hóa thành nhiều bảng con.

- **Key Types**: bao gồm primary key, foreign key, và surrogate key.

## Fact table types, Fact table government

- **Fact Table Types**: các loại fact tables bao gồm transactional, snapshot, và accumulating.

- **Fact Table Government**: quản lý fact table liên quan đến việc duy trì tính toàn vẹn của dữ liệu và chính sách cập nhật.

