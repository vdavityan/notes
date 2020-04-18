# Gitlab Docker Container Registry Configuration
######1. Gitlab:
Gitlab is already installed via the omnibus package on either a debian/ubuntu/rhel or centos machine (bare or virtual):

######2. Registry resolution:
Set a DNS entry resolving registry.yourdomain.com to the gitlab server, or set a host file entry on any docker hosts that will utilize the container registry, allowing the docker server to talk to the registry instance.

Set the host file in /etc/hosts

  1.2.3.4     registry.example.com registry

######3. Obtain or create a registry certificate:
For this walk through we are going to generate a self signed certificate on the gitlab server for the container registry service to use. As a substition to this step, a certificate can be issued from a local CA, or purchased through an SSL certificate vendor.

######Generate the Key:

######RHEL & CentOS:
```
openssl genrsa -out "/etc/pki/tls/private/gitlab-registry.key" 4096
```
######Debian & Ubuntu:
```
openssl genrsa -out "/etc/ssl/private/gitlab-registry.key" 4096
```
######Generate the Certificate:

######RHEL & CentOS:
```
openssl req -x509 -sha512 -nodes -newkey rsa:4096 -days 730 -keyout /etc/pki/tls/private/gitlab-registry.key \ 
  -out /etc/pki/tls/certs/gitlab-registry.crt
```
######Debian  & Ubuntu:
```
openssl req -x509 -sha512 -nodes -newkey rsa:4096 -days 730 -keyout /etc/ssl/private/gitlab-registry.key \
-out /etc/ssl/certs/gitlab-registry.crt
```

Configuration:
NOTICE:
This walk through assumes that Gitlab was installed using the Omnibus package. If gitlab was instead installed from source,
then please visit http://docs.gitlab.com/ee/administration/container_registry.html for alternate instructions.
Edit /etc/gitlab/gitlab.rb
To configure the registry, a few options need to be set in the /etc/gitlab/gitlab.rb file. The options are as follows
```
registry_external_url 'https://registry.example.com'

################
# Registry     #
################
gitlab_rails['registry_enabled'] = true
gitlab_rails['gitlab_default_projects_features_container_registry'] = false
gitlab_rails['registry_path'] = "/mnt/docker_registry"
gitlab_rails['registry_api_url'] = "https://localhost:5000"

################
# GitLab Nginx #
################
## see: https://gitlab.com/gitlab-org/omnibus-gitlab/tree/629def0a7a26e7c2326566f0758d4a27857b52a3/doc/settings/nginx.md

nginx['enable'] = true
# nginx['client_max_body_size'] = '250m'
nginx['ssl_client_certificate'] = "/etc/gitlab/ssl/ca.crt"
registry_nginx['ssl_certificate'] = "/etc/pki/tls/certs/gitlab-registry.crt"
registry_nginx['ssl_certificate_key'] = "/etc/pki/tls/private/gitlab-registry.key"
registry_nginx['proxy_set_headers'] = { "Host" => "registry.example.com" }
```

A brief description of the option settings above are as follows:

registry_external_url	The URL that the registry will listen on
gitlab_rails['registry_enabled']	Enable the contianer registry service
gitlab_rails['gitlab_default_projects_features_container_registry']	T/F setting for registry enabled on every project
gitlab_rails['registry_path']	Optional registry storage location
gitlab_rails['registry_api_url']	URL Gitlab will use to talk to the registry API
nginx['ssl_client_certificate'] = "/etc/gitlab/ssl/ca.crt"	Tell Nginx to load the default CA root certificates
registry_nginx['ssl_certificate']	Path to the certificate that the registry is going to use
registry_nginx['ssl_certificate_key']	Path to the key for the certificate that the registry will use
registry_nginx['proxy_set_headers']	Workaround for an issue that should have been resolved in 8.10.2, but left in just to be safe

Reconfigure Gitlab
```
gitlab-ctl reconfigure
```
Restart Gitlab
```
gitlab-ctl restart
```
Post Requisites:
NOTICE:
This step is only required if using a self signed certificate

In the event that you are using a self signed certificate, then the docker host has to either have the certificate imported so that its trusted, or docker will need to be made aware that the registry is insecure, or it will not be able to log into the new registry.

NOTICE:
These steps should be performed on the Docker host that will use the Gitlab Container Registry

1. SCP the certs:
The first step is from the gitlab server, scp the certificate only.. NOT THE KEY to the docker host

RHEL & CentOS:
```
scp /etc/pki/tls/certs/gitlab-registry.crt root@dockerhost:/tmp
```
Debian & Ubuntu:
```
scp /etc/ssl/certs/gitlab-registry.crt root@dockerhost:/tmp
```
2. Import the certs:

RHEL &  CentOS:
```
mv /tmp/gitlab-registry.crt /etc/pki/ca-trust/source/anchors/
update-ca-trust
```

Debian & Ubuntu:
```
mv /tmp/gitlab-registry.crt /usr/local/share/ca-certificates/
update-ca-certificates
```
3.  Restart Docker:
```
systemctl restart docker.service
```
ALTERNATIVE:
If for some reason it is undesirable to import the certificate, the docker flag DOCKER_OPTS="--insecure-registry=registry.example.com" can be added to the /etc/default/docker file, or directly to the docker daemon start script / unit file

4.  Docker Login:
```
docker login registry.example.com
```
