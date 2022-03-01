

```
./ansible-playbook -i ./inventory.conf "plays/mushroom/helloWorld.yml"
```

### deploy phobos staging
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/phobos.yml" --limit gbjhvice134 --tags apps/phobos
```

### deply phobos PRODUCTION
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/phobos.yml" --limit gbjhvice069 --tags apps/phobos
```

deply phobos LOADING
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/phobos.yml" --limit gbjhvice054 --tags apps/phobos
```

deploy mrmeseq:
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/blaster.yml" --tags apps/blaster/mrmeseqs
```

### deploy PRODUCTION vertuoso
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/marrs.yml" --limit gbjhvice069 --tags services/virtuoso
```

```
./ansible-playbook -i ./inventory.conf "plays/mushroom/phobos.yml" --limit gbjhvice069 --tags services/metricbeat
```

### deploy metric beat blaster

```
./ansible-playbook -i ./inventory.conf "plays/mushroom/metricbeat.yml" --limit gbjhvice134
```

### set repo

```
./ansible-playbook -i ./inventory.conf "plays/mushroom/base.yml" --limit gbjhvice134 --tags repo
```

### deploy phobos dataloading
```
./ansible-playbook -i ./inventory.conf "plays/mushroom/phobos.yml" --limit gbjhvice054 --tags apps/phobos
```