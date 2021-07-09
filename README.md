# Golang Notes

## Main
Go is a call by value language. Every time you pass a parameter to a function, Go makes a copy of the value that’s passed in. Passing a slice to the append function actually passes a copy of the slice to the function.

## Run and install
```shell
go run main.go //will run the code. The executable file will be deleted after exit
go run —work main.go //will show the executable file
go build main.go //will compile the app
go build -o hello_world hello.go //will an put executable file like hello.go
```

## Code quality
```shell
go fmt ./
golint ./...
go vet ./...
```
Or we can use [golangci-lint](https://golangci-lint.run/usage/install/)
```shell
golangci-lint run
```
Check for shadowing
```shell
go install golang.org/x/tools/go/analysis/passes/shadow/cmd/shadow@latest
shadow ./...
```

## Variables
```golang
var power int
power = 900 
```
```golang
var power int = 900
```
```golang
power := 900
```
```golang
name, power := “Vladimir”, 900
```

## Structures
Structures don’t have constructors.
Instead you create a function, that returns an instance of desired type (like a factory). 

```golang
func NewSaiyan(name string, power int) *Saiyan {
	return &Saiyan{
		Name: name,
		Power: power,
	}
}
```

The result of `new(X)` is the same as `&X{}`. It allocates required memory and returns pointer.

```golang
goku := new(Saiyan)
// same as
goku := &Saiyan{}
```

```golang
type person struct {
	name string
	age  int
	pet  string
}

var fred person
bob := person{}
```
```golang
julia := person{
	"Julia",
	40,
	"cat",
}

beth := person{
	age:  30,
	name: "Beth",
}
```

### Composition
```golang
type Person struct {
	Name string
}
func (p *Person) Introduce() {
	fmt.Printf("Hi, I'm %s\n", p.Name)
}
type Saiyan struct {
	*Person
	Power int
}

// and to use it:
goku := &Saiyan{
	Person: &Person{"Goku"},
	Power: 9001,
}
goku.Introduce()
```

The `Saiyan` structure has a field of type `*Person`. Because we didn’t give it an explicit field name, we can implicitly
access the fields and functions of the composed type. However, the Go compiler did give it a field name, consider the
perfectly valid
```golang
goku := &Saiyan{
	Person: &Person{"Goku"},
}
fmt.Println(goku.Name)
fmt.Println(goku.Person.Name)
```

## Arrays
```golang
var scores [10]int
scores[0] = 339
```
```golang
scores := [4]int{9001, 9333, 212, 33}
```
```golang
for index, value := range scores {

}
```

## Slices
A slice isn’t comparable. It is a compile-time error to use == to see if two slices are identical or != to see if they are different. The only thing you can compare a slice with is nil.
```golang
var x []int
```
```golang
scores := []int{1,4,293,4,9}
```
```golang
scores := make([]int, 10)
```
We use `make` instead of new because there’s more to creating a slice than just allocating the memory (which is what new does).

```golang
names := []string{"leto", "jessica", "paul"}
checks := make([]bool, 10)
var names []string
scores := make([]int, 0, 20)
```
```golang
var x = []int{1,2,3}
x = append(x, 4)
x = append(x,5,6,7)
```
Add one slice is appended onto another by using the … operator to expand the source slice into individual values
```golang
var x = []int{1,2,3}
y := []int{20,30,40}
x = append(x, y...)
```

## Maps
Maps are not comparable. You can check if they are equal to nil, but you cannot check if two maps have identical keys and values using == or differ using !=
```golang
var nilMap map[string]int
```
```golang
totalWins := map[string]int{}
```
```golang
teams := map[string][]string {
	"Orcas": []string{"Fred", "Ralph", "Bijou"},
	"Lions": []string{"Sarah", "Peter", "Billie"},
	"Kittens": []string{"Waldo", "Raul", "Ze"},
}
```
```golang
m := map[string]int{
	"hello": 5,
	"world": 0,
}
v, ok := m["hello"]
fmt.Println(v, ok)
```
```golang
func main() {
	lookup := make(map[string]int)
	lookup["goku"] = 9001
	power, exists := lookup["vegeta"]
	// prints 0, false
	// 0 is the default value for an integer
	fmt.Println(power, exists)
	// returns 1
	total := len(lookup)
	// has no return, can be called on a nonexisting key
	delete(lookup, "goku")
}
```

```golang
type Saiyan struct {
	Name string
	Friends map[string]*Saiyan
}
goku := &Saiyan{
	Name: "Goku",
	Friends: make(map[string]*Saiyan),
}
goku.Friends["krillin"] = ... //todo load or create Krillin
```
```golang
lookup := map[string]int{
	"goku": 9001,
	"gohan": 2044,
}
```
```golang
for key, value := range lookup {
...
}
```
```golang
commits := map[string]int{
    "rsc": 3711,
    "r":   2138,
    "gri": 1908,
    "adg": 912,
}
```
The delete function takes a map and a key and then removes the key-value pair with the specified key. If the key isn’t present in the map or if the map is nil, nothing happens. The delete function doesn’t return a value.
```golang
m := map[string]int{
	"hello": 5,
	"world": 10,
}
delete(m, "hello")
```

## Sorting
Sort int slice desc
```golang
sort.Sort(sort.Reverse(sort.IntSlice(are)))
```

## MaxInt MinInt
```golang
const MaxInt = int(^uint(0) >> 1)
const MinInt = -MaxInt - 1
```