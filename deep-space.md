
### Build

```
docker build -t deep-space . && docker run -v ./universes:/usr/src/app/universes -P --name deep-space deep-space
docker tag deep-space docker.generalbioinformatics.com/dstein/deep-space:latest
docker push docker.generalbioinformatics.com/dstein/deep-space
```


### machine

gbjhvice075


```
cp ~/deep-space/universes/identifier-mapping.yaml /app/deep-space/identifier-mapping/universe.yaml

docker stop deep-space-identifier-mapping && docker rm deep-space-identifier-mapping

docker run -it -d \
 --name deep-space-identifier-mapping \
 -p 3000:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=identifier-mapping.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
```

```
cp ~/deep-space/universes/peptide-annotation.yaml /app/deep-space/peptide-annotation/universe.yaml

docker stop deep-space-peptide-annotation && docker rm deep-space-peptide-annotation

docker run -it -d \
 --name deep-space-peptide-annotation \
 -p 3001:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=peptide-annotation.yaml \
 docker.generalbioinformatics.com/dstein/deep-space

docker exec -it deep-space-peptide-annotation sh
```

##### staging


```
cp ~/deep-space/universes/universe-staging.yaml /app/deep-space/ceres-graphql-playground-staging/universe.yaml

docker stop deep-space-staging && docker rm deep-space-staging

docker run -it -d \
 --name deep-space-staging \
 -p 3002:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=universe-staging.yaml \
 docker.generalbioinformatics.com/dstein/deep-space

docker exec -it deep-space-staging sh
```
```
docker stop deep-space-production && docker rm deep-space-production

cp ~/deep-space/universes/universe-production.yaml /app/deep-space/ceres-graphql-playground-production/universe.yaml

docker run -it -d \
 --name deep-space-production \
 -p 3003:3000 \
 -e GRAPHQL_HOST=ceres.app.intra \
 -e BEARER_TOKEN=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiQmlzaG9wIENocmlzIEdCSkg6czEwNDExMjQiLCJkaXNwbGF5TmFtZSI6IkNocmlzIn0sImlhdCI6MTYwNTYzMjY1OSwiZXhwIjoxNjM3MTY4NjU5fQ.NLq7pplagNfd7G21XOr0CNZqqAicX12abx7OavT9sdk \
 -e UNIVERSE_NAME=universe-production.yaml \
 -v /app/deep-space/etc:/usr/src/app/etc \
 docker.generalbioinformatics.com/dstein/deep-space
```

##### Reaxys
```
cp ~/deep-space/universes/reaxys-poc.yaml /app/deep-space/ceres-graphql-playground-reaxys/universe.yaml

docker stop deep-space-reaxys && docker rm deep-space-reaxys

docker run -it -d \
 --name deep-space-reaxys \
 -p 3004:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=reaxys-poc.yaml \
 docker.generalbioinformatics.com/dstein/deep-space

docker exec -it deep-space-peptide-annotation sh
```



### Troubleshooting

Run docker run as root

##### targetfinder-dev

```
cp ~/deep-space/universes/targetfinder-dev.yaml /app/deep-space/targetfinder-dev/universe.yaml

docker stop targetfinder-dev && docker rm targetfinder-dev

docker run -it -d \
 --name targetfinder-dev \
 -p 3005:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=targetfinder-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

 ##### identifier-mapping-dev

 ```
cp ~/deep-space/universes/identifier-mapping-dev.yaml /app/deep-space/identifier-mapping-dev/universe.yaml

docker stop identifier-mapping-dev && docker rm identifier-mapping-dev

docker run -it -d \
 --name identifier-mapping-dev \
 -p 3006:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=identifier-mapping-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### peptide-annotation-dev

 ```
cp ~/deep-space/universes/peptide-annotation-dev.yaml /app/deep-space/peptide-annotation-dev/universe.yaml

docker stop peptide-annotation-dev && docker rm peptide-annotation-dev

docker run -it -d \
 --name peptide-annotation-dev \
 -p 3007:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=peptide-annotation-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### interaction-dev

 ```
cp ~/deep-space/universes/interaction-dev.yaml /app/deep-space/interaction-dev/universe.yaml

docker stop interaction-dev && docker rm interaction-dev

docker run -it -d \
 --name interaction-dev \
 -p 3008:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=interaction-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### homologs-dev

 ```
cp ~/deep-space/universes/homologs-dev.yaml /app/deep-space/homologs-dev/universe.yaml

docker stop homologs-dev && docker rm homologs-dev

docker run -it -d \
 --name homologs-dev \
 -p 3009:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=homologs-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### phenotypes-dev

 ```
cp ~/deep-space/universes/phenotypes-dev.yaml /app/deep-space/phenotypes-dev/universe.yaml

docker stop phenotypes-dev && docker rm phenotypes-dev

docker run -it -d \
 --name phenotypes-dev \
 -p 3010:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e UNIVERSE_NAME=phenotypes-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### opentargets-dev

 ```
cp ~/deep-space/universes/opentargets-dev.yaml /app/deep-space/opentargets-dev/universe.yaml

docker stop opentargets-dev && docker rm opentargets-dev

docker run -it -d \
 --name opentargets-dev \
 -p 3011:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e GRAPHQL_HOST=api.platform.opentargets.org \
 -e GRAPHQL_SERVER_TYPE=https \
 -e GRAPHQL_PATH=api/v4/graphql \
 -e GRAPHQL_PORT=443 \
 -e UNIVERSE_NAME=opentargets-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space
 ```

##### gex-dev

 ```
cp ~/deep-space/universes/gex-dev.yaml /app/deep-space/gex-dev/universe.yaml

docker stop gex-dev && docker rm gex-dev

docker run -it -d \
 --name gex-dev \
 -p 3012:3000 \
 -v /app/deep-space/etc:/usr/src/app/etc \
 -e GRAPHQL_HOST=gbjhvice073.eame.syngenta.org \
 -e GRAPHQL_SERVER_TYPE=http \
 -e GRAPHQL_PATH=marrs/v1/graphql \
 -e GRAPHQL_PORT=81 \
 -e UNIVERSE_NAME=gex-dev.yaml \
 docker.generalbioinformatics.com/dstein/deep-space

 docker exec -it gex-dev sh
 ```

