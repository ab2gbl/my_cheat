

- **syntax**
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
  
 

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk5MjIxMjU2NywtMTQ3NTgwNDQ4MiwxNj
EyMzI2MTc4LC0xMjYzNTQxNTldfQ==
-->