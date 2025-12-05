# base

### sort

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
	numbers := []int{5, 3, 7, 1, 4}
	sort.Ints(numbers)
	fmt.Println(numbers) // [1 3 4 5 7]

	words := []string{"banana", "apple", "cherry"}
	sort.Strings(words)
	fmt.Println(words) // [apple banana cherry]

	people := []struct {
		Name string
		Age  int
	}{
		{"Alice", 30},
		{"Bob", 25},
		{"Charlie", 35},
	}

	sort.Slice(people, func(i, j int) bool {
		return people[i].Age < people[j].Age
	})
}
```
