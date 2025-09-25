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

# Pull the latest Nginx image
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
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
