
<h1>Cronômetro de Corrida com Sensor de Ultrassom e Display LCD</h1>
<p>Este repositório contém um código Arduino para um cronômetro de corrida que utiliza um sensor de ultrassom para iniciar e finalizar a contagem do tempo, exibindo o resultado em um display LCD. O sistema é projetado para detectar a presença de um corredor, iniciando a contagem quando ele começa a correr e parando a contagem quando ele termina a corrida.</p>
<h2>Integrantes do grupo</h2>
<ul>
    <li>Rickelmyn de Souza Ruescas (RM556055)</li>
    <li>Fabrini Conte Araujo Soares (RM557813)</li>
    <li>Guilherme Cezarino Simoes (RM557724)</li>
    <li>Vinicius dos Santos Wince (RM557033)</li>
</ul>

<h2>Código do projeto</h2>
<pre>
<code>
#include &lt;Wire.h&gt;
#include &lt;LiquidCrystal_I2C.h&gt;
const int trigPin = 9;
const int echoPin = 10;

long duration;
int distance;
boolean raceStarted = false;
unsigned long startTime;
unsigned long endTime;
boolean runnerFinished = false;

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
// Configuração dos pinos do sensor de ultrassom
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);

// Inicialização da tela LCD
lcd.init();
lcd.backlight();
lcd.setCursor(0, 0);
lcd.print("Pronto para");
lcd.setCursor(0, 1);
lcd.print("iniciar a corrida");

// Inicialização da comunicação serial
Serial.begin(9600);
}

void loop() {
// Verifica se a corrida ainda não começou
if (!raceStarted) {
distance = measureDistance(trigPin, echoPin);
// Inicia a corrida se a distância for menor que 10 cm
if (distance < 10) {
raceStarted = true;
startTime = millis(); // Registra o tempo de início
lcd.clear();
lcd.setCursor(0, 0);
lcd.print("Corrida Iniciada!");
delay(1000); // Anti-rebote
}
}

// Verifica se a corrida começou
if (raceStarted) {
// Verifica se o corredor não terminou a corrida
if (!runnerFinished) {
distance = measureDistance(trigPin, echoPin);
// Termina a corrida se a distância for menor que 10 cm
if (distance < 10) {
endTime = millis(); // Registra o tempo de fim
runnerFinished = true;
lcd.setCursor(0, 1);
lcd.print("Tempo: ");
lcd.print(endTime - startTime); // Exibe o tempo da corrida
lcd.print(" ms");
delay(1000); // Anti-rebote
}
}
// Se o corredor terminou a corrida
if (runnerFinished) {
raceStarted = false;
runnerFinished = false;
Serial.print("Tempo de corrida: ");
Serial.print(endTime - startTime);
Serial.println(" ms");

// Prepara a tela LCD para a próxima corrida
  lcd.setCursor(0, 0);
  lcd.print("Pronto para");
  lcd.setCursor(0, 1);
  lcd.print("iniciar a corrida");
}
}
}

// Função para medir a distância com o sensor de ultrassom
int measureDistance(int trigPin, int echoPin) {
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

long duration = pulseIn(echoPin, HIGH);
int distance = duration * 0.034 / 2;
return distance;
}
</code>
</pre>

<h2>Estrutura do Código</h2>
<p>O código está organizado para lidar com a medição da distância através do sensor de ultrassom, iniciar e parar a contagem do tempo da corrida, e exibir o resultado no display LCD.</p>
<h3>Lista de Funções</h3>
<ul>
    <li><strong>setup()</strong>
        <ul>
            <li>Configura os pinos do sensor de ultrassom.</li>
            <li>Inicializa a tela LCD e a comunicação serial.</li>
        </ul>
    </li>
    <li><strong>loop()</strong>
        <ul>
            <li>Verifica se a corrida ainda não começou e inicia a contagem do tempo quando detecta o corredor.</li>
            <li>Verifica se a corrida começou e monitora a chegada do corredor para parar a contagem do tempo.</li>
            <li>Exibe o tempo da corrida no display LCD e no monitor serial.</li>
        </ul>
    </li>
    <li><strong>measureDistance(int trigPin, int echoPin)</strong>
        <ul>
            <li>Mede a distância utilizando o sensor de ultrassom.</li>
            <li>Retorna a distância medida.</li>
        </ul>
    </li>
