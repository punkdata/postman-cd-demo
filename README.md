# Building Continuos Delivery Pipelines in CircleCI with Newman Stream code sample 

This repository hosts the code examples for the [Building Continuos Delivery Pipelines in CircleCI with Newman Stream code sample](https://youtu.be/xyu7CG3msQY) session

**NOTE**: In the stream this project didn't work because we ran out of time but this repo has the complete example in this [config.yml](.circleci/config.yml)

Here is the code example of the newman pipeline job:

```
  newman_smoketest:
    docker:
     - image: cimg/node:14.16.0
    steps:
      - checkout
      - attach_workspace:
          at: /tmp/ecs/
      - run:
          name: Update Postman Env host-url
          command: |
            source /tmp/ecs/endpoint
            sudo npm install -g newman
            curl --location -g --request PUT 'https://api.getpostman.com/environments/a297f3e8-03a3-4802-8f2c-21398c28aaf7' \
                --header 'Content-Type: application/json' \
                --header 'X-API-Key: '"$POSTMAN_API_KEY"'' \
                  --data-raw '{
                      "environment": {
                          "values": [
                              {
                                  "key": "host-url",
                                  "value": "'"$ENDPOINT"'"
                              }
                          ]
                      }
                  }'
      - newman/newman-run:
          collection: https://api.getpostman.com/collections/85d349a8-a486-431d-a258-aaf94f91915d?apikey=${POSTMAN_API_KEY}
          environment: https://api.getpostman.com/environments/a297f3e8-03a3-4802-8f2c-21398c28aaf7?apikey=${POSTMAN_API_KEY}
```