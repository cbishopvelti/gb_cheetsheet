

```
nohup node src/index.js migrate --path /grid/BIFO/data/srs_data_b/SRS_DATA/TORS_2020_2 > /var/log/youseqs.log &
```

cpsd data:
```
/var/lib/gb/cpsd/2021.1/review2
```

```

unprot dir:
````
/mnt/svm-chem/grid/BIFO/data/srs_data_b/SRS_DATA/TORS_2020_2
```

staging db: gbjhvice073
run youseqs on: marrs-workspace (gbjhvice075)

docker staging run
```
docker run -d \
-p 2011:5432 \
--name youseqs_postgres \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v /var/lib/pgsql_youseqs/data:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

docker production run
```
docker run -d \
-p 2011:5432 \
--name youseqs_postgres \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v /home/gb-greenphyl/pgsql_youseqs/data:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

Copy the database
```
pg_dump -C -h gbjhvice073.eame.syngenta.org -p 2010 -U youseqs youseqs | psql -h gbjhvice073.eame.syngenta.org -p 5432 -U youseqs youseqs
pg_dump -C -h host.docker.internal -p 2010 -U youseqs youseqs -n public | psql -h localhost -p 5432 -U youseqs youseqs
pg_dump -C -h host.docker.internal -p 2010 -U greenphyl youseqs | psql -h localhost -p 5432 -U youseqs youseqs
```

on 115, copy from staging:
```
pg_dump -C -h gbjhvice073.eame.syngenta.org -p 2011 -U youseqs youseqs -n public | psql -h localhost -p 5432 -U youseqs youseqs
```

