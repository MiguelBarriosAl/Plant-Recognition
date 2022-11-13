
<h1 align="center"> Deploy Tensorflow model with FastAPI and Docker</h1>
For this project I pretend to simulate a production deployment of a Tensorflow model taking into account the client component that will request predictions from the model, the front-end component that will develop the services with FastAPi, and the model in docker compose.

# Table of Contents

- [Model in DEV](#model-in-dev)
- [Model in PROD](#model-in-prod)

## Model In DEV

### Docker Container *.pd

`1. docker pull tensorflow/serving`

`2. env var => export MODEL_PB=$(pwd)/model/tf2x/tensorflow/`

`3. docker run -p 9500:8500 -p 9501:8501 -v "$MODEL_PB:/models/flowers/" -e MODEL_NAME=flowers -t tensorflow/serving &`

`4. Into of localhost server`

  `curl http://localhost:9501/v1/models/flowers`
    
    {
     "model_version_status": [
      {
       "version": "1",
       "state": "AVAILABLE",
       "status": {
        "error_code": "OK",
        "error_message": ""
       }
      }
     ]
    }


Test Http Model Images 

Command: python3
Script: test/test-http.py
Arguments: 
- "-i", "--image": Image PATH is required.
- "-m", "--model": Model NAME is required.
- "-v", "--version": Model VERSION is required.
- "-p", "--port": Model PORT number is required.

    `python3 test-http.py --image images/img01.jpg --model flowers --version 1 --port 9501`

## Model In PROD
### Docker compose

    docker-compose -f compose-config.yml up &

  `curl http://localhost:9501/v1/models/flowers`
    
    {
     "model_version_status": [
      {
       "version": "1",
       "state": "AVAILABLE",
       "status": {
        "error_code": "OK",
        "error_message": ""
       }
      }
     ]
    }

    docker-compose -f compose-config.yml down

### Docker Swarm
    cd docker/

    docker/
    ├── compose-config-swarm.yml
    └── compose-config.yml
    
    ### START
    docker stack deploy -c compose-config-swarm.yml MYSTACK

    #### Visualizer
    curl http://localhost:9001/

    ### STOP
    docker service ls
    docker service rm <ID>
    
    curl http://localhost:9001/

