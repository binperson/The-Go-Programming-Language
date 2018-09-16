### string

###rune相当于go的char

 - 使用range遍历pos，rune对
 - 使用utf8.RuneCountInSring获取字符数量
 - 使用len获取字节长度
 - 使用[]byte获取字节

 ```go
 func main() {
	s := "Yes我爱你!" // UTF-8
	fmt.Print(len(s)) // 13
	fmt.Println(s) // Yes我爱你!

	for _, b := range []byte(s) {
		fmt.Printf("%X ", b) // 59 65 73 E6 88 91 E7 88 B1 E4 BD A0 21
	}
	fmt.Println()

	for i, ch := range s { // ch is a rune
		fmt.Printf("(%d %X) ", i, ch) //(0 59) (1 65) (2 73) (3 6211) (6 7231) (9 4F60) (12 21)
	}
	fmt.Println()

	fmt.Println("Rune count:",
		utf8.RuneCountInString(s)) // 7

	bytes := []byte(s)
	for len(bytes) > 0 {
		ch, size := utf8.DecodeRune(bytes)
		fmt.Println(ch)
		fmt.Println(size)
		bytes = bytes[size:]
		fmt.Println(bytes)
		fmt.Printf("%c ", ch)
		fmt.Println()
	}
	/*
89
1
[101 115 230 136 145 231 136 177 228 189 160 33]
Y
101
1
[115 230 136 145 231 136 177 228 189 160 33]
e
115
1
[230 136 145 231 136 177 228 189 160 33]
s
25105
3
[231 136 177 228 189 160 33]
我
29233
3
[228 189 160 33]
爱
20320
3
[33]
你
33
1
[]
!

	*/
	fmt.Println()

	for i, ch := range []rune(s) {
		fmt.Printf("(%d %c) ", i, ch) // (0 Y) (1 e) (2 s) (3 我) (4 爱) (5 你) (6 !)
	}
	fmt.Println()
}
```

### 其他字符串操作

 - Filds, Split, Join
 - Contains, Index
 - ToLower, ToUpper
 - Trim, TrimRight, TrimLeft  