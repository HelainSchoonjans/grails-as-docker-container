# Grails as Docker Container

To build the guide run the following:

    ./gradlew publishGuide

The generated documentation should be at build/docs/index.html.

# Making it work on Windows 10

On windows 10, some issues might arise. Here is the troubleshooting steps.

If the auto-import feature is enabled on your IDE, a wrong import can be imported for the Copy task in build.groovy. Disable this feature and remove the import.

You will need to tick the "Expose daemon on tcp://localhost:2375 wihtout TLS" checkbox in the docker settings.


## Other troubleshooting
If you get the error "driver failed programming external connectivity on endpoint": restart docker.

If you get the error "exec user process caused "no such file or directory": run dos2unix on app-entrypoint.sh.

On 'no main manifest attribute, in /app/application.jar', you need to make the generated war/jar file executable; add these lines to the build.gradle:

    springBoot {
        // Enable the creation of a fully
        // executable archive file.
        executable = true
    }

# How to pass grails environment to the container

Change the entrypoint.sh file to this:

    #!/bin/sh
    
    # environment variable, defaulting to 'production'
    # careful not to pass quotes to docker environment variable!
    environment=${env:-production}
    echo "Launching application in environment '${environment}'";
    java -Djava.security.egd=file:/dev/./urandom -Dgrails.env=${environment} -jar /app/application.jar
    
You will be able to pass the environment like this:

    docker run -p 8080:8080 -e env=staging me/project
    
    
# Other
    
    org.gradle.api.tasks.TaskExecutionException: Execution failed for task ':server:bootRun'.
	at org.gradle.api.internal.tasks.execution.ExecuteActionsTaskExecuter.execute(ExecuteActionsTaskExecuter.java:95)
	at org.gradle.api.internal.tasks.execution.ResolveTaskOutputCachingStateExecuter.execute(ResolveTaskOutputCachingStateExecuter.java:91)
	at org.gradle.api.internal.tasks.execution.ValidatingTaskExecuter.execute(ValidatingTaskExecuter.java:57)
	at org.gradle.api.internal.tasks.execution.SkipEmptySourceFilesTaskExecuter.execute(SkipEmptySourceFilesTaskExecuter.java:119)
	at org.gradle.api.internal.tasks.execution.ResolvePreviousStateExecuter.execute(ResolvePreviousStateExecuter.java:43)
	at org.gradle.api.internal.tasks.execution.CleanupStaleOutputsExecuter.execute(CleanupStaleOutputsExecuter.java:93)
	at org.gradle.api.internal.tasks.execution.FinalizePropertiesTaskExecuter.execute(FinalizePropertiesTaskExecuter.java:45)
	at org.gradle.api.internal.tasks.execution.ResolveTaskArtifactStateTaskExecuter.execute(ResolveTaskArtifactStateTaskExecuter.java:94)
	at org.gradle.api.internal.tasks.execution.SkipTaskWithNoActionsExecuter.execute(SkipTaskWithNoActionsExecuter.java:56)
	at org.gradle.api.internal.tasks.execution.SkipOnlyIfTaskExecuter.execute(SkipOnlyIfTaskExecuter.java:55)
	at org.gradle.api.internal.tasks.execution.CatchExceptionTaskExecuter.execute(CatchExceptionTaskExecuter.java:36)
	at org.gradle.api.internal.tasks.execution.EventFiringTaskExecuter$1.executeTask(EventFiringTaskExecuter.java:67)
	at org.gradle.api.internal.tasks.execution.EventFiringTaskExecuter$1.call(EventFiringTaskExecuter.java:52)
	at org.gradle.api.internal.tasks.execution.EventFiringTaskExecuter$1.call(EventFiringTaskExecuter.java:49)
	at org.gradle.internal.operations.DefaultBuildOperationExecutor$CallableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:315)
	at org.gradle.internal.operations.DefaultBuildOperationExecutor$CallableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:305)
	at org.gradle.internal.operations.DefaultBuildOperationExecutor.execute(DefaultBuildOperationExecutor.java:175)
	at org.gradle.internal.operations.DefaultBuildOperationExecutor.call(DefaultBuildOperationExecutor.java:101)
	at org.gradle.internal.operations.DelegatingBuildOperationExecutor.call(DelegatingBuildOperationExecutor.java:36)
	at org.gradle.api.internal.tasks.execution.EventFiringTaskExecuter.execute(EventFiringTaskExecuter.java:49)
	at org.gradle.execution.plan.LocalTaskNodeExecutor.execute(LocalTaskNodeExecutor.java:43)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionGraph$InvokeNodeExecutorsAction.execute(DefaultTaskExecutionGraph.java:355)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionGraph$InvokeNodeExecutorsAction.execute(DefaultTaskExecutionGraph.java:343)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionGraph$BuildOperationAwareExecutionAction.execute(DefaultTaskExecutionGraph.java:336)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionGraph$BuildOperationAwareExecutionAction.execute(DefaultTaskExecutionGraph.java:322)
	at org.gradle.execution.plan.DefaultPlanExecutor$ExecutorWorker$1.execute(DefaultPlanExecutor.java:134)
	at org.gradle.execution.plan.DefaultPlanExecutor$ExecutorWorker$1.execute(DefaultPlanExecutor.java:129)
	at org.gradle.execution.plan.DefaultPlanExecutor$ExecutorWorker.execute(DefaultPlanExecutor.java:202)
	at org.gradle.execution.plan.DefaultPlanExecutor$ExecutorWorker.executeNextNode(DefaultPlanExecutor.java:193)
	at org.gradle.execution.plan.DefaultPlanExecutor$ExecutorWorker.run(DefaultPlanExecutor.java:129)
	at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:63)
	at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:46)
	at org.gradle.internal.concurrent.ThreadFactoryImpl$ManagedThreadRunnable.run(ThreadFactoryImpl.java:55)
