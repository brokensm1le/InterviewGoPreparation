# Strings 

### Rune

Чтобы правильно работать с юникодом в Go, нужно понять, что такое руна (rune). Это псевдоним для int32, и он представляет
кодовую точку Unicode — то есть конкретный символ в юникод.

Разница между руной и байтом — огромная. Один символ может занимать от одного до четырех байтов. Поэтому, если работать 
с текстом, содержащим неанглоязычные символы, игнорировать это нельзя.

```go
package main
import (
	"fmt"
)

func main() {
	s := "qwerty"
	for i, r := range s {
	    // r - rune 
		fmt.Printf("position %d: %c\n", i, r)
	}
	
	// cast string to slice runes
	sRune := []rune(s)
	for i := 0; i < len(sRune); i++ {
		fmt.Printf("position %d: %c\n", i, sRune[i])
	}
	// cast slice runes to string
	s = string(sRune)
}
```

### package "strings"

```go
package main
import (
	"fmt"
	"strings"
)

func main() {
	result := strings.Split("h-e-l-l-o", "-")
	fmt.Println(result) // [h e l l o]

	s := strings.Contains("world", "rl")
	fmt.Println(s) // true

	s = strings.Contains("world", "rrl")
	fmt.Println(result) // false
	
	str := "Hello World. Good bye World"
	fmt.Println("Count of `World`:", strings.Count(str, "World"))  // 2

	str = "Hello World, Good bye World"
	fmt.Println(strings.ReplaceAll(str, "World", "work"))  // Hello work, Good bye work
}

```

### int to string and reverse

```go

package main
import (
	"fmt"
	"strconv"
)

func main() {
	age := 24
	fmt.Println("i am " +  strconv.Itoa(age) + " years old")     // i am 24 years old
	fmt.Println("i am " +  strconv.Itoa(24) + " years old")      // i am 24 years old
	fmt.Println("i am " +  fmt.Sprintf("%d", 24) + " years old") // i am 24 years old

	ageStr := "24"
	res, err := strconv.Atoi(ageStr)
	if err != nil {
		return 
	}
	fmt.Println(res)
}

```

### Upper & lower

```go
package main
import (
    "fmt"
    "strings"
)
 
func main() {
     
    str := "Hello World"
    
	// use package Strings
    fmt.Println(strings.ToUpper(str))     // HELLO WORLD
    fmt.Println(strings.ToLower(str))     // hello world


    // use logic
	builder := strings.Builder{}

	diff := 'A' - 'a'

	for _, val := range str {
		if 'A' <= val && val <= 'Z' {
			val = val - diff
		}
		builder.WriteRune(val)
	}

	fmt.Println(builder.String()) // hello world
}

```