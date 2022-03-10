# CI/CD

- CI/CD architecture:
    ![CI/CD architecture](images/CICD1.png)
- Branching architecture:
    ![Branching architecture](images/CICD2.png)
- Code pipeline:
    ![Code pipeline](images/CICD3.png)
- Each pipeline has stages
- Each pipeline should be linked to a single branch in a repository
    ![Code deployment](images/CICD4.png)
- CodeBuild/CodeDeploy configuration files:
    - `buildspec.yml, appspec.[yml|json]`
    - Reference to these files: [https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html)
    - buildspec is used to influence the way the build process occurs within CodeBuild
    - appspec allows the influence how the deployment process proceeds in CodeDeploy