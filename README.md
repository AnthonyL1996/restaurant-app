# restaurant-app

Steps building

Create GH PAT token

Execute following commands on dev machine to auth to GH container registery
export CR_PAT=YOUR_TOKEN
echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin

Create image

pack build ghcr.io/anthonyl1996/restaurant-app:latest \
--builder paketobuildpacks/builder:base \
--path . \
--env "BP_JVM_VERSION=17"

Go to https://github.com/users/AnthonyL1996/packages/container/restaurant-app/settings set to public

Upload image
fly launch --image ghcr.io/anthonyl1996/restaurant-app:latest

GraalVM image

Image 5.12.0

pack build ghcr.io/anthonyl1996/restaurant-app:latest \
--builder paketobuildpacks/builder:tiny \
--buildpack paketo-buildpacks/java-native-image@7.26.1 \
--path . \
--env "BP_JVM_VERSION=17" \
--env "BP_NATIVE_IMAGE=true" \
--cache-image ghcr.io/anthonyl1996/restaurant-app-paketo-cache-image:latest \
--publish


./mvnw spring-boot:build-image -Pnative
docker tag restaurant-app:0.0.1-SNAPSHOT ghcr.io/anthonyl1996/restaurant-app:latest
docker push ghcr.io/anthonyl1996/restaurant-app:latest

