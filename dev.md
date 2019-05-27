## Requirements
    " Set cwd to top level git dir
    : exe 'lcd ' . system('git rev-parse --show-toplevel') | vsplit
    : vsplit

## References
    docs/common-commands.md
    : tabe docs/doc-dev.md
    : tabe docs/k8s-dev.md

## Main
- app/Main.hs

## Sources
    ./src
    ├── KanbanFlow
    │   ├── Board.hs
    │   ├── Color.hs
    │   ├── Column.hs
    │   ├── Name.hs
    │   ├── Task.hs
    │   ├── TaskId.hs
    │   ├── Token.hs
    │   ├── Url.hs
    │   ├── Util.hs
    │   └── Uuid.hs
    ├── KfCron
    │   ├── Blueprint.hs
    │   ├── Board.hs
    │   ├── Config.hs
    │   ├── CronSchedule.hs
    │   └── Task.hs
    └── Util.hs

## Configuration
### Files
    .
    ├── ChangeLog.md
    ├── Setup.hs
    ├── package.yaml
    └── stack.yaml

## Docker
### Files
- Dockerfile

### Build/Run
    $ docker build -t kfcron:$(git rev-parse --short HEAD) .
    $ docker run -it kfcron:$(git rev-parse --short HEAD) /bin/bash

### Format Code
    $ docker run -v $(pwd):/app kfcron:$(git rev-parse --short HEAD) \
        /bin/bash -c "find /app -name '*.hs' | xargs stylish-haskell -i"

### Locally Test Binary
    $ docker run \
        -v $(pwd):/mnt/app \
        -v $HOME/.config/kfcron:/mnt/kfcron-token \
        -v $(pwd)/../kfcron-work-schedule:/mnt/kfcron-schedule \
        -it kfcron:$(git rev-parse --short HEAD) /bin/bash

### Base Docker Container
    $ docker pull haskell:8.3
    $ docker run -it haskell:8.6.5 /bin/bash
    $ docker ps -q
    $ docker kill $(docker ps -q)

## Documentation Submodule
### Setup
    $ git submodule add https://github.com/larioj/kfcron-docs.git
