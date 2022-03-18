# gofuzzgen [![Go Reference](https://pkg.go.dev/badge/github.com/kimuson13/gofuzzgen.svg)](https://pkg.go.dev/github.com/kimuson13/gofuzzgen)
gofuzzgen is generate template of fuzzing test code
## Caution
gofuzzgen is intended to be used for Go1.18 or higher
## Install
```
$ go install github.com/kimuson13/gofuzzgen/cmd/gofuzzgen@latest
```
## Situation to use
When you want to generate a template code of go standard fuzzing test, `gofuzzgen` help that.
## How to use
If you want to generate fuzzing test code with a package, give the package path of interest as the first
argument:
```
$ gofuzzgen github.com/kimuson13/gofuzzgen
```
To generate fuzzing test code with all packages beneath the current directory:
```
$ gofuzzgen ./...
```
### Options
- -o
`-o` can select ouput file name.  
If you type into "hoge" and go package name is "sample", output file name will be hoge_sampel_fuzz_test.go
- -f
`-f` can select only one function.
## Demo
If you are on a directoty like that
```
$ tree .
sample
├── cmd
│   └── sample
│       └── main.go
├── go.mod
├── go.sum
└── sample.go
```
```
$ cat sample.go
package sample

func CanFuzzFunc(a int, b int) {
    return a * b
}
```
If you run `gofuzzgen` with no options, result report to stdout.
```
$ gofuzzgen ./...
// This file is generated by gofuzzgen.
// Only generate fuzzing template.
package sample_test

import "testing"

func FuzzCanFuzzFunc(f *testing.F) {
    testcases := []struct{
        // Add seed corpus here
        arg0 int
        arg1 int
    }

    for _, tc := range testcases {
        f.Add(tc.arg0, tc.arg1)
    }

    f.Fuzz(func(t *testing.T, orig0 int, orig1 int) {
        // implemnt fuzzing test code
    })

}
```
If you run `gofuzzgen` with option `-o`, generate test file.
```
$ gofuzzgen -o=hoge ./...
$ cat hoge_sample_fuzz_test.go
// This file is generated by gofuzzgen.
// Only generate fuzzing template.
package sample_test

import "testing"

func FuzzCanFuzzFunc(f *testing.F) {
    testcases := []int{
        arg0 int
        arg1 int
    }{
        // Add seed corpus here
    }

    for _, tc := range testcases {
        f.Add(tc.arg0, tc.arg1)
    }

    f.Fuzz(func(t *testing.T, orig0 int, orig1 int) {
        // implemnt fuzzing test code
    })
}
```
## Future Outlook
### Overwrite files
Now, `gofuzzgen` can't over write file that already exist.
### No duplicated tests
Now, `gofuzzgen` generate all functions that can fuzz test whether fuzz test exist or not.
## License
The source code is licensed MIT. The website content is licensed CC BY 4.0,see LICENSE.
