# Golang Notes

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

## Maps
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