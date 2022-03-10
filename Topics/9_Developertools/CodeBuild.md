# AWS CodeBuild:
- A fully managed build service in the cloud that compiles your source code, runs unit tests, and produces artifacts that are ready to deploy.
- Provides prepackaged build environments for popular programming languages.
- A build environment represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build. 
- You can store the source code to be built and the build specification in a CodeCommit repository.
- You can use CodeBuild directly with CodeCommit, or you can incorporate both CodeBuild and CodeCommit in a continuous delivery pipeline with CodePipeline. 
- If there is any build output, the build environment uploads its output to an S3 bucket. 