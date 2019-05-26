## Requirements
  " Set cwd to top level git dir
  : exe 'lcd ' . system('git rev-parse --show-toplevel') | vsplit

## References
  docs/common-commands.md
  docs/mac-setup.md
  : tabe docs/doc-dev.md

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

  $ docker run -v $(pwd):/mnt/app -it kfcron:$(git rev-parse --short HEAD) /bin/bash

## Documentation Submodule
### Setup
  $ git submodule add https://github.com/larioj/kfcron-docs.git
