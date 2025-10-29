# Projeto: Semáforo 2 Caua Pirilo Asquino


##  Descrição do Projeto

Este projeto implementa um semáforo utilizando Arduino, onde um botão controla a execução contínua da sequência de LEDs (verde, amarelo e vermelho) de forma alternada (liga/desliga). O código utiliza **ponteiros** para manipular a classe do semáforo e emprega lógica não bloqueante com millis() para permitir a interrupção imediata do loop.

---

##  Código Utilizado

```cpp
class Semaforo {
  private:
    int pinoVermelho, pinoAmarelo, pinoVerde;

  public:
    Semaforo(int vermelho, int amarelo, int verde) {
      pinoVermelho = vermelho;
      pinoAmarelo = amarelo;
      pinoVerde = verde;
    }

    void iniciar() {
      pinMode(pinoVermelho, OUTPUT);
      pinMode(pinoAmarelo, OUTPUT);
      pinMode(pinoVerde, OUTPUT);
      apagarTudo();
    }

    void apagarTudo() {
      digitalWrite(pinoVermelho, LOW);
      digitalWrite(pinoAmarelo, LOW);
      digitalWrite(pinoVerde, LOW);
    }

    void ligarVermelho()  { digitalWrite(pinoVermelho, HIGH); }
    void ligarAmarelo()   { digitalWrite(pinoAmarelo, HIGH); }
    void ligarVerde()     { digitalWrite(pinoVerde, HIGH); }
};

Semaforo* semaforo = new Semaforo(3, 2, 4);  // Uso do ponteiro

const int pinoBotao = 5;
bool emLoop = false;
bool estadoAnteriorBotao = HIGH;
unsigned long tempoAnterior = 0;
int estado = 0;

void setup() {
  pinMode(pinoBotao, INPUT_PULLUP);
  semaforo->iniciar();
}

void loop() {
  bool estadoAtualBotao = digitalRead(pinoBotao);

  // Botão com função toggle
  if (estadoAtualBotao == LOW && estadoAnteriorBotao == HIGH) {
    emLoop = !emLoop;
    if (!emLoop) {
      semaforo->apagarTudo();
      estado = 0;
    }
    delay(200); 
  }
  estadoAnteriorBotao = estadoAtualBotao;

  if (emLoop) {
    unsigned long agora = millis();

    switch (estado) {
      case 0:
        semaforo->apagarTudo();
        semaforo->ligarVerde();
        tempoAnterior = agora;
        estado = 1;
        break;

      case 1:
        if (agora - tempoAnterior >= 4000) estado = 2;
        break;

      case 2:
        semaforo->apagarTudo();
        semaforo->ligarAmarelo();
        tempoAnterior = agora;
        estado = 3;
        break;

      case 3:
        if (agora - tempoAnterior >= 2000) estado = 4;
        break;

      case 4:
        semaforo->apagarTudo();
        semaforo->ligarVermelho();
        tempoAnterior = agora;
        estado = 5;
        break;

      case 5:
        if (agora - tempoAnterior >= 6000) estado = 0;
        break;
    }
  }
}

```

---

##  Vídeo de Funcionamento


<video width="560" height="315" controls>
  <source src="https://raw.githubusercontent.com/c4uazinnnn/Ponderada-Arduino-Semaforo-2/main/Video semaforo.mp4" type="video/mp4">
  Seu navegador não suporta vídeo HTML5. <a href="Video_semaforo.mp4">Baixar o vídeo</a>
</video>


---

## ✅ Conclusão

Este projeto demonstra o uso de orientação a objetos com ponteiros em Arduino e controle lógico por botão utilizando programação não bloqueante. A implementação garante maior responsividade e controle total sobre o ciclo do semáforo em tempo real.

---

