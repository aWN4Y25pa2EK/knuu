## Getting Started

This section will guide you on how to set up and run **knuu**.

### Prerequisites

1. **Docker**: Knuu requires Docker to run
   > You can install Docker by following the instructions [here](https://docs.docker.com/get-docker/).

2. **Kubernetes cluster**: Set up access to a Kubernetes cluster using a kubeconfig.
   > In case you have no Kubernets cluster running yet, you can get more information [here](https://kubernetes.io/docs/setup/).

3. **'test' Namespace**: Create a namespace called 'test' in your Kubernetes cluster.
   > **Note:** The used namespace can be changed by setting the `KNUU_NAMESPACE` environment variable.

### Writing Tests

The documentation you can find  [here](https://pkg.go.dev/github.com/celestiaorg/knuu).

Simple example:

1. Add the following to your `go.mod` file:

    ```go
    require (
        github.com/celestiaorg/knuu v0.8.2
        github.com/stretchr/testify v1.8.4
    )
    ```

2. Run `go mod tidy` to download the dependencies.

3. Create a file called `main_test.go` with the following content to initialize knuu:

```go
package main

import (
    "fmt"
    "os"
    "testing"
    "time"

    "github.com/celestiaorg/knuu/pkg/knuu"
)

func TestMain(m *testing.M) {
    err := knuu.Initialize()
    if err != nil {
        log.Fatalf("Error initializing knuu: %v:", err)
    }
    exitVal := m.Run()
    os.Exit(exitVal)
}
```

4. Create a file called `example_test.go` with the following content:

```go
package main

import (
    "os"
    "testing"

    "github.com/celestiaorg/knuu/pkg/knuu"
    "github.com/stretchr/testify/assert"
)

func TestBasic(t *testing.T) {
    t.Parallel()
    // Setup

    instance, err := knuu.NewInstance("alpine")
    if err != nil {
        t.Fatalf("Error creating instance '%v':", err)
    }
    err = instance.SetImage("docker.io/alpine:latest")
    if err != nil {
        t.Fatalf("Error setting image: %v", err)
    }
    err = instance.SetCommand("sleep", "infinity")
    if err != nil {
        t.Fatalf("Error setting command: %v", err)
    }
    err = instance.Commit()
    if err != nil {
        t.Fatalf("Error committing instance: %v", err)
    }

    t.Cleanup(func() {
        // Cleanup
        if os.Getenv("KNUU_SKIP_CLEANUP") == "true" {
            t.Log("Skipping cleanup")
            return
        }

        err = instance.Destroy()
        if err != nil {
            t.Fatalf("Error destroying instance: %v", err)
        }
    })

    // Test logic

    err = instance.Start()
    if err != nil {
        t.Fatalf("Error starting instance: %v", err)
    }
    err = instance.WaitInstanceIsRunning()
    if err != nil {
        t.Fatalf("Error waiting for instance to be running: %v", err)
    }
    wget, err := instance.ExecuteCommand("echo", "Hello World!")
    if err != nil {
        t.Fatalf("Error executing command '%v':", err)
    }

    assert.Equal(t, wget, "Hello World!\n")
}
```

You can find more examples in the following repositories:

- [celestiaorg/knuu-example](https://github.com/celestiaorg/knuu-example)
- [celestiaorg/celestia-app](https://github.com/celestiaorg/celestia-app/tree/main/test/e2e)

### Running Tests

You can use the built-in `go test` command to run the tests.

To run all tests in the current directory, you can run:

```shell
go test -v ./...
```