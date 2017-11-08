Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

### Use multi-stage builds

In Docker, one of the main issues is the size of the final image. It’s not uncommon to end up with images over 1 GB even for simple Java applications. Since version 17.05 of Docker, it’s possible to have multiple builds in a single Dockerfile, and to access the output of the previous build into the current one. Those are called multi-stage builds. The final image will be based on the last build stage.


