docker-compose for Twitter + Logstash + ElasticSearch
=============================

### Update Configuration of Logstash 
1. To Update the key , secret , token in logstash.config
2. To Update the "keywords" in logstash.config

		input {  
			twitter {
	
			# add your data

			consumer_key => "YOUR_CONSUMER_KEY"

			consumer_secret => "YOUR_CONSUMER_SECTET"

			oauth_token => "YOUR_OAUTH_TOKEN"

			oauth_token_secret => "YOUR_OAUTH_TOKEN_SECRET"

			keywords => ["NBA"]

			full_tweet => true
				}
			}

		output{
			elasticsearch{

			hosts => "http://elasticsearch:9200/"

			index => "tweets"

			document_type => "NBA"

			template => "/config-dir/twitter_template.json"

			template_name => "twitter"
					}
			stdout { codec => dots }
			}


### To run the container
	docker-compose up

### To connect the head plugin of elasticsearch
	http://localhost:29200/_plugin/head/

![Word Cloud](https://github.com/lucasko-tw/docker-compose-ES-Logstash-Twitter/blob/master/elasticsearch-tweets.png)


### The detail of docker-compose.yml
```YML
	elasticsearch:
	  image: lucasko/elasticsearch
	  user: elsearch
	  ports:
	    - "29200:9200" 
	    - "29300:9300" 
	  restart: always

	logstash:
	  image: logstash
	  links:
	    - elasticsearch
	  volumes:
	    - $PWD:/config-dir
	  command: -f "/config-dir/logstash.config"
	  restart: always
```


