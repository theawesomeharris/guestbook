# This line specifies the base image for the first stage of the build. 
# Here, we’re using the official Go image (version 1.18) and naming this build stage "builder."
FROM golang:1.18 as builder

# This sets the working directory inside the container to /app. 
# Any subsequent commands will be executed in this directory.
WORKDIR /app

# This copies the main.go file from your local machine into the current working directory (/app) 
# in the container.
COPY main.go .

# This initializes a new Go module named "guestbook." 
# This is necessary for managing dependencies in Go projects.
RUN go mod init guestbook
# This command cleans up the go.mod file by removing any dependencies that are no longer needed 
# and adding any that are required but missing.
RUN go mod tidy

# This builds the Go application, producing an executable named main. 
# The -o main option specifies the output filename.
RUN go build -o main main.go

# This begins a new build stage, using the official Ubuntu image (version 18.04) as the base 
# for the final image.
FROM ubuntu:18.04

# This copies the built Go executable from the "builder" stage into the /app directory of the 
# final image and renames it to guestbook.
COPY --from=builder /app/main /app/guestbook

# This copies the index.html file from your local public directory into the /app/public directory 
# in the container.
COPY public/index.html /app/public/index.html
# This copies the script.js file into the same directory in the container.
COPY public/script.js /app/public/script.js
# This copies the style.css file into the same directory in the container.
COPY public/style.css /app/public/style.css
# This copies the jquery.min.js file into the same directory in the container.
COPY public/jquery.min.js /app/public/jquery.min.js

# This sets the working directory for the subsequent commands to /app.
WORKDIR /app

# This specifies the command to run when the container starts. 
# Here, it runs the guestbook executable, which is the built Go application.
CMD ["./guestbook"]
# This tells Docker that the container listens on port 3000 at runtime. 
# It’s an indication for anyone running the container about which port to map to the host.
EXPOSE 3000

# SUMMARY
# This Dockerfile is structured into two main stages: 
    # the first stage builds the Go application, 
    # the second stage sets up a lightweight container to run the built application, 
    # along with necessary static files. 
# The use of a multi-stage build helps keep the final image smaller by only including the 
# necessary files and dependencies. 