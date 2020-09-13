# Introduction
In this section, you’ll learn how to get up and running with a stand-alone Anchore Engine installation for trial, demonstration and review with Docker Compose.

# Requirements

The following instructions assume you are using a system running Docker v1.12 or higher, and a version of Docker Compose that supports at least v2 of the docker-compose configuration format. This is provided by the Katacoda environment.

- A stand-alone installation will requires at least 4GB of RAM, and enough disk space available to support the largest container images you intend to analyze (we recommend 3x largest container image size). For small images/testing (basic Linux distro images, database images, etc), between 5GB and 10GB of disk space should be sufficient.
