version: '2'

volumes:
  stocks-news_app:
  stocks-news_signal:

services:
  stocks-news_signal:
    container_name: stocks-news_signal
    image: bbernhard/signal-cli-rest-api:latest
    volumes:
      - stocks-news_signal:/home/.local/share/signal-cli/data/
    restart: unless-stopped
    environment:
      - MODE=normal #Supported modes: json-rpc, native, normal
    ports:
      - 8085:8080 #Required to register device using http://[ip]:[port]/v1/qrcodelink?device_name=[device_name]

  stocks-news_app:
    container_name: stocks-news_app  
    image: williswschan/stocks-news:latest
    restart: unless-stopped
    volumes:
      - stocks-news_app:/data
    links:
      - stocks-news_signal
    environment:
      - n=5
      - news_interval=60
      - tickers=PPSI,FUTU,XPEV,TSLA,NU,BABA,AVCT,ARDX,AMC,NOAH,DB,MRVL,DIDI,BNTX #Desired stock code
      - number=+852xxxxxxxx #Sender number
      #- recipients=group.VWZFcmZJbG1uRG1iV2c1S3pQSjNzc3ZrbmFXaXg1b0pkSFZFRTBvQWhLUT0= #Recipients number or group_id
      - recipients=+852xxxxxxxx
    depends_on:
      - stocks-news_signal
