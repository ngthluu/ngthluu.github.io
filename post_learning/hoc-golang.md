---
sort: 1
---

# Ngôn ngữ lập trình: Golang

Bài viết được dịch ngựa từ trang [https://golang.org/ref/spec](https://golang.org/ref/spec)

## Mục lục
* [Giới thiệu](#giới-thiệu)
* [Source code represetation](#source-code-representation)
    * [Kí tự (Characters)](#kí-tự-characters)
    * [Chữ và số (Letters and digits)](#chữ-và-số-letters-and-digits)
* [Những thành phần của ngôn ngữ](#những-thành-phần-của-ngôn-ngữ)
    * [Comments](#comments)
    * [Tokens](#tokens)
    * [Chấm phẩy](#chấm-phẩy)
    * [Định danh (Identifiers)](#định-danh-identifiers)
    * [Từ khóa (Keywords)](#từ-khóa-keywords)
    * [Toán tử và chấm câu (Operators and punctuation)](#toán-tử-và-chấm-câu-operators-and-punctuation)

## Giới thiệu
Go là một ngôn ngữ lập trình đa năng, được thiết kế theo phong cách `system programming`. Go là một ngôn ngữ `strongly typed` và có `garbage-collector` cũng như hỗ trợ cho `concurrent programming`. Chương trình viết bằng Go sẽ được tổ chức dưới dạng `packages`, giúp việc quản lý dependencies được hiệu quả.

Ngữ pháp của Go rất nhỏ gọn và đơn giản, từ đó cho phép phân tích dễ dàng bởi các công cụ tự động như các IDE (integrated development environment).

## Source code representation
Source code được thể hiện bằng Unicode (bảng mã UTF-8). Một số lưu ý:
- Một `kí tự có dấu` sẽ khác với một kí tự tương đương được tạo bởi `1 dấu + 1 kí tự thường` (coi như 2 kí tự khác nhau).
- Để cho dễ, khái niệm `kí tự` (character) sẽ tương đương với `một Unicode code point`.
- `Kí tự thường` và `kí tự in hoa` là khác nhau.
- Implementation restrictions:
    - Để tương thích với một số tools khác, compiler có thể sẽ không cho phép kí tự `NUL (U+0000)` trong source.
    - Để tương thích với một số tools khác, compiler có thể bỏ qua `UTF-8-encoded byte order mark (U+FEFE)` nếu nó là kí tự Unicode đầu tiên trong source. Có thể sẽ không cho phép một `byte order mark` ở những vị trí khác.

### Kí tự (Characters)
```
newline        = /* U+000A */
unicode_char   = /* kí tự Unicode bất kì trừ newline */
unicode_letter = /* một kí tự Unicode được phân loại là chữ */
unicode_digit  = /* một kí tự Unicode được phân loại là số, chữ số thập phân */
```

### Chữ và số (Letters and digits)
```
letter        = unicode_letter và _
decimal_digit = 0 -> 9
binary_digit  = 0, 1
octal_digit   = 0 -> 7
hex_digit     = 0 -> 9, a -> f và A -> F
```

## Những thành phần của ngôn ngữ

### Comments
Comments dùng cho program documentation. Có 2 loại:
1. Line comments. Bắt đầu từ // và kết thúc ở cuối hàng.
2. General comments. Bắt đầu /*, kết thúc ở */ gần nhất.
Một comment không thể bắt đầu trong một `rune`, một `chuỗi kí tự`, hay một `comment`. Một `general comment mà không có newline` sẽ được đổi thành một `space`. Những `comment khác` được đổi thành một `newline`.

### Tokens
Tokens cấu thành nên từ vựng của ngôn ngữ Go. Tokens chia làm 4 lớp: định danh (`identifiers`), từ khóa (`keywords`), toán tử và chấm câu (`operators and punctuation`) và chuỗi (`literals`). 

White space, được cấu thành bởi `spaces` (U+0020), `tabs ngang` (U+0009), `carriage returns` (U+000D) và `newlines` (U+000A), sẽ bị bỏ qua trừ khi chúng chia những tokens khác. Ngoài ra, newline hoặc EOF có thể trigger việc chèn một dấu chấm phẩy. Khi breaking input thành các tokens, token kế tiếp là chuỗi dài nhất các kí tự có thể cấu thành một token hợp lệ.

### Chấm phẩy
Ngữ pháp chuẩn sử dụng dấu chấm phẩy ";" như terminators. Chương trình Go bỏ qua các dấu chấm phẩy thông qua các luật sau:
1. Khi input được break thành tokens, dấu chấm phẩy được thêm vào ngay lập tức vào token stream ngay sau token cuối của dòng khi token cuối là:
    * Một định danh.
    * Một số nguyên, thực, phức, rune hoặc chuỗi kí tự.
    * Một trong các keywords: `break`, `continue`, `fallthrough`, `return`.
    * Một trong các toán tử và chấm câu (operators and punctuation): ++, --, ), ], }
2. Để cho phép một statement phức tạp có thể nằm trên 1 dòng, dấu chấm phẩy có thể được bỏ qua trước ), }.

### Định danh (Identifiers)
Định danh giúp đặt tên những thực thể của chương trình như biến và kiểu. 
- Là chuỗi của một hoặc nhiều hơn một letters + digits.
- Bắt đầu bằng letter.
Một số định danh đã được định nghĩa trước (predeclared).

### Từ khóa (Keywords)
```
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

### Toán tử và chấm câu (Operators and punctuation)
```
+    &     +=    &=     &&    ==    !=    (    )
-    |     -=    |=     ||    <     <=    [    ]
*    ^     *=    ^=     <-    >     >=    {    }
/    <<    /=    <<=    ++    =     :=    ,    ;
%    >>    %=    >>=    --    !     ...   .    :
     &^          &^=
```