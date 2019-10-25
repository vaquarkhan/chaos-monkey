# Chaos-monkey

Chaos Monkey is a software tool that was developed by Netflix engineers to test the resiliency and recoverability of their Amazon Web Services


![Alt Text](https://scontent-ort2-2.xx.fbcdn.net/v/t1.0-9/51830166_2076134922467281_773329791020433408_n.jpg?_nc_cat=107&_nc_eui2=AeEWUvTs70bHfecJqdHOuUQg_y5iTG-XGgJzqnlwSv58RjcKNMj9tiVRzxGiTSAL5XCd1GmkywrxqYaRRQUA_XwTTnKrAmHuftf2mBlIAgpAVg&_nc_oc=AQnOIocjtA5hnET2KiYbbpsqjdxA1616b3sl8Ss4QhW3j_okdkl-iy_-fR3nClIIdUwD7GKgDvUD2e_9VgavrsVe&_nc_ht=scontent-ort2-2.xx&oh=21203f10f265791df2aebb3463186ad2&oe=5E18783B)


Imagine a monkey entering a “data center”, these “farms” of servers that host all the critical functions of our online activities. The monkey randomly rips cables, destroys devices and returns everything that passes by the hand. The challenge for IT managers is to design the information system they are responsible for so that it can work despite these monkeys, which no one ever knows when they arrive and what they will destroy.

Chaos Monkeys by Antonio Garcia Martinez

- https://www.youtube.com/watch?v=7sQiIR9qCdA&list=PLD9VybHH2wnZW1UdD1AwgR61a0Ba0OFb5&via=tb

-------------------------------------------------------------

## Chaos Engineering is 
"the discipline of experimenting on a distributed system in order to build confidence in the system's capability to withstand turbulent conditions in production.

- https://principlesofchaos.org/?lang=ENcontent
- https://www.gremlin.com/community/tutorials/chaos-engineering-the-history-principles-and-practice/
- https://medium.com/netflix-techblog/tagged/chaos-monkey
- https://codecentric.github.io/chaos-monkey-spring-boot/

-----------------------------------------------------------
![Alt Text](https://segmentfault.com/img/remote/1460000014509662?w=2694&h=1122)


## What is Resilient Testing?
it's Non Functional Testing, Certifying one system is resilient to some extent.
This testing is very much needed for 24*7 service apps like ecommerce, healthcare apps etc


## Springboot Chaos support

- https://codecentric.github.io/chaos-monkey-spring-boot/

### Assault

- Latency Assault – Inject random latency to the request

                chaos.monkey.assaults.latencyActive=true
                chaos.monkey.assaults.latencyRangeStart=3000
                chaos.monkey.assaults.latencyRangeEnd=15000

 or 
 
 
                 curl -X POST http://localhost:8080/chaosmonkey/assaults \
                -H 'Content-Type: application/json' \
                -d \
                '
                {
                    "latencyRangeStart": 2000,
                    "latencyRangeEnd": 5000,
                    "latencyActive": true,
                    "exceptionsActive": false,
                    "killApplicationActive": false
                }'
                
                
                
- Exception Assault – throws Runtime Exception


                  curl -X POST http://localhost:8080/chaosmonkey/assaults \
                  -H 'Content-Type: application/json' \
                  -d \
                  '
                  {
                      "latencyActive": false,
                      "exceptionsActive": true,
                      "killApplicationActive": false
                  }'


- AppKiller Assault – kill application


                  curl -X POST http://localhost:8080/chaosmonkey/assaults \
                  -H 'Content-Type: application/json' \
                  -d \
                  '
                  {
                      "latencyActive": false,
                      "exceptionsActive": false,
                      "killApplicationActive": true
                  }'

### Watcher
By default, Watcher is only enabled for our services @Service, but we can enable in propeties file 

                      chaos.monkey.watcher.controller=false
                      chaos.monkey.watcher.restController=false
                      chaos.monkey.watcher.service=true
                      chaos.monkey.watcher.repository=false
                      chaos.monkey.watcher.component=false
                      
                      



application.properties 

                        chaos.monkey.enabled: true
                        # actuator REST API
                        management.endpoint.chaosmonkey.enabled: true

application.yml

                            chaos:
                              monkey:
                                assaults:
                              level: 8
                              latencyRangeStart: 1000
                              latencyRangeEnd: 10000
                              exceptionsActive: true
                              killApplicationActive: true
                            watcher:
                              repository: true
                                  restController: true
                            management:
                                endpoint:
                                  chaosmonkey:
                                    enabled: true
                                endpoints:
                                  web:
                                    exposure:
                                      include: health,info,chaosmonkey

                                  
---------------------------------

                          POST  http://127.0.0.1:8080/actuator/chaosmonkey/enable

                          POST  http://127.0.0.1:8080/actuator/chaosmonkey/disable

                          POST  http://127.0.0.1:8080/actuator/chaosmonkey/assaults

                          {
                            "level": 1,
                            "latencyRangeStart": 100,
                            "latencyRangeEnd": 600,
                            "latencyActive": true,
                            "exceptionsActive": false,
                            "killApplicationActive": false,
                            "restartApplicationActive": false,
                          }

-------------------------------
Pom

                        <dependency>
                            <groupId>de.codecentric</groupId>
                            <artifactId>chaos-monkey-spring-boot</artifactId>
                            <version>2.0.0-SNAPSHOT</version>
                        </dependency>
                        
                        
 Set profile 
 
                       $ java -jar target/xyz-service-1.0-SNAPSHOT.jar --spring.profiles.active=chaos-monkey
                       
                       
                       




- https://www.thecuriousdev.org/spring-boot-chaos-monkey/
- https://piotrminkowski.wordpress.com/2017/06/26/monitoring-microservices-with-spring-boot-admin/
- https://dzone.com/articles/chaos-monkey-for-spring-boot-microservices

