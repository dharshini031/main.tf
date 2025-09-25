# main.tf
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.0"
    }
  }
}

provider "docker" {}
 #Creating a Docker Image ubuntu with the latest as the Tag.
resource "docker_image" "ubuntu" {
  name = "ubuntu:latest"
}

# Creating a Docker Container using the latest ubuntu image.
resource "docker_container" "webserver" {
  image             = docker_image.ubuntu.latest
  name              = "terraform-docker-test"
  must_run          = true
  publish_all_ports = true
  command = [
    "tail",
    "-f",
    "/dev/null"
  ]
}

# Pull the latest Nginx image
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "nginx-test"
  ports {
    internal = 80
    external = 8000
  }
}

# Run a container using the image
resource "docker_container" "nginx" {
  name  = "my-nginx"
  image = docker_image.nginx.image_id
  ports {
    internal = 80
    external = 8080
  }
}
# deploy resource
resource "docker_network" "private_network" {
  name = "my_network"
}

resource "docker_secret" "foo" {
  name = "foo"
  data = base64encode("{\"foo\": \"s3cr3t\"}")
}

resource "docker_volume" "shared_volume" {
  name = "shared_volume"
}

# The source image must exist on the machine running the docker daemon.
resource "docker_tag" "tag" {
  source_image = "xxxx"
  target_image = "xxxx"
}

