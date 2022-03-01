

```
ssh s1041124@gbjhvice054
cd ~/reaxys && node proxy.js
```

```
{
  proteins(
    ids: ["P25087"]
    chemblmap: false
    pdbmap: false
    limit: 100
    offset: 0
  ) {
    id,
    reaxys {
      citations {
        article_count
      },
      bioactivity {
      	id,
        subunit_name,
        reaxys_organism,
        target_type,
        target_details,
        target_key,
        target_short_key,
        target_role,
      }
    }
  }
}
```

Working proteins ids: 
```
P25087
```

None working proteins ids:
```
A0A021WW32, Q6BX67, Q32LM0

```