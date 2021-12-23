# stocks-news

version: '2'

services:
  stocks-news_signal:
    container_name: stocks-news_singal
    image: bbernhard/signal-cli-rest-api:latest
    restart: unless-stopped
    environment:
      - MODE=normal #Supported modes: json-rpc, native, normal
    ports:
      - 8085:8080 #Required to register device using http://[ip]:[port]/v1/qrcodelink?device_name=[device_name]

  stocks-news_app:
    container_name: stocks-news_app  
    image: williswschan/stocks-news:latest
    restart: unless-stopped
    links:
      - stocks-news_signal
    environment:
      - n=5
      - news_interval=60
      - tickers=PPSI,FUTU,XPEV,TSLA,NU,BABA,AVCT,ARDX,AMC,NOAH,DB,MRVL,DIDI,BNTX #Desired stock code
      - number=+[country_code][mobile_number] #Sender number
      - recipients=+[country_code][mobile number] #Recipients number or group_id      
    depends_on:
      - stocks-news_signal