Caused by: org.gradle.process.internal.ExecException: A problem occurred starting process 'command 'C:\Program Files\Java\jdk1.8.0_162\bin\java.exe''
	at org.gradle.process.internal.DefaultExecHandle.execExceptionFor(DefaultExecHandle.java:232)
	at org.gradle.process.internal.DefaultExecHandle.setEndStateInfo(DefaultExecHandle.java:209)
	at org.gradle.process.internal.DefaultExecHandle.failed(DefaultExecHandle.java:356)
	at org.gradle.process.internal.ExecHandleRunner.run(ExecHandleRunner.java:86)
	at org.gradle.internal.operations.CurrentBuildOperationPreservingRunnable.run(CurrentBuildOperationPreservingRunnable.java:42)
	... 3 more
Caused by: net.rubygrapefruit.platform.NativeException: Could not start 'C:\Program Files\Java\jdk1.8.0_162\bin\java.exe'
	at net.rubygrapefruit.platform.internal.DefaultProcessLauncher.start(DefaultProcessLauncher.java:27)
	at net.rubygrapefruit.platform.internal.WindowsProcessLauncher.start(WindowsProcessLauncher.java:22)
	at net.rubygrapefruit.platform.internal.WrapperProcessLauncher.start(WrapperProcessLauncher.java:36)
	at org.gradle.process.internal.ExecHandleRunner.startProcess(ExecHandleRunner.java:97)
	at org.gradle.process.internal.ExecHandleRunner.run(ExecHandleRunner.java:70)
	... 4 more
Caused by: java.io.IOException: Cannot run program "C:\Program Files\Java\jdk1.8.0_162\bin\java.exe" (in directory "C:\Users\hes\IdeaProjects\path\server"): CreateProcess error=206, The filename or extension is too long
	at net.rubygrapefruit.platform.internal.DefaultProcessLauncher.start(DefaultProcessLauncher.java:25)
	... 8 more
Caused by: java.io.IOException: CreateProcess error=206, The filename or extension is too long
	... 9 more
  
== Solution
https://github.com/viswaramamoorthy/gradle-util-plugins
    
# Other

2019-10-11 10:36:46.845 ERROR --- [           main] o.s.boot.SpringApplication               : Application run failed

org.springframework.context.ApplicationContextException: Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:155)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:543)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:140)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:742)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:389)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:311)
	at grails.boot.GrailsApp.run(GrailsApp.groovy:97)
	at grails.boot.GrailsApp.run(GrailsApp.groovy:458)
	at grails.boot.GrailsApp.run(GrailsApp.groovy:445)
	at path.Application.main(Application.groovy:11)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.springframework.boot.loader.MainMethodRunner.run(MainMethodRunner.java:47)
	at org.springframework.boot.loader.Launcher.launch(Launcher.java:86)
	at org.springframework.boot.loader.Launcher.launch(Launcher.java:50)
	at org.springframework.boot.loader.WarLauncher.main(WarLauncher.java:57)
Caused by: org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getWebServerFactory(ServletWebServerApplicationContext.java:202)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:178)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:152)
	... 17 common frames omitted

# Main class name has not been configured and it could not be resolved

For Gradle 5, Grails 4,

springBoot {
    mainClassName = 'my.path.to.Application'
}
    
# On grails 4: Predestroy when running in docker

spring boot tomcat gradle dependency should be in 'compiled', not provided
