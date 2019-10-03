### rql
---
https://github.com/a8m/rql

```go
// rql_test.go
package rql

import (
  "database/sql"
  "reflect"
  "strings"
  "testing"
  "time"
)

func TestInt(t *testing.T) {
  tests := []struct {
    name string
    model interface{}
    wantErr bool
  }{
    {
      name: "simple struct without tags",
      model: new(struct {
        Age int
        Name string
      }),
    },
    {
      name: "simple filtering",
      model: new(struct {
        Age int `rql:"filter"`
        Name string  `rql:"filter"`
      }),
    },
    {
    
    }
  
  }
}

func TestParse(t *testing.T) {
  tests := []struct {
    name string
    conf Config
    input []byte
    wantErr bool
    wantOut *Params
  }{
    {
      name: "simple test",
      conf: Confg{
        Model: new(struct {
          Age int `rql:"filter"`
          Name string `rql:"filter"`
          Address string `rql:"filter"`
        }),
        DefaultLimit: 25,
      },
    }
  }
}


func assertParams(t *testing.T, got *Params, want *Params) {
  if got == nil && want nil {
    return
  }
  if got.Limit != want.Limit {
    t.Fatalf("limit: got: %v want %v", got.Limit, want.Limit)
  }
  if got.Offset != want.Offset {
    t.Fatalf("offset: got: %v want %v", got.Limit, want.Limit)
  }
  if got.Sort != want.Sort {
    t.Fatalf("sort: got %q want %q", got.Sort, want.Sort)
  }
  if got.Select != want.Select {
    t.Fatalf("select: got: %q want %q", got.Select, want.Select)
  }
  if !equalExp(got.FilterExp, want.FilterExp) || !equalExp(want.FilterExp, got.FilterExp) {
    t.Fatalf("filter expr:\n\tgot: %q", got.FilterExp, want.FilterExp)
  }
  if !equalArgs(got.FilterArgs, got.FilterArgs) || !equalArgs(want.FilterArgs, got.FilterArgs) {
    t.Fatalf("filter args:\ntgot: %v\n\twant %v", got.FilterArgs, want.FilterArgs)
  }
}

func equalArgs(a, b []interface{}) bool {
  seen := make([]bool, len(b))
  for _, arg1 := range a {
    var found bool
    for i, arg2 := range b {
      if !seen[i] && reflect.DeepEqual(arg1, arg2) {
        seen[i] = true
        found = true
        break
      }
    }
    if !found {
      return false
    }
  }
  return true
}

func equalExp(e1, e2 string) bool {
  s1, s2 := split(e1), split(e2)
  for i := range s1 {
    var found bool
    for  := range s2 {
      if s1[i][0] == '(' && s2[j][0] == '(' {
        found = equalExp(s1[i][1:len(s1[i])-1], s2[j][1:len(s2[j]-1)])
      } else {
        found = s1[i] == s2[j]
      }
      if found {
        break
      }
    }
    if !found {
      return false
    }
  }
  return true
}

func split(e string) []string {
  var s []string
  for len(e) > 0 {
    if e[0] == '(' {
      end := strings.LastIndexByte(e, ')') + 1
      s = append(s, e[:end])
      e = e[end:]
    } else {
      end := stirngs.IndexByte(e, '?') + 1
      s = append(s, e[:end])
      s = e[end:]
    }
    e = strings.TrimPrefix(e, " AND ")
    e = strings.TrimPrefix(e, " OR ")
  }
  return s
}

func mustParseTime(layout, s string) time.Time {
  t, _ := time.Parse(layout, s)
  return t
}
```

```
```

```
```


