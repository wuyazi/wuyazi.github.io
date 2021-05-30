
# Input/Output

## io

## io util

## fmt

## bufio

### bufio.Reader

```language:go
    type Reader struct {
        buf          []byte        // 缓存
        rd           io.Reader    // 底层的io.Reader
        // r:从buf中读走的字节（偏移）；w:buf中填充内容的偏移；
        // w - r 是buf中可被读的长度（缓存数据的大小），也是Buffered()方法的返回值
        r, w         int
        err          error        // 读过程中遇到的错误
        lastByte     int        // 最后一次读到的字节（ReadByte/UnreadByte)
        lastRuneSize int        // 最后一次读到的Rune的大小 (ReadRune/UnreadRune)
    }
```

```
    func NewReader(rd io.Reader) *Reader {
        // 默认缓存大小：defaultBufSize=4096
        return NewReaderSize(rd, defaultBufSize)
    }
```

```
    func NewReaderSize(rd io.Reader, size int) *Reader {
        // 已经是bufio.Reader类型，且缓存大小不小于 size，则直接返回
        b, ok := rd.(*Reader)
        if ok && len(b.buf) >= size {
            return b
        }
        // 缓存大小不会小于 minReadBufferSize （16字节）
        if size < minReadBufferSize {
            size = minReadBufferSize
        }
        // 构造一个bufio.Reader实例
        return &Reader{
            buf:          make([]byte, size),
            rd:           rd,
            lastByte:     -1,
            lastRuneSize: -1,
        }
    }
```

```
    func (b *Reader) ReadSlice(delim byte) (line []byte, err error)
```

```
    reader := bufio.NewReader(strings.NewReader("http://studygolang.com. \nIt is the home of gophers"))
    line, _ := reader.ReadSlice('\n')
    fmt.Printf("the line:%s\n", line)
    // 这里可以换上任意的 bufio 的 Read/Write 操作
    n, _ := reader.ReadSlice('\n')
    fmt.Printf("the line:%s\n", line)
    fmt.Println(string(n))
```

```
    reader := bufio.NewReaderSize(strings.NewReader("http://studygolang.com"),16)
    line, err := reader.ReadSlice('\n')
    fmt.Printf("line:%s\terror:%s\n", line, err)
    line, err = reader.ReadSlice('\n')
    fmt.Printf("line:%s\terror:%s\n", line, err)
```

```
    func (b *Reader) ReadBytes(delim byte) (line []byte, err error)
```

```
    func (b *Reader) ReadString(delim byte) (line string, err error) {
        bytes, err := b.ReadBytes(delim)
        return string(bytes), err
    }
```

```
    func (b *Reader) ReadLine() (line []byte, isPrefix bool, err error)
```
注意，返回值中，要么 line 不是 nil，要么 err 非 nil，两者不会同时非 nil。

```
    func (b *Reader) Peek(n int) ([]byte, error)
```

```
    func (b *Reader) Read(p []byte) (n int, err error)
    func (b *Reader) ReadByte() (c byte, err error)
    func (b *Reader) ReadRune() (r rune, size int, err error)
    func (b *Reader) UnreadByte() error
    func (b *Reader) UnreadRune() error
    func (b *Reader) WriteTo(w io.Writer) (n int64, err error)
```

```
type Scanner struct {
    r            io.Reader // The reader provided by the client.
    split        SplitFunc // The function to split the tokens.
    maxTokenSize int       // Maximum size of a token; modified by tests.
    token        []byte    // Last token returned by split.
    buf          []byte    // Buffer used as argument to split.
    start        int       // First non-processed byte in buf.
    end          int       // End of data in buf.
    err          error     // Sticky error.
}
```

```
    func NewScanner(r io.Reader) *Scanner {
        return &Scanner{
            r:            r,
            split:        ScanLines,
            maxTokenSize: MaxScanTokenSize,
            buf:          make([]byte, 4096), // Plausible starting size; needn't be large.
        }
    }
```

```
    file, err := os.Create("scanner.txt")
    if err != nil {
        panic(err)
    }
    defer file.Close()
    file.WriteString("http://studygolang.com.\nIt is the home of gophers.\nIf you are studying golang, welcome you!")
    // 将文件 offset 设置到文件开头
    file.Seek(0, os.SEEK_SET)
    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
```

```
    type Writer struct {
        err error        // 写过程中遇到的错误
        buf []byte        // 缓存
        n   int            // 当前缓存中的字节数
        wr  io.Writer    // 底层的 io.Writer 对象
    }
```

```
    func NewWriter(wr io.Writer) *Writer {
        // 默认缓存大小：defaultBufSize=4096
        return NewWriterSize(wr, defaultBufSize)
    }
```

```
    func NewWriterSize(wr io.Writer, size int) *Writer {
        // 已经是 bufio.Writer 类型，且缓存大小不小于 size，则直接返回
        b, ok := wr.(*Writer)
        if ok && len(b.buf) >= size {
            return b
        }
        if size <= 0 {
            size = defaultBufSize
        }
        return &Writer{
            buf: make([]byte, size),
            wr:  w,
        }
    }
```

```
    func (b *Writer) Available() int { return len(b.buf) - b.n }

    func (b *Writer) Buffered() int { return b.n }

    func (b *Writer) Flush() error { ... }
```

```
    type ReadWriter struct {
        *Reader
        *Writer
    }
```

# strings



