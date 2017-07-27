# mappe-cratere
Mappe relative alla zona del cratere.


## Estrazione URL pagine wikipedia da codice comunale ISTAT

La query SPARQL usata è la scelta migliore. Questa è un'alternativa oneline da shell.

I requirementes sono `jq` e `csvkit`.

Questo il comando che:

- estrae soltanto la colonna 7 dal file `comuni_cratere.csv`;
- da questo output rimuove l'intestazione;
- fa una chiamata curl a wikidata, passando il codice ISTAT, e via xargs fa una chiamata per ogni riga del file di input;
- viene estratto il sitelink con jq dall'output in JSON


```bash
<comuni_cratere.csv csvcut -c 7 | tail -n +2 | xargs -I foobar curl -s -X GET "https://query.wikidata.org/bigdata/namespace/wdq/sparql?format=json&query=SELECT%20%3Fsitelink%20WHERE%20%7B%3Fitem%20wdt%3AP635%20%22foobar%22%20.%20%7B%3Fsitelink%20schema%3Aabout%20%3Fitem%20.%20%3Fsitelink%20schema%3AinLanguage%20%22it%22%20%7D%7D" |  jq ".results.bindings[0].sitelink.value"
```

In output

```
"https://it.wikipedia.org/wiki/Force"
"https://it.wikipedia.org/wiki/Cortino"
"https://it.wikipedia.org/wiki/Crognaleto"
"https://it.wikipedia.org/wiki/Fano_Adriano"
"https://it.wikipedia.org/wiki/Isola_del_Gran_Sasso_d%27Italia"
"https://it.wikipedia.org/wiki/Montorio_al_Vomano"
"https://it.wikipedia.org/wiki/Norcia"
"https://it.wikipedia.org/wiki/Poggiodomo"
"https://it.wikipedia.org/wiki/Preci"
...
```