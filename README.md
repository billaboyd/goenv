## Credit
https://github.com/caarlos0/env

## Example
A very basic example (check the examples folder):

```go
package main

import (
	"fmt"
	"time"

	"github.com/billaboyd/goenv"
)

type config struct {
	Home         string        `env:"HOME"`
	Port         int           `env:"PORT" envDefault:"3000"`
	IsProduction bool          `env:"PRODUCTION"`
	Hosts        []string      `env:"HOSTS" envSeparator:":"`
	Duration     time.Duration `env:"DURATION"`
	TempFolder   string        `env:"TEMP_FOLDER" envDefault:"${HOME}/tmp" envExpand:"true"`
}

func main() {
	cfg := config{}
	err := env.Parse(&cfg)
	if err != nil {
		fmt.Printf("%+v\n", err)
	}
	fmt.Printf("%+v\n", cfg)
}
```

```sh
You can run it like this:

$ PRODUCTION=true HOSTS="host1:host2:host3" DURATION=1s go run examples/first.go
{Home:/your/home Port:3000 IsProduction:true Hosts:[host1 host2 host3] Duration:1s}
```

Required fields
The env tag option required (e.g., env:"tagKey,required") can be added to ensure that some environment variable is set. In the example above, an error is returned if the config struct is changed to:

```go
type config struct {
    Home         string   `env:"HOME"`
    Port         int      `env:"PORT" envDefault:"3000"`
    IsProduction bool     `env:"PRODUCTION"`
    Hosts        []string `env:"HOSTS" envSeparator:":"`
    SecretKey    string   `env:"SECRET_KEY,required"`
}
```