
### Dev cache
```
ssh -XAC -o 'ServerAliveInterval 240' -L 6376:marrs-development.xbxas3.0001.euc1.cache.amazonaws.com:6379 chris@lovelace.eame.syngenta.org
```

### Staging cache

```
ssh -XAC -o 'ServerAliveInterval 240' -L 6378:marrs-staging.xbxas3.0001.euc1.cache.amazonaws.com:6379 chris@lovelace.eame.syngenta.org
```

### Production cache

```
ssh -XAC -o 'ServerAliveInterval 240' -L 6377:marrs-production.xbxas3.0001.euc1.cache.amazonaws.com:6379 chris@lovelace.eame.syngenta.org
```

### Develpoment/dataloading cache

```
ssh -XAC -o 'ServerAliveInterval 240' -L 6370:marrs-development.xbxas3.0001.euc1.cache.amazonaws.com:6379 chris@lovelace.eame.syngenta.org
```

### FLUSHALL
```
redis-cli -p 6376
FLUSHALL
```