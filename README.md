# gitlab-install

container üzerinde kurmak için.

```mkdir gitlab
docker run -d --hostname gitlab.devops.cloudns.asia \
-p 443:443 -p 80:80 -p 2222:22 \
--name gitlab \
--restart unless-stopped \
-v /root/gitlab/config:/etc/gitlab \
-v /root/gitlab/logs:/var/log/gitlab \
-v /root/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest
```

root password için container içinde /etc/gitlab/initial_root_password dan alırız.

runner için.

```
mkdir gitlab-runner
docker run -d --name gitlab-runner --restart always \
    -v /root/gitlab-runner/config:/etc/gitlab-runner \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:v14.7.0
``` 

runner kurulduktan sonra toml dosyasında priviled yapmamız gerekiyor.

```
sed -i 's,privileged = false,privileged = true,g' /etc/gitlab-runner/config.toml
```