</ul>
<h2>Requisitos do Sistema</h2>
<p>Para rodar a aplicação, são necessários os seguintes requisitos do sistema:</p>
<ul>
    <li><strong>Sistema Operacional:</strong> Windows 7 ou superior, macOS 10.13 ou superior, ou distribuições Linux modernas.</li>
    <li><strong>Arduino IDE:</strong> Versão mais recente.</li>
    <li><strong>Espaço em Disco:</strong> Pelo menos 50 MB de espaço livre.</li>
    <li><strong>Memória RAM:</strong> Pelo menos 1 GB de RAM.</li>
    <li><strong>Outros Requisitos:</strong> Acesso à internet para baixar a Arduino IDE e outras dependências, se necessário.</li>
</ul>
<h2>Execução</h2>
<h3>Execução no Arduino IDE</h3>
<p>Para executar o código utilizando a Arduino IDE, siga os passos abaixo:</p>
<ol>
    <li>Abra a Arduino IDE.</li>
    <li>Baixe o código do repositório e salve-o em um diretório de sua escolha.</li>
    <li>Na Arduino IDE, clique em <strong>File</strong> > <strong>Open</strong> e navegue até o diretório onde o código foi salvo. Abra o arquivo do código.</li>
    <li>Conecte seu Arduino ao computador.</li>
    <li>Selecione a placa e a porta correta na Arduino IDE.</li>
    <li>Clique no botão "Upload" para carregar o código no Arduino.</li>
    <li>Abra o Monitor Serial para ver os resultados da corrida.</li>
</ol>
<h3>Execução no Wokwi</h3>
<p>Para executar o código utilizando o simulador Wokwi, siga os passos abaixo:</p>
<ol>
    <li>Visite o site do Wokwi: <a href="https://wokwi.com/">https://wokwi.com/</a></li>
    <li>Clique em "Start new project" (ou "Iniciar novo projeto").</li>
    <li>Escolha o tipo de projeto "Arduino" e clique em "Create" (ou "Criar").</li>
    <li>No editor de código, copie e cole o código fornecido acima.</li>
    <li>Configure os componentes necessários no simulador (sensor de ultrassom HC-SR04 e display LCD I2C):
        <ul>
            <li>Adicione um sensor de ultrassom (HC-SR04) à área de trabalho e conecte os pinos conforme especificado no código:
                <ul>
                    <li>VCC -> 5V no Arduino</li>
                    <li>GND -> GND no Arduino</li>
                    <li>Trig -> Pino digital 9 no Arduino</li>
                    <li>Echo -> Pino digital 10 no Arduino</li>
                </ul>
            </li>
            <li>Adicione um display LCD I2C à área de trabalho e conecte os pinos conforme especificado no código:
                <ul>
                    <li>VCC -> 5V no Arduino</li>
                    <li>GND -> GND no Arduino</li>
                    <li>SDA -> SDA no Arduino</li>
                    <li>SCL -> SCL no Arduino</li>
                </ul>
            </li>
        </ul>
    </li>
    <li>Clique no botão "Start Simulation" (ou "Iniciar Simulação") para executar o código no simulador.</li>
    <li>Observe os resultados no display LCD e no console serial do Wokwi.</li>
</ol>
<h3>Exemplo de Uso</h3>
<ol>
    <li>O sistema estará pronto para iniciar a corrida quando o display LCD mostrar "Pronto para iniciar a corrida".</li>
    <li>Quando o corredor passar em frente ao sensor de ultrassom, a contagem do tempo será iniciada.</li>
    <li>
    <h2>Simulador no Wokwi</h2>
<a href="https://wokwi.com/projects/400955742313861121">https://wokwi.com/projects/400955742313861121</a>
