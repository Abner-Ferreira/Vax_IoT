# Vax_IoT
Projeto de IoT para a Global Solutions da Faculdade de Informática e Administração Paulista (FIAP)


# Introdução do Projeto
Atualmente, é evidente que nem todas as pessoas têm acesso à imunização adequada. No Brasil, mais de 69 milhões de cidadãos ainda não receberam a primeira dose da vacina contra a COVID-19, conforme divulgado pelo governo brasileiro. 
A pergunta que surge é: por que essa lacuna existe? A resposta remonta à persistente desigualdade que permeia o país desde o seu surgimento, sendo um dos principais obstáculos para a plena vacinação.

Nos dá VAX temos uma resposta direta a essa desigualdade. Trata-se de um aplicativo gratuito destinado a possibilitar que pessoas em todo o território nacional identifiquem quais vacinas são recomendadas para elas e onde podem recebê-las com facilidade. O aplicativo desempenhará um papel crucial ao ajudar os indivíduos a se organizar, fornecendo um calendário de vacinação e permitindo que se preparem de maneira adequada para receber as doses necessárias. 
Além disso, oferecerá informações sobre quais vacinas ainda estão pendentes, permitindo que as pessoas se programem de forma eficaz e, assim, melhorem sua qualidade de vida.


# Como utilizamos a IoT no projeto?
Utilizamos a esp32 junto com o dht22 para verificar a temperatura do paciente e verificar se ele poderá ou não tomar a vacina, ao verificar que sua temperatura esta maior que 38°C o led vermelho acenderá dizendo que ele não está apto a tomar a vacina, caso sua temperatura seja menor que 35°C o led amarela ligará dizendo que ele não está apto a tomar a vacina e se sua temperatura estiver normal, entre 35 e 38 graus celsius, o led verde acenderá dizendo que ele está apto a tomar a vacina prosseguir com o processo.
Utilizamos o MQTT para se conectar com o WIFI e enviar mensagens para o NODE-RED assim fazendo um fluxo para criar um dashboard com a temperatura do paciente e um status dizendo se ele pode ou não tomar a vacina e o por quê.

# Como utilizar o projeto
Abra o wikwi e cole o arquivo chamado codigo-fonte, em seguida selecione a placa ESP32, selecione tambem o DHT22 e selecione 3 leds (vermelho, amarelo e verde). Depois que todas as 5 peças estiverem no simulador, ligue a entrada dht1:VCC na entrada de 3V do ESP32 e mude a cor do fio para vermelho simulando que está passando energia, logo depois ligue a entrada dht1:SDA na porta digital 15 do ESP32 (altere a cor para facilitar na compreensão) e por último ligue a entrada dht1:GND no GND da ESP32 (altere a cor para preto ou marrom pois simbolizará que está enterrando a eletricidade).

Logo depois de conectar todas as peças do ESP32 com o DHT22 ligaremos os leds, primeiramente ligue o lado positivo do led (onde tem uma "bundinha") com o resistor de 165Ω (ohms) e ligue o resitor com a porta digital D21 (mude a cor do led para vermelha) e do lado negativo ligue o fio preto/marrom no GND da ESP32, com o led amarelo faça os mesmos passos do led vermelho mas ligando o resistor com a porta digital D19 e ao final com o led verde siga os mesmos passos mas ligando o resistor com a porta D18.

Em seguida abra o node-red e coloque o seguinte fluxo:
- MQTT IN com o server test.mosquitto.org utilizando a porta 1883
- Utilize no tópico o seguinte: /indobot/p/temp
- Instale o node-red-dashboard no node-red
- Interligue o MQTT IN com o Gauge
- No Gauge crie um grupo, coloque um label para o dashboard, no range do dash coloque de 0 a 100, no color gradient utilize as seguintes cores (amarelo,verde,vermelho), nos sector utilize de 0 a 35 e depois de 38 a 100
- Utilize outro MQTT IN com o mesmo servidor anterior test.mosquitto.org utilizando a porta 1883
- Utilize o seguinte tópico: /status/temp
- Interligue o MQTT IN com o TEXT
- NO text utlize a seguinte configuração: crie um grupo, coloque "status" no label e no format value insira {{msg.payload}}

E finalmente utilize o teste do nosso projeto!!
