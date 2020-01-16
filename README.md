# elk-with-filebeat-by-docker-compose
![GitHub](https://img.shields.io/github/license/gnokoheat/elk-with-filebeat-by-docker-compose) ![GitHub last commit](https://img.shields.io/github/last-commit/gnokoheat/elk-with-filebeat-by-docker-compose)

ELK with Filebeat by Docker-compose - Simple &amp; Easy way to file logging

## Work flow
- mylog -> filebeat -> logstash -> elasticsearch <- kibana

## Usage

### Make ELK with Filebeat
1. Docker & Docker-compose install (Ubuntu)
```
# Docker install
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $(whoami)
```
```
# Docker-compose install
sudo apt-get install docker-compose
```

2. Git clone this repository
```
git clone https://github.com/gnokoheat/elk-with-filebeat-by-docker-compose
cd elk-with-filebeat-by-docker-compose/
```

3. Set to making your log file into 'mylog' folder.
- Filebeat can auto-detect to *.log file was made, updated and push it to logstash.

4. Docker-compose up command
```
docker-compose up -d
```

### Customize Config
- logstash.conf
```
# Change 'timestamp' to your log custom timestamp key
filter {
  ...
  date{
    match => ["timestamp", "UNIX_MS"]
    target => "@timestamp"
  }
}
```
```
# Change 'time.localtime' to your location time
filter {
  ...
  ruby {
    code => "event.set('indexDay', event.get('[@timestamp]').time.localtime('+09:00').strftime('%Y%m%d'))"
  }
}
```
- logstash.template.json : Change it to your log index
