# Pulling & Publishing Images

```bash
$($env:DOCKER_REPO_URL) = "SomeUrl"

# How to grab a public image & push private
$image = "alpine"
$version = "3.13"
$tag = "$($image):$($version)"

docker pull $tag

#Pull the latest image & get the tag
$handle = docker images | grep $image | grep $version
#ubuntu  latest  8dbd9e392a96  12 weeks ago  263 MB (virtual 263 MB)
# $handle = "8dbd9e392a96"

# Tag to create a repository with the full registry location.
# The location becomes a permanent part of the repository name.
docker tag $handle "$env:DOCKER_REPO_URL/$($image):$($version)"

$tag = "$($env:DOCKER_REPO_URL)/$($image):$($version)"

docker tag $handle $tag

# Finally, push the new repository to its home location.
docker push $tag
```

