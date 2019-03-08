# Solr

## Cheatsheet

**TL;DR** :

* List Collection

```bash
curl -s 'http://localhost:8983/solr/admin/collections?action=LIST&wt=json'
```

* Create Collection

```bash
curl -s "http://localhost:8983/solr/admin/collections?action=CREATE&name=montebello&numShards=1&collection.configName=montebello_conf"

```

* Check Cluster Status

```bash
curl -s 'http://localhost:8983/solr/admin/collections?action=CLUSTERSTATUS'
```

* List ConfigSets

```bash
curl -s 'http://localhost:8983/solr/admin/configs?action=LIST'
```

* Delete ConfigSets

```bash
curl -s 'http://localhost:8983/solr/admin/configs?action=DELETE&name=name'
```

* Upload ConfigSets

```bash
bin/solr zk upconfig -n <name for configset> -d <path to directory with configset>

solr zk upconfig -n montebello_conf -d montebello_conf -z 127.0.0.1:9983
```

```bash
(cd solr/server/solr/configsets/sample_techproducts_configs/conf && zip -r - *) > myconfigset.zip

curl -X POST --header "Content-Type:application/octet-stream" --data-binary @myconfigset.zip "http://localhost:8983/solr/admin/configs?action=UPLOAD&name=myConfigSet"
```

**NOTE** : The zip file must be created from within the conf directory (i.e. the solrconfig.xml must be the top level entry in the zip file).

## Containerization

## Reference

1. [Containerizing Zookeeper](https://sookocheff.com/post/docker/containerizing-zookeeper-a-guided-tour/)
2. <https://vsupalov.com/cache-docker-build-dependencies-without-volume-mounting/>