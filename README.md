# docker-elk
Docker-compose for setting up a elk-stack, with automatic logging from docker containers (stdin/stdout) and other log files in the container by configuring a /etc/logstash-forwarder.conf file in the container. Check out https://github.com/digital-wonderland/docker-logstash-forwarder for more information. (PS: does not work very well with aufs).

## JSON logging from your application
To make your logback logging java application log as json, check out https://github.com/logstash/logstash-logback-encoder

## External logging setup
For logging from external applications to the ELK-stack, 
1. setup logstash-forwarder (https://github.com/elastic/logstash-forwarder) on the nodes you want to log from to log to port 5043 on the machine with the elk stack.
2. Create a lumberjack.crt and lumberjack_ca.crt and key which you add to the logstash docker container through docker-logstash-forwarder.yml through the volume part(check docker-logstash-forwarder.yml and logstash.conf) and in the logstash-forwarder.conf on the logging machines.
3. Add a static field "parseas" to "json"  in order for the message part of your log files to be parsed as json in your logstash-forwarder.conf on the external nodes you want to log from. Example:
```
{                                                                                                                                                                                             
    "network":{                                                                                                                                                                               
        "servers":["logstash:5043"],                                                                                                                                                          
        "ssl certificate":"./lumberjack.crt",                                                                                                                                                 
        "ssl key":"./lumberjack.key",                                                                                                                                                         
        "ssl ca":"./lumberjack_ca.crt"                                                                                                                                                        
    },                                                                                                                                                                                        
    "files":[                                                                                                                                                                                 
        {                                                                                                                                                                                     
            "paths":["/usr/local/tomcat/logs/access.log"],                                                                                                                                    
            "fields":{                                                                                                                                                                        
                "type":"tomcat-access",                                                                                                                                                       
                "parseas":"json"                                                                                                                                                              
            }                                                                                                                                                                                 
        }                                                                                                                                                                           
    ]                                                                                                                                                                                         
}
```

## Kibana
To log into kibana, go to 127.0.0.1:9200/_plugin/kibana3 on the machine you deployed your elk stack.

## Bugs/Readme
There are a few known bugs and the elasticsearch and kibana version are outdated.

I'm going to improve the documentation (this readme) when I get time. The goal is to make this a 5 second setup solution for proper log handling for all your applications/projects.
