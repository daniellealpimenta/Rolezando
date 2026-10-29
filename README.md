<div align="center">

# 🌤️ Rolezando

**O melhor dia para o seu rolê, decidido pelo clima.**

App iOS que cruza a previsão do tempo dos próximos dias com o tipo de passeio que você quer
fazer (praia, trilha, piquenique, corrida...) e recomenda o dia ideal. Depois é só salvar,
mandar para o Calendário e avisar a turma.

![Swift](https://img.shields.io/badge/Swift-5-F05138?logo=swift&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-0175C2?logo=swift&logoColor=white)
![iOS](https://img.shields.io/badge/iOS-26-000000?logo=apple&logoColor=white)
![WeatherKit](https://img.shields.io/badge/WeatherKit-000000?logo=apple&logoColor=white)
![Apple Intelligence](https://img.shields.io/badge/Foundation%20Models-on--device%20LLM-8E44AD?logo=apple&logoColor=white)
![SwiftData](https://img.shields.io/badge/SwiftData-000000?logo=apple&logoColor=white)
![CloudKit](https://img.shields.io/badge/CloudKit-3693F3?logo=icloud&logoColor=white)
![EventKit](https://img.shields.io/badge/EventKit-FF3B30?logo=apple&logoColor=white)

</div>

<!--
  TODO: adicionar prints em docs/screenshots/ e descomentar.
<table>
  <tr>
    <td width="25%"><img src="docs/screenshots/home.png" alt="Início" /><br/><sub><b>Início</b>: clima da sua cidade</sub></td>
    <td width="25%"><img src="docs/screenshots/atividade.png" alt="Escolha da atividade" /><br/><sub><b>Atividade</b></sub></td>
    <td width="25%"><img src="docs/screenshots/recomendacao.png" alt="Dias recomendados" /><br/><sub><b>Dias recomendados</b></sub></td>
    <td width="25%"><img src="docs/screenshots/detalhe.png" alt="Detalhe do dia" /><br/><sub><b>Detalhe</b>: mensagem gerada no aparelho</sub></td>
  </tr>
</table>
-->

> [!NOTE]
> Projeto em equipe da **Apple Developer Academy | UCB**, focado em frameworks nativos novos:
> WeatherKit e o modelo de linguagem on-device do iOS 26 (Foundation Models).

---

## ✨ O que ele faz

| | |
|---|---|
| 📍 **Clima onde você está** | Localização atual (Core Location) + previsão diária do WeatherKit |
| 🎯 **10 tipos de rolê** | Praia, piquenique, corrida, trilha, ciclismo, fotografia, acampamento, meditação ao ar livre, esportes e passear com o cachorro |
| 📊 **Nota por dia** | Cada atividade tem pesos e faixas ideais de temperatura, chuva, UV e umidade; o app dá uma nota de 0 a 100 a cada dia da previsão |
| 🤖 **Mensagem com IA local** | O modelo de linguagem do próprio aparelho escreve uma mensagem curta sobre o clima do dia, sem enviar nada para servidor |
| 📅 **Mandar para o Calendário** | Cria o evento no app Calendário com EventKit |
| 💾 **Histórico** | Rolês salvos localmente com SwiftData |
| ☁️ **Compartilhado** | Rolês publicados no CloudKit; quem usa o app recebe push quando um novo é adicionado |
| 🚫 **Sem permissão, sem crash** | Tela própria quando a localização é negada |

## 🧮 Como a recomendação funciona

Cada atividade define **pesos** e **faixas** em [`RecommendedDays.swift`](Challenge09/Managers/RecommendedDays.swift):

```swift
case .beachDay:
    WeatherWeights(temp: 0.4, rain: 0.2, uv: 0.3, humidity: 0.1),
    ActivityPreferences(idealTemp: 28...32, maxTemp: 24...36, idealUv: 4...9, ...)
```

Para cada dia da previsão:

1. Cada métrica vira uma nota de 0 a 1: **1** dentro da faixa ideal, caindo até **0** no limite permitido.
2. A chuva usa nota inversa (quanto menos, melhor) e a condição do céu entra como ajuste por atividade.
3. A soma ponderada vira a recomendação de 0 a 100, e os dias são ordenados.

A mensagem do dia usa `LanguageModelSession` com uma *seed* baseada no dia do ano: a frase
fica a mesma durante o dia e muda no seguinte
([`FoundationManager.swift`](Challenge09/Managers/Foundation/FoundationManager.swift)).

## 🧱 Estrutura

```
Challenge09/
├── Managers/
│   ├── WeatherManager.swift      WeatherKit + atribuição obrigatória
│   ├── LocationManager.swift     Core Location
│   ├── RecommendedDays.swift     algoritmo de nota por dia
│   ├── Foundation/               mensagem com o LLM on-device
│   ├── EventKit/                 evento no Calendário
│   ├── CloudKit/                 CRUD no banco público
│   └── PushNotification/         assinatura CKQuerySubscription
├── Models/          atividade, cidade, clima, dia selecionado (@Model)
└── Views/           início, escolher atividade, detalhe, permissão negada, componentes
```

## 🚀 Primeiros passos

```bash
git clone https://github.com/daniellealpimenta/Challenge09.git
open Challenge09/Challenge09.xcodeproj
```

1. Em *Signing & Capabilities*, escolha um time com **WeatherKit** e **iCloud (CloudKit)**
   habilitados no portal da Apple Developer.
2. Rode num aparelho com **iOS 26** (a mensagem com IA precisa de um aparelho com Apple
   Intelligence; sem ele, o app mostra o texto padrão).

## 👥 Time

[**Daniel Leal Pimenta**](https://github.com/daniellealpimenta),
[Filipi Romão](https://github.com/FilipiNyetz),
[Ana Luisa Coleone](https://github.com/isacoleone),
[@WiseJao](https://github.com/WiseJao) e
[@rebecamariamg](https://github.com/rebecamariamg).

## 🗺️ Roadmap

- [x] Recomendação por atividade com WeatherKit
- [x] Mensagem gerada com Foundation Models
- [x] Calendário, SwiftData, CloudKit e notificações
- [ ] Prints e vídeo curto neste README
- [ ] Testes de unidade para o algoritmo de recomendação
- [ ] Widget com o melhor dia da semana
