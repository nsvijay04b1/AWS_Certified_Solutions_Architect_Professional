# AWS CodeBuild:
- A fully managed build service in the cloud that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy.
- Provides prepackaged build environments for popular programming languages.
- A build environment represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build. 
- You can store the source code to be built and the build specification in a CodeCommit repository.
- You can use CodeBuild directly with CodeCommit, or you can incorporate both CodeBuild and CodeCommit in a continuous delivery pipeline with CodePipeline. 
- If there is any build output, the build environment uploads its output to an S3 bucket. 
- CodeBuild is a build as a service product
- It is fully managed, we pay only for the resources consumed during builds
- CodeBuild is an alternative to the solutions provided by third party solutions such as Jenkins
- CodeBuild uses Docker for build environments which can be customized by us
- CodeBuild integrates with other AWS services such as KMS, IAM, VPC, CloudTrails, S3, etc.
- Architecturally CodeBuild gets source material from GitHub, CodeCommit, CodePipeline or even S3
- It builds and tests code. The build can be customized via `buildspec.yml` file which has to be located in the root of the source
- CodeBuild output logs are published to CloudWatch Logs, metrics are also published to CloudWatch Metrics and events to Event Bridge (or CloudWatch Events)
- CodeBuild supports build environments such as Java, Ruby, Python, Node.JS, PHP, .NET, Go and many more

### `buildspec.yml`

- It is used to customize the build process
- It has to be located in root folder of the repository
- It can contain four main phases:
    - `install`: used to install packages in the build environment
    - `pre_build`: sign-in to things or install code dependencies
    - `build`: commands run during the build process
    - `post_build`: used for packaging artifacts, push docker images, explicit notifications
- It can contain environment variables: shell, variables, parameter-store, secret-manager variables
- `Artifacts` part of the file: specifies what stuff to put where