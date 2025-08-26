# NLP Dependency Graph with SpaCy & Neo4j

This project extracts **grammatical dependency relations** from French texts using **SpaCy**, then stores the results in a **Neo4j graph database** running in Docker.

## 🚀 Setup

### 1. Create venv
```bash
python -m venv .venv
source .venv/bin/activate   # Mac/Linux
```

### 2. Install deps
```
pip install spacy neo4j pandas
python -m spacy download fr_core_news_sm
```
### 3. Run Neo4j in Docker
```
docker pull neo4j:latest
```
Set your login: neo4j / test -> USE YOUR username and password below:
```
docker run --name neo4j-test \
  -p 7474:7474 -p 7687:7687 \
  -e NEO4J_AUTH=neo4j/test \
  -d neo4j:latest
```
Neo4j at: http://localhost:7474

### 📓 Usage
Start Jupyter:
```
from neo4j import GraphDatabase
driver = GraphDatabase.driver("bolt://127.0.0.1:7687", auth=("neo4j", "test")) REPLACE WITH YOUR USERNAME/PASSWORD

with driver.session() as session:
    session.execute_write(insert_tokens, tokens)
    session.execute_write(insert_deps, deps)
driver.close()
```
### 🔍 Useful Neo4j Cypher Clauses

MATCH - find existing nodes/edges
```
MATCH (t:Token) RETURN t LIMIT 10;
```
WHERE - filter conditions
```
MATCH (t:Token) WHERE t.pos = "NOUN" RETURN t;
```

CREATE – add a new node/edge
```
CREATE (t:Token {text:"chat", pos:"NOUN"});
```

MERGE – create if not exists, else match
```
MERGE (t:Token {text:"chat"});
```

SET – update properties
```
MATCH (t:Token {text:"chat"}) SET t.lemma="chat";
```
### 🐳 Docker Cheatsheet
```
docker ps              # list containers
docker stop neo4j-test # stop
docker start neo4j-test # start
docker rm -f neo4j-test # remove
```
