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
    * [Số nguyên](#số-nguyên)
    * [Số thực dấu chấm động](#số-thực-dấu-chấm-động)
    * [Số ảo](#số-ảo)
    * [Rune](#rune)
    * [Chuỗi kí tự](#chuỗi-kí-tự)
* [Hằng](#hằng)
    

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

### Chữ và chữ số (Letters and digits)
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
Tokens cấu thành nên từ vựng của ngôn ngữ Go. Tokens chia làm 4 lớp: định danh (`identifiers`), từ khóa (`keywords`), toán tử và chấm câu (`operators and punctuation`) và `literals`. 

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

### Số nguyên
Số nguyên (integer literal) là một chuỗi các chữ số, biểu thị một hằng nguyên. Tiền tố `0b, 0B ứng với số nhị phân`, `0o, 0O ứng với số bát phân` và `0x, 0X ứng với số hexa`. 

Để dễ đọc, ta có thể dùng dấu gạch dưới _ sau tiền tố để tách các chữ số ra.
```
42  0_600   0o600   0xBadFace   0xBad_Face
```

### Số thực dấu chấm động
Số thực dấu chấm động (floating-point literal) là biểu diễn thập phân (hoặc hexa) của hằng dấu chấm động.
* Số thực dấu chấm động `thập phân` gồm phần số nguyên, phần phân số đều biểu diễn bởi chữ số thập phân, và phần số mũ (`e hoặc E` - ứng với 10^exp) tùy chọn.
* Số thực dấu chấm động `hexa` có tiền tố 0x hoặc 0X, gồm phần số nguyên, phần phân số đều biểu diễn bởi chữ số hexa, và phần số mũ (`p hoặc P` - ứng với 2^p) tùy chọn.
```
72.40   .12345E+5   0.15e+0_2   0x1p-2  0X_1FFFP-16
```

### Số ảo
Số ảo (imaginary literal) biểu diễn phần ảo của một số phức. Nó gồm số nguyên + dấu chấm động kết hợp với "i". 
```
0123i   0xabci  0.i     0x1p-2i 
```

### Rune
Rune biễu diễn cho một hằng rune, là một giá trị nguyên đại diện cho một kí tự Unicode. Một rune được biểu thị bằng một hoặc nhiều kí tự trong cặp dấu nháy đơn, ví dụ như 'x' hay '\n'. Trong cặp dấu nháy đơn, kí tự nào cũng sẽ xuất hiện, ngoại trừ newline và dấu nháy đơn unescaped.

Một số kí tự đặc biệt:
```
\a   U+0007 alert or bell
\b   U+0008 backspace
\f   U+000C form feed
\n   U+000A line feed or newline
\r   U+000D carriage return
\t   U+0009 horizontal tab
\v   U+000b vertical tab
\\   U+005c backslash
\'   U+0027 single quote  (valid escape only within rune literals)
\"   U+0022 double quote  (valid escape only within string literals)
\012 (đúng 3 kí tự oct)
\xab (đúng 2 kí tự hex)
\uabcd (litte u - đúng 4 kí tự hex)
\Uabcdabcd (big u - đúng 8 kí tự hex)
```

### Chuỗi kí tự
Chuỗi kí tự (string constant) biễu diễn hằng chuỗi được cấu thành từ nối một chuỗi các kí tự. Có 2 loại: `raw string literals` và `intepreted string literals`.
* Raw string literals là những chuỗi các kí tự giữa cặp dấu \`\`, ví dụ như \`foo\`. Trong cặp dấu này, bất kì kí tự nào cũng xuất hiện, ngoại trừ chính nó. Những dấu backslashes không hề có ý nghĩa gì cũng như trong raw string có thể có newline.
* Interpreted literals là những chuỗi các kí tự giữa cặp dấu \"\", ví dụ như \"bar\". Trong cặp dấu này, bất kì kí tự nào cũng xuất hiện, ngoại trừ newline và dấu \" unescaped. 
```
`abc`   = "abc'
`\n
\n`     = "\\n\n\\n"
```
Nếu source code có `kí tự với 2 code points` (vd như kết hợp giữa kí tự và dấu), thì có thể xảy ra `lỗi nếu đặt chúng ở trong rune literals`, và sẽ xuất hiện dưới dạng `2 code points nếu được đặt trong string literals`.

## Hằng
Gồm hằng luận lý, rune, số nguyên, số thực dấu chấm động, số phức và chuỗi. Hằng Rune, số nguyên, số thực, số phức được gọi chung là hằng số.