### youseqs_2
CREATE UNIQUE INDEX peptide_header_btree
    ON public.peptide USING btree
    (header COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE pg_default;

CREATE UNIQUE INDEX header_btree
    ON public.peptide USING btree
    (header COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE pg_default;


```
vacuum analyze
SET enable_seqscan = OFF;
```

.pgpass
```
gbjhvice073.eame.syngenta.org:2011:youseqs:youseqs:sequences
```

### Run the etl

locally:
```
docker build . -t docker.generalbioinformatics.com/dev-all/youseqs
docker push docker.generalbioinformatics.com/dev-all/youseqs
```

remote (marrs-workspaces)
```
docker pull docker.generalbioinformatics.com/dev-all/youseqs
docker rm youseqs_etl

docker run \
--name youseqs_etl \
-v /var/lib/gb/cpsd/2021.1/review2:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2011 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
ccd5bbd0ac01
```

scp s1041124@marrs-workspace:/var/lib/gb/cpsd/2021.1/review2/Triticum_aestivum.4565.fasta ~/mrmeseq_data/review2/
scp s1041124@marrs-workspace:/var/lib/gb/cpsd/2021.1/review2/Tritrichomonas_foetus.1144522.transcript.fasta ~/mrmeseq_data/review2/
scp s1041124@marrs-workspace:/var/lib/gb/cpsd/2021.1/review2/Tritrichomonas_foetus.1144522.mapping.tsv ~/mrmeseq_data/review2/

```
\copy (select * from transcript where db_source in ('data', 'CPSD2', 'TORS_2020_2')) TO '/tmp/transcript.csv' DELIMITER ',' CSV HEADER;
docker cp youseqs_postgres:/tmp/peptides.csv /tmp/peptides.csv
```

# youseqs v2

postgres
````
docker run -d \
-p 2012:5432 \
--name youseqs_v2_postgres \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v /var/lib/pgsql_youseqs_v2/data:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

youseqs etl
run on marrs-workspace (gbjhvice075)
```
docker pull docker.generalbioinformatics.com/dev-all/youseqs

LOAD cpsd
```
docker run -it -d \
--name youseqs_etl \
-v /var/lib/gb/cpsd/2021.1/review2:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2012 \
-e PG_NAME=youseqs_2 \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e DATABASE_SOURCE=cpsd \
docker.generalbioinformatics.com/dev-all/youseqs
```

LOAD uniprot
```
docker run -it -d \
--name youseqs_etl \
-v /mnt/srs_data_b-svm-nfs01/SRS_DATA/TORS_2021_1:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2012 \
-e PG_NAME=youseqs_2 \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e DATABASE_SOURCE=uniprot \
docker.generalbioinformatics.com/dev-all/youseqs
```

uLTr4PePtide$

### 3
```
postgres
````
docker run -d \
-p 2013:5432 \
--name youseqs_v2_postgres_b \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

```
docker run -it -d \
--name youseqs_etl_6 \
-v /var/lib/gb/cpsd/2021.1/review2:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2013 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

```
docker run -it -d \
--name youseqs_etl_6b \
-v /mnt/srs_data_b-svm-nfs01/SRS_DATA/blast/uniprot_trembl:/data \
-e FILE_PATH=/data/uniprot_trembl.fasta \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2013 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e DATABASE_SOURCE=uniprot \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 4
```
postgres
````
docker run -d \
-p 2014:5432 \
--name youseqs_v2_postgres_c \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata14 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

```
docker run -it -d \
--name youseqs_etl_6 \
-v /var/lib/gb/cpsd/2021.1/review2:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2014 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

```
docker run -it -d \
--name youseqs_etl_6b \
-v /mnt/srs_data_b-svm-nfs01/SRS_DATA/blast/uniprot_trembl:/data \
-e FILE_PATH=/data/uniprot_trembl.fasta \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2014 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e DATABASE_SOURCE=uniprot \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

```
docker run -it -d \
--name youseqs_etl_6c \
-v /var/lib/gb/cpsd/2021.1/august_point_release/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2014 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

To load:
/var/lib/gb/cpsd/2021.1/august_point_release/CPSD/

### TEMP:
```
docker run -it -d \
--name youseqs_etl_6_temp \
-v /var/lib/gb/test_data:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2014 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_DOWN=false \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 5

postgres on gbjhvice073
````
docker run -d \
-p 2015:5432 \
--name youseqs_v2_postgres_d \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata15 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6c \
-v /var/lib/gb/cpsd/2021.1/august_point_release/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2015 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=false \
docker.generalbioinformatics.com/dev-all/youseqs
```

scp -i "~/.ssh/gb-argo.pem" s1041124@marrs-workspace:/var/lib/gb/cpsd/2021.1/august_point_release/CPSD/ \
 ec2-user@ec2-18-219-227-209.us-east-2.compute.amazonaws.com:/efs/cpsd/2021.1/august_point_release/CPSD/


### aws v2

```
docker run -it -d \
--name youseqs_etl_6c \
-v /var/lib/gb/cpsd/2021.1/august_point_release/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=youseqs.cueajylbgodv.us-east-2.rds.amazonaws.com \
-e PORT=5432 \
-e PG_NAME=youseqs_2 \
-e PG_USERNAME=postgres \
-e PG_PASSWORD=uLTr4PePtide$ \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

###TODO DELETE: 
gbjhvice073:/var/lib/pgsql_youseqs_v2/data


### 5

### 6

postgres on gbjhvice073
````
docker run -d \
-p 2016:5432 \
--name youseqs_v2_postgres_e \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata16 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

in youseqs, edit env
```

npm run migrate
```

```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6c \
-v /var/lib/gb/cpsd/2021.1/october_point_release:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2016 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 7

/var/lib/gb/cpsd/2021.2/october_review/CPSD

postgres on gbjhvice073
```
docker run -d \
-p 2017:5432 \
--name youseqs_v2_postgres_f \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata17 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

in youseqs, edit env
```
npm run migrate
```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6f \
-v /var/lib/gb/cpsd/2021.2/october_review/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2017 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### localhost test

```
docker run -d \
-p 2017:5432 \
--name youseqs_v2_postgres_f \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata17 \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13
```

```
docker run -it -d \
--name youseqs_etl_6f \
-v /Users/chris/mrmeseq_data/test8:/data \
-e FILE_PATH=/data \
-e HOST=host.docker.internal \
-e PG_NAME=youseqs_4 \
-e PG_USERNAME=postgres \
-e PG_PASSWORD=postgres \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 8 BBlast

postgres on gbjhvice073
```
docker run -d \
-p 2018:5432 \
--name youseqs_v2_postgres_f_blast \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata18 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

in youseqs, edit env
```
npm run migrate
```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6f_blast \
-v /var/lib/gb/cpsd/2021.2/october_review/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2018 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 9

/var/lib/gb/cpsd/2021.2/october_review/CPSD

postgres on gbjhvice073
```
docker run -d \
-p 2019:5432 \
--name youseqs_v2_postgres_g \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata19 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

in youseqs, edit env
```
npm run migrate
```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6g \
-v /var/lib/gb/cpsd/2021.2/november_review/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2019 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 20
PRODUCTION

postgres on gbjhvice073
```
docker run -d \
-p 2020:5432 \
--name youseqs_v2_postgres_h \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata20 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

in youseqs, edit env
```
npm run migrate
```

on marrs-workspace
```
docker run -it -d \
--name youseqs_etl_6h \
-v /mnt/svm-nfs03/vol/MARRS/var/lib/gb/cpsd/2021.2/november_review/CPSD:/data \
-e FILE_PATH=/data \
-e HOST=gbjhvice073.eame.syngenta.org \
-e PORT=2020 \
-e PG_NAME=youseqs \
-e PG_USERNAME=youseqs \
-e PG_PASSWORD=sequences \
-e INDEX_UP=true \
docker.generalbioinformatics.com/dev-all/youseqs
```

### 21
DELETED
For kubernetes
postgres on gbjhvice054
```
docker run -d \
-p 2021:5432 \
--name youseqs_v2_postgres_i \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata21 \
-v /apps/docker_postgresql:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off \
-c max_connections=800
```

### 22
on gbjhvice115

```
docker run -d \
-p 2022:5432 \
--name youseqs_v2_postgres_h \
-e POSTGRES_PASSWORD=sequences \
-e POSTGRES_USER=youseqs \
-e PGDATA=/var/lib/postgresql/data/pgdata22 \
-v /mnt/usre_fasta_mirror/dataload:/var/lib/postgresql/data \
--shm-size=1g \
--add-host=host.docker.internal:host-gateway \
postgres:13 \
-c shared_buffers=13GB \
-c effective_cache_size=13GB \
-c enable_seqscan=off
```

```
docker exec -it youseqs_v2_postgres_h /bin/bash
```