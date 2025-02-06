+++
title = 'SpotBot Docker Container'
date = 2025-01-20T11:11:20-05:00
[cover]
image = "images/docker.jpg"
+++


## Introduction

A few friends and I are working on an open-source project called [SpotBot](https://karltrowbridge1.github.io/projects/spotbot/). The best description of the project is "an overengineered shared playlist." While working on this project, things became a little convoluted. Initially, we had issues with the application running properly on Linux. I took the initiative to test the entire application on a virtual machine running Debian to ensure everything functioned correctly in a Linux environment. Here's how the process went:

1. Start the VM
2. Delete the old repo that was locally cloned from GitHub
3. Enter the VENV
4. Re-clone the repo that needs to be tested
5. Move the configuration file to the directory
6. Begin testing

I imgaine anyone who works in automation is disappointed.

Docker quickly became the obvious choice to automate this entire process. Through this endeavor, I learned the benefits of Docker's lightweight nature, the usability of volumes for a better user experience, and how application concerns often extend beyond the end user.

A link to the SpotBot Docker testing container can be found [here](https://github.com/karltrowbridge1/spotbotDocker).

## Lightweight Containers

Initially, the container I created utilized Ubuntu, which was very straightforward to work with. Many of the tools the application relies on were already included in the image, making build times fast.

After sharing my work with a friend more experienced with Docker, he provided feedback that shaped the lessons in this post. He suggested using Alpine or Manjaro Linux instead.

I transitioned to Alpine, which required modifying several aspects of the `Dockerfile`. While this added steps to install additional packages during the build process, it resulted in a container that was much more efficient to run and deploy.

## Volumes

To run SpotBot, the administrator needs to configure and provide a JSON file containing setup information. In my first attempt at creating the container, I copied this file into the container via the `Dockerfile`. However, my friend pointed out that this approach was not best practice. Instead, utilizing a [volume](https://docs.docker.com/engine/storage/volumes/) allows users to build the image once and run the container with the flexibility to update setup files without rebuilding.

After adjusting the configuration, users can now freely add or change a setup file on the fly via a volume.

For ease of use, I created a directory, `/app/setup/`, for the volume to mount. The container's run command now requires a volume declaration:
`-v /path/to/local/dirContainingSetupFile:/app/setup`

This allows the startup shell script to reference the `setup.json` file provided by the user.

## Issues in Automation Process

An interestign issue with the flask applicaiton used for authentication with the Spotify servers was made apparant when the prompt I had created to close the application after authentication got burried by logs from the flask application.

To terminate the process after successful authentication, I used a `read` statement in the bash script. In classic primitive programming fashion, the application closes when the user hits <Enter>.

When running SpotBot on bare metal, the webserver is stopped via a keyboard interrupt. While this approach works well enough, it’s a low-priority item on the to-do list. However, creating this Docker container taught me the importance of thinking beyond just the end user to consider system integration and automation. Where a keyboard interrupt worked manually, automated systems may struggle with such a design. This was a small but valuable takeaway.

## Conclusion

Creating and using this container for fast testing has been an excellent time-saver and learning experience. The ability to spin up and tear down an entire environment in seconds is fantastic. Passing this container between developers has significantly reduced the "it works on my machine" scenarios that lead to broken code being pushed.

Furthermore, this simple container can serve as a platform to jumpstart a production container, simplifying administration for end users.

Enjoying this project has inspired me to explore creating a CI/CD pipeline for SpotBot. One step at a time.