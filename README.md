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
