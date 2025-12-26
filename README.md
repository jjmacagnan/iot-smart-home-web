# 🔌 Simulador IoT - Smart Home

**Descrição**

Projeto front-end simples que simula um dispositivo IoT (edge) para uma **Smart Home**. A interface (arquivo `index.html`) gera leituras de sensores (Temperatura, Umidade e Luminosidade) e permite controlar atuadores (Ventilador e Iluminação) via a API REST do **Firebase Realtime Database**.

---

## 🚀 Funcionalidades principais

- Simulação de sensores: DHT22 (temperatura e umidade) e LDR (luminosidade)
- Modos de sensor: **AUTO** (valores aleatórios em intervalo configurável), **MANUAL** (valores informados pelo usuário) e **READ** (apenas leitura do Firebase)
- Controle de atuadores (ventilador e luz) com modo manual e modo automático (baseado em thresholds salvos no Firebase)
- Exibição de logs de eventos na interface
- Comunicação via REST com o Realtime Database do Firebase (PUT/GET em endpoints `.json`)

---

## ⚙️ Requisitos

- Navegador moderno (Chrome, Firefox, Edge, Safari)
- Acesso à internet para comunicar com o Firebase Realtime Database
- Conta no Firebase com um Realtime Database configurado

---

## ▶️ Como usar

1. Clone ou baixe este repositório.

2. Abra o arquivo `index.html` no navegador (basta dar duplo-clique ou servir via um servidor estático).

3. No campo **Firebase URL** informe a URL do seu Realtime Database (ex.: `https://seu-projeto.firebaseio.com`). No campo **Device ID** informe um identificador (ex.: `device_001`).

4. Clique em **Conectar ao Firebase**.

5. Configure o **Modo dos Sensores**:
   - `Automático`: envia leituras simuladas periodicamente (configurar intervalo em ms)
   - `Manual`: permite inserir valores e enviar
   - `Apenas leitura`: apenas lê valores existentes no Firebase

6. Controle os atuadores (Ventilador / Iluminação) usando os switches — as mudanças são gravadas no nó `devices/<deviceId>/actuators` no Firebase.

## 🔁 Estrutura esperada no Firebase

```json
{
  "devices": {
    "device_001": {
      "name": "Sala de Estar",
      "location": "Casa",
      "status": "offline",
      "lastUpdate": 0,
      "sensors": {
        "temperature": {
          "value": 22,
          "unit": "°C",
          "timestamp": 0
        },
        "humidity": {
          "value": 60,
          "unit": "%",
          "timestamp": 0
        },
        "light": {
          "value": 400,
          "unit": "lux",
          "timestamp": 0
        }
      },
      "actuators": {
        "fan": {
          "state": false,
          "mode": "manual"
        },
        "light": {
          "state": false,
          "mode": "manual"
        }
      },
      "settings": {
        "tempThreshold": 26,
        "lightThreshold": 300,
        "autoMode": true
      }
    }
  }
}
```
> Observação: Para testes rápidos, ajuste as regras do Realtime Database para permitir leitura/escrita (não recomendado em produção). Exemplo mínimo de regra para testes:
>
> {
>   "rules": {
>     ".read": true,
>     ".write": true
>   }
> }

---

## 📁 Estrutura do projeto

- `index.html` — aplicação principal (UI + lógica em vanilla JS)

---

## 💡 Dicas de desenvolvimento

- O envio/recebimento para o Firebase é feito pelo método REST: `https://<your-db>.firebaseio.com/devices/<deviceId>/*.json` (método PUT/GET).
- Valores e thresholds iniciais são criados quando você clica em **Conectar ao Firebase** (veja função `initializeDevice()` no JS).
- Para ajustar intervalos, limites ou comportamentos automáticos edite diretamente o `index.html`.