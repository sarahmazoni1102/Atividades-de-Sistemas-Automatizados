<h1 align ="center"> 🎛️ Sistema de Controle de Tanque de Água </h1>

<div align="center">

![UNIVERSIDADE](https://img.shields.io/badge/UNIVERSIDADE-USJT-blue)
![PROJETO](https://img.shields.io/badge/PROJETO-A3-purple)
![STATUS](https://img.shields.io/badge/STATUS-Concluído-green)


</div>

Este repositório contém a modelagem e implementação da lógica de controle para um sistema automatizado de armazenamento e gerenciamento de nível de um tanque de água. O projeto e aplicado em Controladores Lógicos Programáveis (CLPs) industriais.

O sistema conta com modos de operação automático e manual, controle por histerese, temporizadores de proteção contra trabalho a seco e transbordo, além de uma rotina dedicada de reset e sinalização visual completa.

---

## 🔬 Simulador Online

A lógica deste projeto está totalmente montada e pronta para ser testada interativamente de forma online. 

👉 **[Clique aqui para acessar o Projeto no PLCFiddle.](https://www.plcfiddle.com:/fiddles/a86091b2-6d96-4077-a774-39b4aaa13de0)**

### Como interagir na plataforma:
1. Abra o link acima.
2. Na aba lateral ou no topo (variando com o layout do site), clique nas variáveis de entrada (**Variables**) para forçar o estado delas (`true`/`false`).
3. Observe os blocos de temporização (TON) e contadores (CTU) incrementando em tempo real conforme as condições das linhas (*Rungs*) são atendidas.

---

## 🔌 Mapeamento de Variáveis

O mapeamento das Entradas (I), Saídas (Q), Memórias Internas (M), Temporizadores (T) e Contadores (C) segue o padrão industrial configurado no simulador:

### Entradas Físicas (Inputs)
| Identificador | Variável | Descrição |
| :---: | :--- | :--- |
| **I0** | `COMEÇAR` | Botão de pulso para iniciar o sistema / acionar bomba manual |
| **I1** | `PARAR` | Botão de emergência/parada do sistema (NF) |
| **I2** | `MODO_AUTOMATICO` | Chave seletora para o modo automático |
| **I3** | `MODO_MANUAL` | Chave seletora para o modo manual |
| **I4** | `SENSORNIVEL_BAIXO` | Sensor indicador de nível baixo atingido |
| **I5** | `SENSORNIVEL_MEDIO` | Sensor indicador de nível médio atingido |
| **I6** | `SENSORNIVEL_ALTO` | Sensor indicador de nível alto atingido |
| **I7** | `SENSORNIVEL_MAXIMO` | Sensor crítico de transbordo (Emergência) |
| **I8** | `RESET` | Botão para limpar alarmes e reiniciar contadores |

### Saídas Físicas (Outputs)
| Identificador | Variável | Descrição |
| :---: | :--- | :--- |
| **Q0** | `BOMBA` | Atuador do motor da bomba d'água |
| **Q1** | `ALARME` | Sirene física de alerta de falha |
| **Q2** | `LED_SISTEMA_LIGADO` | Sinalização visual de painel energizado |
| **Q3** | `LED_BOMBA_LIGADO` | Sinalização visual de bomba em operação |
| **Q4** | `VÁLVULA_DRENAGEM` | Atuador da válvula de escoamento de emergência |
| **Q5** | `FAROL_VERDE_BAIXO` | Presença de água no nível baixo (Seguro) |
| **Q6** | `FAROL_VERDE_MEDIO` | Presença de água no nível médio (Seguro) |
| **Q7** | `FAROL_LARANJA_ALTO` | Alerta de tanque quase cheio |
| **Q8** | `FAROL_VERMELHO_MAX` | Alerta crítico de transbordo em iminência |

### Elementos Internos (Memórias, Timers e Contadores)
* **M0**: `MEMORIA_SL` — Retenção de Sistema Ligado.
* **M1**: `MEMORIA_MA` — Status do Modo Automático ativo.
* **M2**: `MEMORIA_MM` — Status do Modo Manual ativo.
* **M3**: `MEMORIA_CB` — Comando Interno de Bomba Ativa (Lógica Histerese).
* **M4**: `MEMORIA_AL` — Alarme travado por falha/transbordo.
* **M5**: `MEMORIA_RR` — Intervém na rotina de esvaziamento seguro via Reset.
* **T0**: `TEMP_Proteção` — Timer de segurança contra funcionamento a seco (**Preset: 8s**).
* **T1**: `TEMP_Max` — Timer de limite de tempo de enchimento contínuo (**Preset: 90s**).
* **C0**: `Contador_CicloEnchimento` — Contador de anomalias/ciclos executados (**Preset: 999**).

---

## ⚙️ Arquitetura e Funcionamento

1.  **Segurança e Intertravamento:** O sistema só opera se o painel geral for iniciado (`I0`) e a memória `M0` estiver ativa. O botão `I1` corta imediatamente toda a operação.
2.  **Controle Automático (Histerese):** Quando em modo automático (`M1`), a bomba liga ao detectar que o nível está abaixo de `I4` (Sensor Baixo desativado) e permanece selada/ligada até atingir o Sensor Alto (`I6`).
3.  **Proteções Temporizadas:**
- Se a bomba (`Q0`) ligar e em até **8 segundos** (`T0`) a água não atingir os níveis mínimos esperados, o sistema assume funcionamento a seco e interrompe a bomba.
- Se a bomba ficar ativa por mais de **90 segundos** (`T1`) direto, o sistema acusa falha de tempo limite (possível vazamento ou sensor travado).
4.  **Tratamento de Anomalias (Reset e Drenagem):** Ao atingir o nível máximo crítico (`I7`), o alarme (`M4`) trava e a válvula de drenagem (`Q4`) abre. O operador precisa pressionar `RESET` (`I8`), disparando uma rotina (`M5`) que mantém a drenagem aberta até que o nível caia com segurança abaixo do sensor baixo (`I4`).

---

## 📋 Diagrama do Tanque

<img width="1871" height="738" alt="image" src="https://github.com/user-attachments/assets/d8c82fb4-62b9-4104-8fd3-48aa7f7c54b8" />

<img width="1323" height="655" alt="image" src="https://github.com/user-attachments/assets/f3ee9cdc-6b0d-4d73-9136-6849eb1cd4b1" />

---

## 👨‍💻 Autores

🎓 Anderson Rugo Santos - GitHub: https://github.com/anderson-r-s

🎓 Jhonathan Guedes Nantes - GitHub: https://github.com/JhowNantes

🎓 Lucas Gonçalves Nascimento - GitHub: https://github.com/lucasgnascimento-hub

🎓 Matheus Dias Pereira - GitHub: https://github.com/matheusdiasz2007 

🎓 Marcus V. Araujo Tavares - GitHub: https://github.com/Marcus-Tavares 

🎓 Sara Hakim dos Santos - GitHub: https://github.com/hakim-sara 

🎓 Sarah Mazoni Camargo - GItHub: https://github.com/sarahmazoni1102 

---