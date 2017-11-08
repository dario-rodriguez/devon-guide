Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

## Use multi-stage builds
  In Docker, one of the main issues is the size of the final image. It’s not uncommon to end up with images over 1 GB even for simple Java applications. Since version 17.05 of Docker, it’s possible to have multiple builds in a single Dockerfile, and to access the output of the previous build into the current one. Those are called multi-stage builds. The final image will be based on the last build stage.

Let’s imagine the code is hosted on GitHub, and that it’s based on Maven. Build stages would be as follows:

1. *1. Clone the code from GitHub.
2. *Copy the folder from the previous stage; build the app with Maven.
3. *Copy the JAR from the previous stage; run it with java -jar


