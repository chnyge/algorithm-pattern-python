[Github Actions Docs]: https://docs.github.com/en/actions/quickstart

**概述：**

借助Github Actions，实现可定制和可执行的Github仓库自动化开发工作流，可以发现、创建和共享任何的Github Actions(例如CI/CD)，并将这些Actions加入自定义的工作流中。



Github Actions属于事件驱动类型，意思是可以在某个事件之后执行一系列的指令(事后驱动脚本)。

模型： event驱动 -> 任务(工作项) -> step(步骤) -> Action(动作)

- event驱动： 

  [url]: https://docs.github.com/en/actions/reference/events-that-trigger-workflows

- 任务Jobs：

  任务是由一系列执行在相同runner上的step组成的。默认情况下，同个工作流下的多个任务是

并行执行的。用户可以通过配置使得这些任务串行化执行。

- 动作Action：

  工作流的最小组成模块，可以组合进不同的step从而创建一个任务。action的来源可以是自己创建，也可以从github社区获取。

- 执行器runners：

  安装了github actions runner application的服务器（可以是github提供的，也可以是自己搭建的）。



.github/workflows/xx.yml 示例

```yaml
name: learn-github-actions // 可选，工作流名称
on: [push] // 触发事件类型，支持字符串或数组形式或map形式，map形式可以指定tags和分支
jobs: // 所有待执行的job列表
  check-bats-version: // job-1的名称
    runs-on: ubuntu-latest // 执行器名称，ubuntu-latest即该job运行于github提供的ubuntu虚机
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '14'
      - run: npm install -g bats
      - run: bats -v
```



**on字段：**

- 支持字符串/数组/map形式(支持单、多事件类型)

  ```yaml
  on: push
  
  on: [push, pull_request]
  
  on:
    # Trigger the workflow on push or pull request,
    # but only for the main branch
    push:
      branches:
        - main
    pull_request:
      branches:
        - main
    # Also trigger on page_build, as well as release created events
    page_build:
    release:
      types: # This configuration does not affect the page_build event above
        - created
  ```

- on.<event_name>.types 事件可选的活动类型

  - 单独检查 check_run (-created -requested -completed) -> last commit on default branch
  - 检查测试套 check_suite(-created -requested -completed) -> last commit on default branch 
  - 创建分支或tag create() -> last commit on created branch or tag
  - 删除分支或tag delete() ->  last commit on default branch 
  - 部署 deployment() -> commit to be deployed
  - ..

  ```yaml
  # Trigger the workflow on release activity
  on:
    release:
      # Only use the types keyword to narrow down the activity types that will trigger your workflow.
      types: [published, created, edited]
  ```

- on.<push|pull_request>.<branches|tags> 对于push和pull_request两种类型，可以配置匹配/除外的分支和tag规则(匹配和除外规则不能同时写)，也可以使用 ! 作为取反条件

    ```yaml
    on:
      push:
        # Sequence of patterns matched against refs/heads
        branches:    
          # Push events on main branch
          - main
          # Push events to branches matching refs/heads/mona/octocat
          - 'mona/octocat'
          # Push events to branches matching refs/heads/releases/10
          - 'releases/**'
        # Sequence of patterns matched against refs/tags
        tags:        
          - v1             # Push events to v1 tag
          - v1.*           # Push events to v1.0, v1.1, and v1.9 tags
           # Sequence of patterns matched against refs/heads
    
    on:
      push:
        branches:    
          - 'releases/**'
          - '!releases/**-alpha'
    
    on:
      push:
        # Sequence of patterns matched against refs/heads
        branches-ignore:
          # Do not push events to branches matching refs/heads/mona/octocat
          - 'mona/octocat'
          # Do not push events to branches matching refs/heads/releases/beta/3-alpha
          - 'releases/**-alpha'
        # Sequence of patterns matched against refs/tags
        tags-ignore:
          - v1.*           # Do not push events to tags v1.0, v1.1, and v1.9
    ```

- on.<push|pull_request>.paths 对于push和pull_request两种类型，可以配置匹配/除外的文件规则(匹配和除外规则不能同时写)，也可以使用 ! 作为取反条件

- on.schedule

  ```yaml
  on:
    schedule:
      # * is a special character in YAML so you have to quote this string
      - cron:  '30 5,17 * * *'
  ```
