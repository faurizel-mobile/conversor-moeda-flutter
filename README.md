# Conversor de Moedas Flutter

Aplicativo mobile em Flutter para converter valores entre Reais, Dólares e Euros em tempo real, utilizando uma API de câmbio externa.

## Visão Geral

Este projeto foi desenvolvido como exemplo prático de utilização de:

- Widgets do Flutter
- Estado local com `StatefulWidget`
- Requisições HTTP
- Conversão e formatação de valores
- Tratamento de carregamento e erro assíncrono

A aplicação permite que o usuário informe um valor em uma moeda e veja automaticamente a conversão nas outras moedas.

## Funcionalidades

- Conversão entre:
  - Real (R$)
  - Dólar (US$)
  - Euro (€)
- Atualização em tempo real ao digitar o valor
- Consumo de API para buscar as taxas de câmbio
- Estado de carregamento durante a consulta
- Mensagem de erro quando a API não responde
- Interface simples e intuitiva em Material Design

## Tecnologias Utilizadas

- Flutter
- Dart
- HTTP
- Material Design

## Estrutura do Projeto

```bash
conversor-moeda-flutter/
├── lib/
│   ├── app/
│   │   └── tela_conversor.dart
│   └── main.dart
├── android/
├── ios/
├── test/
├── pubspec.yaml
├── README.md
└── GUIA_PEDAGOGICO.md
```

## Pré-requisitos

Antes de iniciar, certifique-se de ter instalado:

- Flutter SDK
- Dart SDK
- Android Studio ou VS Code
- Emulador Android ou dispositivo físico
- Acesso à internet

## Instalação

1. Clone o repositório:

```bash
git clone https://github.com/faurizel-mobile/conversor-moeda-flutter.git
cd conversor-moeda-flutter
```

2. Instale as dependências:

```bash
flutter pub get
```

3. Execute o app:

```bash
flutter run
```

## Como funciona

A aplicação realiza uma requisição HTTP para a API de câmbio da HG Brasil e, em seguida, extrai os valores das moedas USD e EUR para realizar as conversões.

O fluxo principal é:

1. Carrega os dados da API
2. Exibe tela de carregamento enquanto a resposta chega
3. Se a requisição for bem-sucedida, mostra os campos de entrada
4. Ao digitar um valor, o app recalcula automaticamente os outros campos

## Exemplo de API utilizada

A requisição atual do projeto é semelhante a esta:

```dart
const request = "https://api.hgbrasil.com/finance?key=d18b57f7";
```

A resposta da API contém as taxas de câmbio em formato JSON, sendo acessadas com `json.decode(...)`.

## Observações

- Este projeto serve como base para aprender Flutter do zero.
- A aplicação foi escrita em um estilo didático para facilitar o entendimento de conceitos como `StatefulWidget`, `FutureBuilder`, `TextEditingController` e consumo de APIs.
- Para fins de estudo, a documentação do projeto também está disponível em `GUIA_PEDAGOGICO.md`.

## Próximos passos sugeridos

- Adicionar mais moedas
- Exibir as taxas atuais na tela
- Salvar o último valor convertido com `SharedPreferences`
- Criar histórico de conversões
- Implementar tema claro/escuro
- Adicionar testes unitários

## Autor

Projeto desenvolvido para fins educacionais e de aprendizado em Flutter.
