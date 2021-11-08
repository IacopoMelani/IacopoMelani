```yaml
# me.yaml
name: Iacopo
from: Italy
based: Somewhere in the blockchain
role: Full Stack Developer
nick: Santiago
languages:
  - lang: Go
    affinity: 90
  - lang: PHP
    affinity: 80
  - lang: JS/TS
    affinity: 75
  - lang: CSS/HTML
    affinity: 70
  - lang: Dart
    affinity: WIP
databases:
  - MariaDB
  - MySQL
  - SQLServer
  - SQLite
  - Redis
miscs:
  - Docker
  - Kubernetes
  - Linux
hobbies:
  - Videogames
  - Good drinks
  - Coding
  - Repeat
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"

	"gopkg.in/yaml.v2"
)

type Person struct {
	Name      string   `json:"name"      yaml:"name"`
	From      string   `json:"from"      yaml:"from"`
	Based     string   `json:"based"     yaml:"based"`
	Role      string   `json:"role"      yaml:"role"`
	Nick      string   `json:"nick"      yaml:"nick"`
	Langs     []Lang   `json:"languages" yaml:"languages"`
	Databases []string `json:"databases" yaml:"databases"`
	Miscs     []string `json:"miscs"     yaml:"miscs"`
	Hobbies   []string `json:"hobbies"   yaml:"hobbies"`
}

type Lang struct {
	Name     string `json:"lang"     yaml:"lang"`
	Affinity int    `json:"affinity" yaml:"affinity"`
}

func main() {

	var me Person

	yamlFile, err := ioutil.ReadFile("me.yaml")
	if err != nil {
		panic(err)
	}

	if err := yaml.Unmarshal(yamlFile, &me); err != nil {
		panic(err)
	}

	http.HandleFunc("/me", func(rw http.ResponseWriter, r *http.Request) {
		rw.Header().Set("Content-Type", "application/json")
		rw.WriteHeader(http.StatusOK)

		contentJson, err := json.Marshal(me)
		if err != nil {
			log.Fatal(err)
		}

		rw.Write([]byte(contentJson))
	})

	fmt.Println("Listening on :8080")

	log.Fatal(http.ListenAndServe(":8080", nil))
}
```
