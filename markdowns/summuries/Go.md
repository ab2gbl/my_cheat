

- **syntax**
-  create module `go mod init example.com/module` 
- use local storage `go mod edit -replace example.com/greetings=..\greetings\`
- sync packages `go mod tidy `
```go
package main
import "fmt"
func main() {
	// var declaration
	var test int
	test = 1000
	// short var declaration
	test2 := 1000
	// print 
	fmt.Println("Your SMS sending limit is", test2 )
	// formating
	msg := fmt.Sprintf("Hi %s, your open rate is %.1f percent\n",name,openRate)
	// if
	if height > 6 {
	    fmt.Println("You are super tall!")
	} else if height > 4 {
	    fmt.Println("You are tall enough!")
	} else {
	    fmt.Println("You are not tall enough!")
	}
	// if with init condition
	if length := getLength(email); length < 10 {
	    fmt.Printf("Email must be at least 10 characters, is %d\n", length)
	}
	// switch
	switch os {
	    case "linux":
	        creator = "Linus Torvalds"
	    default:
	        creator = "Unknown"
	}
	// func
	func sub(x int, y int) int {
	    return x-y
	}
	// slices pic example
	func Pic(dx, dy int) [][]uint8 {
		result := make([][]uint8, dy)

		for y := range result {
			result[y] = make([]uint8, dx)
			for x := range result[y] {
				result[y][x] = uint8(x*y)
			}
		}

		return result
	}
  
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE3MTI1NTc1NywtMzExNzAwOTM5LC0xNz
A5MjMxMTgwLDE5OTIyMTI1NjcsLTE0NzU4MDQ0ODIsMTYxMjMy
NjE3OCwtMTI2MzU0MTU5XX0=
-->