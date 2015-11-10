# docker-elk

For logging from external applications to the ELK-stack, setup logstash-forwarder (https://github.com/elastic/logstash-forwarder) to log to the open port from 5043 and create a lumberjack.crt and lumberjack_ca.crt and key which you add to the logstash docker container through docker-logstash-forwarder.yml through the volume part(check docker-logstash-forwarder.yml and logstash.conf). You also have to add a static field "parseas" to "json"  in order for the message part to be parsed as json. Example:
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

To make your logback logging java application log as json, check out https://github.com/logstash/logstash-logback-encoder
