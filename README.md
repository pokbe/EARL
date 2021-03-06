# EARL 
## Joint Entity and Relation Linking for Question Answering

#### UPDATE (16.08.2018): We have new bloom files for download. The earlier blooms had some triples missing.

EARL (Entity and Relation Linker), a system for jointly linking entities and relations in a question to a knowledge graph. EARL treats entity linking and relation linking as a single task and thus aims to reduce the error caused by the dependent steps. To realise this, EARL uses the knowledge graph to jointly disambiguate entity and relations. EARL obtains the context for entity disambiguation by observing the relations surrounding the entity. Similarly, it obtains the context for relation disambiguation by looking at the surrounding entities. We support multiple entities and relations occurring in complex questions by modelling the joint entity and relation linking task as an instance of the Generalised Travelling Salesman Problem (GTSP).


# INSTRUCTIONS

    $cd scripts/
    $python TextMatchServer.py 8888  (this takes several minutes to load)
    $python api.py 4999

This starts the api server at port 4999. Install all dependencies required that are mentioned in dependencies.txt. Download bloom files from https://drive.google.com/drive/folders/1lKu0tVA5APhZVOZqRQK2tCk0FDj82lvo?usp=sharing and store them at data/blooms/. Download the archived elastic search dumps from the same google drive link and import them into a local running elasticsearch 5.x instance. The mappings can be found in data/elasticsearchdump/ folder. Also download https://www.dropbox.com/s/flh1fjynqvdsj4p/lexvec.commoncrawl.300d.W.pos.vectors.gz?dl=1, unzip it, and store it in data/ folder.

To import elasticsearch data one could install elasticdump https://www.npmjs.com/package/elasticdump

    npm install elasticdump -g

Then import the mapping:

    elasticdump --input=dbentityindex11mapping.json  --output=http://localhost:9200/dbentityindex11 --type=mapping
    
Then import the actual data:

    elasticdump --limit=10000 --input=dbentityindex11.json  --output=http://localhost:9200/dbentityindex11 --type=data

You may need to add the following to the above elasticdump commands to make it work on some setups:

    --headers='{"Content-Type": "application/json"}'
    
    
To consume the API

    curl -XPOST 'localhost:4999/processQuery' -H 'Content-Type: application/json' -d"{\"nlquery\":\"Who is the president of USA?\"}"

