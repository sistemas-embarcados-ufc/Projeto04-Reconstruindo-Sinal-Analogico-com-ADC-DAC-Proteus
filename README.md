# 🔄 Projeto 04 – Conversão AD/DA: Impacto da Taxa de Amostragem

Este projeto simula no Proteus um pipeline de conversão **Analógico-Digital (ADC)** e **Digital-Analógica (DAC)**. O objetivo principal é analisar, de forma visual, como a **taxa de amostragem** (`fₛ`) influencia a fidelidade da reconstrução do sinal analógico, evidenciando na prática os efeitos da **quantização** e os limites impostos pelo **Teorema da Amostragem de Nyquist-Shannon**.

---

## ⚙️ Arquitetura do Circuito

<p align="center">
<img src="esquema-circuito.png" width="800px">
</p>

### 🔧 Componentes e Topologia:

* **CIs utilizados**: `ADC0804` (Conversor AD de 8 bits), `DAC0800` (Conversor DA por corrente), `LM741` (Amplificadores operacionais).
* **Alimentação**:

  * +12V para circuitos analógicos (DAC e amplificadores);
  * +5V para circuitos digitais (ADC e controle).
* **Fonte de Sinal de Entrada**:

  * **Variável**: Sensor LDR com iluminação variável (tensão analógica dinâmica).
  * **Ideal**: Gerador senoidal (1 Hz, 2 Vpp, offset de 2.5 V).
* **Clock de Amostragem (`fₛ`)**: Definido via gerador `DCLOCK`, aplicado ao pino `WR` do `ADC0804`. Frequências testadas: **5 Hz**, **30 Hz** e **100 Hz**.
* **Observação dos sinais**: Saída conectada ao osciloscópio de dois canais para análise comparativa entre sinal original e reconstruído.

---

### ⛓️ Etapas Funcionais da Conversão

1. **Condicionamento do Sinal de Entrada**
   Sinal analógico é amplificado por um amplificador não-inversor para se adequar à faixa operacional de 0–5 V do ADC.

2. **Conversão Analógico-Digital (ADC0804)**
   O sinal é digitalizado em resolução de 8 bits sob controle externo (clock assíncrono via `WR`). O ADC opera em modo de conversão contínua com disparos periódicos.

3. **Conversão Digital-Analógica (DAC0800)**
   Os 8 bits são convertidos em corrente proporcional à magnitude binária. O DAC opera com corrente de saída diferencial.

4. **Conversão Corrente-Tensão (I/V)**
   Um segundo amplificador operacional realiza a conversão da corrente do DAC para uma tensão proporcional, recuperando o sinal analógico reconstruído.

---

## 🧪 Procedimento de Simulação no Proteus

1. Abra o arquivo `.pdsprj` no Proteus.
2. Configure o `DCLOCK` para as taxas desejadas: 5 Hz, 30 Hz ou 100 Hz.
3. Execute a simulação (▶️).
4. Monitore o osciloscópio:

   * **Canal A (Amarelo)**: Sinal analógico amplificado (entrada).
   * **Canal B (Azul)**: Sinal após conversão DA (reconstruído).
5. Para testes com senóide, substitua o LDR por um gerador `SINE`.

---

## 📊 Análise da Resposta em Frequência

A seguir, são apresentadas capturas do osciloscópio para diferentes valores de `fₛ`, com entrada analógica do LDR.

### Amostragem a 5 Hz

<p align="center">
<img src="amostragem-5hz-osciloscopio.png" width="500px">
</p>

* A amostragem grosseira resulta em uma reconstrução com baixa fidelidade. O sistema perde informações entre amostras, gerando distorção significativa no sinal.

### Amostragem a 30 Hz

<p align="center">
<img src="amostragem-30hz-osciloscopio.png" width="500px">
</p>

* Com maior `fₛ`, a discretização é mais densa, reduzindo o erro de reconstrução. O sinal reconstruído já apresenta forma semelhante à entrada original.

### Amostragem a 100 Hz

<p align="center">
<img src="amostragem-100hz-osciloscopio.png" width="500px">
</p>

* Alta taxa de amostragem gera um sinal com fidelidade elevada. A reconstrução mostra conformidade com o sinal original, validando o Teorema de Nyquist-Shannon.

---

## 🧠 Considerações sobre Quantização

### Sinal Senoidal Ideal com Amostragem a 100 Hz

<p align="center">
<img src="amostragem-100hz-senoidal-osciloscopio.png" width="500px">
</p>

Mesmo sob alta taxa de amostragem, o sinal reconstruído exibe artefatos de **quantização**: pequenos "degraus" resultantes da resolução finita de 8 bits (256 níveis discretos de tensão). A precisão da conversão está limitada não pela taxa de amostragem, mas pela **resolução de quantização**. Para aproximações mais suaves, seria necessário aumentar o número de bits da conversão.
