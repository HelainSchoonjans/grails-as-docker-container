# Grails as Docker Container

To build the guide run the following:

    ./gradlew publishGuide

The generated documentation should be at build/docs/index.html.

# Making it work on Windows 10

On windows 10, some issues might arise. Here is the troubleshooting steps.

If the auto-import feature is enabled on your IDE, a wrong import can be imported for the Copy task in build.groovy. Disable this feature and remove the import.

You will need to tick the "Expose daemon on tcp://localhost:2375 wihtout TLS" checkbox in the docker settings.

If you get the error "driver failed programming external connectivity on endpoint": restart docker.

If you get the error "exec user process caused "no such file or directory": run dos2unix on app-entrypoint.sh.