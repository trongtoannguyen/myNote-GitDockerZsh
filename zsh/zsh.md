# Zsh command

## Open Finder window from current Terminal location

``` code
open .
```

## Standard glob pattern

Ví dụ về cách sử dụng "Standard glob patterns" trong Linux-based:

- *.jpg: Tất cả các tệp có phần mở rộng .jpg.

    ```bash
    mv *.jpg images/
    ```

- file?.txt: Các tệp có tên fileX.txt (với X là một ký tự bất kỳ).

    ```bash
    rm fileX.txt
    ```

- [abc]*.txt: Các tệp bắt đầu bằng 'a', 'b' hoặc 'c' và có phần mở rộng .txt.

    ```bash
    cp [abc]*.txt
    ```

- file[1-3].txt: Các tệp có tên file1.txt, file2.txt, hoặc file3.txt.

    ```bash
    ls *.txt
    ```

- file{a,b}.txt: Các tệp có tên filea.txt hoặc fileb.txt.

    ```bash
    rm file{a,b}.txt
    ```
