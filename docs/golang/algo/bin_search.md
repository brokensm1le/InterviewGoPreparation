# BinarySearch

### default

```go
package main

import (
    "fmt"
)

func bs(num []int, d int) int {
	l := 0
	r := len(num)
		
	for r-l > 1 {
		m := (r + l) / 2
		if num[m] <= d {
			r = m
		} else {
			l = m
        }
    }
	
	return l
}

func main() {
	numbers := []int{1,2,3,5,6,7}
	fmt.Println(bs(numbers, 4)) // 2
}
```


