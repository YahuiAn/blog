![large-file](image/large-file.jpg)

```go
func directlyWrite(name string, data []byte) {
	f, err := os.Create(name)
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	_, err = f.Write(data)
	if err != nil {
		log.Fatal(err)
	}
	// f.WriteString() string转[]byte需要慎用，尤其是数据量大时（每次转化都需要复制内容）
}

func bufioWrite(name string, data []byte) {
	f, err := os.Create(name) // 创建底层文件
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	buf := bufio.NewWriter(f) // 创建缓冲区

	_, err = buf.Write(data) // 向缓冲区写数据
	if err != nil {
		log.Fatal(err)
	}

	if err := buf.Flush(); err != nil { // 保证所有数据都写入磁盘
		log.Fatal(err)
	}
}

func zipWrite(name string, data []byte) {
	zipFile, err := os.Create(name) // 创建zip包
	if err != nil {
		log.Fatal(err)
	}
	defer zipFile.Close()

	zipWriter := zip.NewWriter(zipFile)

	file, err := zipWriter.Create(name) // 在zip包内创建文件
	if err != nil {
		log.Fatal(err)
	}

	_, err = file.Write(data) // 向文件写入数据
	if err != nil {
		log.Fatal(err)
	}

	if err := zipWriter.Flush(); err != nil { // 保证所有数据都写入磁盘
		log.Fatal(err)
	}
}

```

## TODO

并发处理：https://medium.com/swlh/processing-16gb-file-in-seconds-go-lang-3982c235dfa2

