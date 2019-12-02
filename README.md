### Miniclip DevOps Test Task

- Answers to the [First Scenario](./answers.md)

- Steps to the Second Scenario:

  - Requirements to create this stack:
    - awscli

  - Run the command below to create the stack:

```
aws --region eu-west-1 cloudformation create-stack --stack-name MiniClip --template-body file://miniclip.yml
```

