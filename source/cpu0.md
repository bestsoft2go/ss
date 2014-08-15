# CPU0 處理器

商用的處理器通常因為強調效能的原因，其設計都會讓指令格式變得難懂且複雜。

為了讓處理器變得更簡單，更容易理解，我們設計了 CPU0 處理器，這個處理計的設計原則是 KISS，也就是 Keep It Simple and Stupid。

CPU0 的設計不求速度，只求清楚易懂，因此在指令級與指令格式上都盡量簡單，編碼都以 4 位元為單位，因此也很容易可以用人腦將組合語言翻成機器碼。

以下是一個 CPU0 的組合語言範例，該程式所作的事情是計算 1+2+...+10 的結果，最後應該會得到總和為 55。

檔案：sum.as0

```
        LD     R1, sum      ; R1 = sum = 0
        LD     R2, i        ; R2 = i = 1
        LDI    R3, 10       ; R3 = 10
FOR:    CMP    R2, R3       ; if (R2 > R3)
        JGT    EXIT         ;   goto EXIT
        ADD    R1, R1, R2   ; R1 = R1 + R2 (sum = sum + i)
        ADDI   R2, R2, 1    ; R2 = R2 + 1  ( i  = i + 1)
        JMP    FOR          ; goto FOR
EXIT:   ST     R1, sum      ; sum = R1
        ST     R2, i        ; i = R2
        LD     R9, msgptr   ; R9= pointer(msg) = &msg
        SWI    3            ; SWI 3 : 印出 R9 (=&msg) 中的字串
        MOV    R9, R1       ; R9 = R1 = sum
        SWI    4            ; SWI 4 : 印出 R9 (=R1=sum) 中的整數
        RET                 ; return 返回上一層呼叫函數
i:      RESW   1            ; int i
sum:    WORD   0            ; int sum=0
msg:    BYTE   "1+...+10=", 0   ; char *msg = "sum="
msgptr: WORD   msg          ; char &msgptr = &msg
```
