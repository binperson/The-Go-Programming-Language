### Map

 - 创建：make(map[string]int)
 - 获取元素：m[key]
 - key不存在时，获取value类型的初始值
 - 用value, ok:=m[key]来判断是否存在key
 - 用delete删除一个key
 - 使用range遍历key，或者遍历key，value对
 - 不保证遍历顺序，如需顺序，需手动对key排序
 - 使用len获取元素个数

 ### map的key

 - map使用哈希表，必须可以比较相等
 - 除了slice，map，function的内建类型都可以作为key
 - Struct类型不包含上述字段，也可作为key 

 ```go
    m := map[string]string{
		"name":    "ccmouse",
		"course":  "golang",
		"site":    "imooc",
		"quality": "notbad",
	}

	m2 := make(map[string]int) // m2 == empty map

	var m3 map[string]int // m3 == nil

	fmt.Println("m, m2, m3:")
	fmt.Println(m, m2, m3) // map[name:ccmouse course:golang site:imooc quality:notbad] map[] map[]

	fmt.Println("Traversing map m")
	for k, v := range m {
		fmt.Println(k, v)
	}
	/*
	course golang
	site imooc
	quality notbad
	name ccmouse
	*/
	fmt.Println("Getting values")
	courseName := m["course"]
	fmt.Println(`m["course"] =`, courseName) // m["course"] = golang
	if causeName, ok := m["cause"]; ok {
		fmt.Println(causeName)
	} else {
		fmt.Println("key 'cause' does not exist")
	}

	fmt.Println("Deleting values")
	name, ok := m["name"]
	fmt.Printf("m[%q] before delete: %q, %v\n",
		"name", name, ok) // m["name"] before delete: "ccmouse", true

	delete(m, "name")
	name, ok = m["name"]
	fmt.Printf("m[%q] after delete: %q, %v\n",
		"name", name, ok) // m["name"] after delete: "", false
 ```