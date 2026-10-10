# 📱 Guia Pedagógico: Conversor de Moedas Flutter

**Um projeto prático para aprender Flutter do zero!**

---

## 📚 Sumário

1. [Introdução](#introdução)
2. [Pré-requisitos](#pré-requisitos)
3. [Configuração do Ambiente](#configuração-do-ambiente)
4. [Entendendo a Arquitetura](#entendendo-a-arquitetura)
5. [Explicação do Código](#explicação-do-código)
6. [Execução do Projeto](#execução-do-projeto)
7. [Conceitos-Chave Aprendidos](#conceitos-chave-aprendidos)
8. [Desafios e Extensões](#desafios-e-extensões)

---

## 🎯 Introdução

Este é um aplicativo de conversor de moedas desenvolvido em **Flutter**. Ele demonstra:

- ✅ Requisições HTTP para APIs externas
- ✅ Manipulação de dados JSON
- ✅ Widgets com estado mutável (StatefulWidget)
- ✅ Padrões de UI responsiva
- ✅ Tratamento de carregamento e erros assíncrono
- ✅ Validação e formatação de números

**Objetivo**: Converter valores entre Reais (R$), Dólares (US$) e Euros (€) em tempo real.

---

## 📋 Pré-requisitos

Antes de começar, você deve ter:

### Conhecimentos Necessários:
- ✓ Noções básicas de programação (variáveis, funções, condicionais)
- ✓ Familiaridade com linguagem Dart (conceitos básicos)
- ✓ Entendimento de widgets e UI em Flutter

### Ferramentas Necessárias:
- ✓ Flutter SDK instalado (versão 2.0+)
- ✓ Dart SDK (vem com Flutter)
- ✓ Android Studio OU Visual Studio Code
- ✓ Emulador Android OU dispositivo físico
- ✓ Conexão com a internet

---

## 🔧 Configuração do Ambiente

### Passo 1: Verificar Instalação do Flutter

```bash
flutter --version
dart --version
```

Se não tiver Flutter instalado, acesse: https://flutter.dev/docs/get-started/install

### Passo 2: Clonar o Repositório

```bash
git clone https://github.com/faurizel-mobile/conversor-moeda-flutter.git
cd conversor-moeda-flutter
```

### Passo 3: Instalar Dependências

```bash
flutter pub get
```

Isso instalará os pacotes definidos em `pubspec.yaml`:
- **flutter**: Framework principal
- **cupertino_icons**: Ícones iOS
- **http**: Para requisições HTTP
- **async**: Utilitários para programação assíncrona

### Passo 4: Configurar Dispositivo

**Opção A - Emulador Android:**
```bash
flutter emulators --launch <emulator_id>
```

**Opção B - Dispositivo Físico:**
- Conecte o celular via USB
- Ative o "Modo de Desenvolvedor" (Developer Mode)
- Execute: `flutter devices` para verificar conexão

### Passo 5: Executar o Projeto

```bash
flutter run
```

---

## 🏗️ Entendendo a Arquitetura

```
conversor-moeda-flutter/
├── lib/
│   ├── main.dart                 # Ponto de entrada do app
│   └── app/
│       └── tela_conversor.dart   # Tela principal (UI + Lógica)
├── pubspec.yaml                   # Dependências do projeto
└── android/, ios/                # Código nativo
```

### Fluxo da Aplicação

```
┌─────────────────────────────────────────────────────┐
│         main.dart (Inicialização)                   │
│  - Carrega MaterialApp com tema                     │
│  - Define a tela inicial (Home)                     │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│    tela_conversor.dart (Home - StatefulWidget)      │
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │ 1. FutureBuilder (Carregar dados da API)    │  │
│  │    - Chamada: getData()                      │  │
│  │    - Estados: loading, error, success        │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │ 2. TextFields (Entrada de dados)             │  │
│  │    - onChanged: _realChanged()               │  │
│  │    - onChanged: _dolarChanged()              │  │
│  │    - onChanged: _euroChanged()               │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │ 3. Cálculos em tempo real                   │  │
│  │    - Conversão automática entre moedas      │  │
│  │    - Atualização dos controllers            │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 💻 Explicação do Código

### 1️⃣ `main.dart` - Ponto de Entrada

```dart
import 'package:flutter/material.dart';
import 'app/tela_conversor.dart';

void main() async {
  runApp(MaterialApp(
    home: Home(),
    theme: ThemeData(
      hintColor: Colors.amber,
      primaryColor: Colors.white
    ),
  ));
}
```

**O que acontece aqui:**

| Linha | Explicação |
|-------|-----------|
| `import 'package:flutter/material.dart'` | Importa componentes Material Design |
| `import 'app/tela_conversor.dart'` | Importa a tela principal do app |
| `void main() async` | Função de entrada; `async` permite esperar operações |
| `runApp()` | Inicializa o Flutter |
| `MaterialApp()` | Widget raiz com Material Design |
| `home: Home()` | Define a tela inicial |
| `ThemeData()` | Customiza cores (amber e white) |

---

### 2️⃣ `tela_conversor.dart` - Tela Principal

#### Estrutura Geral

```dart
class Home extends StatefulWidget {
  @override
  _HomeState createState() => _HomeState();
}

class _HomeState extends State<Home> {
  // Código aqui
}
```

**Por que StatefulWidget?**
- O app precisa atualizar a UI quando o usuário digita
- `setState()` reconstrói a interface com novos valores
- Diferente de `StatelessWidget` (imutável)

---

#### Controladores e Variáveis

```dart
final realController = TextEditingController();
final dolarController = TextEditingController();
final euroController = TextEditingController();

double dolar;  // Taxa de câmbio
double euro;   // Taxa de câmbio
```

**Explicação:**
- `TextEditingController`: Gerencia o texto dos campos
- `dolar` e `euro`: Armazenam as taxas de câmbio atuais
- `final`: Não podem ser reatribuídos (mas seu conteúdo muda)

---

#### Funções de Conversão

```dart
void _realChanged(String text) {
  double real = double.parse(text);
  dolarController.text = (real / dolar).toStringAsFixed(2);
  euroController.text = (real / euro).toStringAsFixed(2);
}
```

**Como funciona:**
1. Usuário digita no campo de Reais
2. `_realChanged()` é acionada
3. Converte o texto para número
4. Calcula: dólares = reais ÷ taxa_dolar
5. Atualiza os outros campos automaticamente
6. `.toStringAsFixed(2)`: Formata com 2 casas decimais

**Exemplos de uso:**

| Entrada | Cálculo | Resultado |
|---------|---------|-----------|
| R$ 100 | 100 ÷ 5.0 | US$ 20.00 |
| R$ 50 | 50 ÷ 5.5 | € 9.09 |

---

#### Requisição HTTP

```dart
const request = "https://api.hgbrasil.com/finance?key=d18b57f7";

Future<Map> getData() async {
  http.Response response = await http.get(request);
  return json.decode(response.body);
}
```

**Fluxo:**
1. `Future<Map>`: Retorna um mapa no futuro (assíncrono)
2. `await http.get()`: Espera a resposta da API
3. `json.decode()`: Converte JSON em Map
4. Resposta típica:
```json
{
  "results": {
    "currencies": {
      "USD": { "buy": 5.12 },
      "EUR": { "buy": 5.85 }
    }
  }
}
```

---

#### Build com FutureBuilder

```dart
FutureBuilder<Map>(
  future: getData(),
  builder: (context, snapshot) {
    switch (snapshot.connectionState) {
      case ConnectionState.none:
      case ConnectionState.waiting:
        return Center(
          child: Text("Carregando dados...",
            style: TextStyle(color: Colors.amber, fontSize: 25.0),
          ),
        );
      default:
        if (snapshot.hasError) {
          return Center(
            child: Text("Erro ao Carregar Dados :(",
              style: TextStyle(color: Colors.amber, fontSize: 25.0),
            ),
          );
        } else {
          // Extrai as taxas
          dolar = snapshot.data["results"]["currencies"]["USD"]["buy"];
          euro = snapshot.data["results"]["currencies"]["EUR"]["buy"];
          
          // Constrói a UI com os campos
          return SingleChildScrollView(...);
        }
    }
  },
)
```

**Estados do FutureBuilder:**

| Estado | Ação |
|--------|------|
| `none` / `waiting` | Mostra "Carregando dados..." |
| `error` | Mostra mensagem de erro |
| `done` | Mostra a UI com os campos |

---

#### Campos de Entrada (TextFields)

```dart
TextField(
  controller: realController,
  decoration: InputDecoration(
    labelText: "Reais",
    labelStyle: TextStyle(color: Colors.amber),
    border: OutlineInputBorder(),
    prefixText: "R\$"
  ),
  style: TextStyle(color: Colors.amber, fontSize: 25.0),
  onChanged: _realChanged,
  keyboardType: TextInputType.numberWithOptions(decimal: true),
)
```

**Componentes:**
- `controller`: Conecta ao TextEditingController
- `decoration`: Aparência e rótulo
- `prefixText`: Mostra "R$" automaticamente
- `onChanged`: Chamada cada vez que o usuário digita
- `keyboardType`: Teclado numérico com decimal

---

## ▶️ Execução do Projeto

### Teste 1: Verificar Carregamento

```bash
flutter run
```

Você deve ver:
1. "Carregando dados..." (2-3 segundos)
2. Três campos de texto aparecendo
3. Ícone de moeda no topo

### Teste 2: Testar Conversão

1. Digite `100` no campo de Reais
2. Observe os outros campos atualizarem automaticamente
3. Teste em todos os três campos

### Teste 3: Verificar Erro

1. Desligue a internet
2. Reinicie o app (`flutter run`)
3. Deve mostrar "Erro ao Carregar Dados :("

### Teste 4: Hot Reload

```bash
# No terminal (enquanto app está rodando):
r    # Hot reload (rápido)
R    # Hot restart (reinicia)
q    # Sair
```

---

## 🎓 Conceitos-Chave Aprendidos

### 1. Widget Lifecycle

```
StatefulWidget criado
    ↓
_HomeState createState()
    ↓
initState() (opcional)
    ↓
build()
    ↓
setState() ← Usuário digita
    ↓
build() novamente (Rebuild)
    ↓
dispose() (limpeza final)
```

### 2. Padrão FutureBuilder

Ideal para operações assíncronas:
- Requisições HTTP
- Leitura de arquivos
- Consultas de banco de dados

### 3. TextEditingController

Permite controlar e sincronizar múltiplos campos:
- Definir valor: `controller.text = "novo valor"`
- Obter valor: `controller.text`
- Limpar: `controller.clear()`

### 4. Conversão de Tipos

```dart
double.parse("10.5")        // String → double
"10.5".toDouble()           // Alternativa
(10.5).toStringAsFixed(2)   // double → String formatado
json.decode('{"a":1}')      // JSON → Map
```

### 5. Requisições HTTP

```dart
// Requisição GET
http.Response response = await http.get(url);

// Requisição POST
http.Response response = await http.post(url, body: data);

// Verificar status
if (response.statusCode == 200) { ... }

// Extrair corpo
String body = response.body;
```

---

## 🚀 Desafios e Extensões

### Desafio 1: Adicionar Mais Moedas ⭐

**Objetivo**: Adicionar conversão para Libra Esterlina (GBP)

**Passos:**
1. Adicione um novo `TextEditingController`:
```dart
final libraController = TextEditingController();
```

2. Declare variável da taxa:
```dart
double libra;
```

3. Extraia a taxa da API:
```dart
libra = snapshot.data["results"]["currencies"]["GBP"]["buy"];
```

4. Crie a função de conversão:
```dart
void _libraChanged(String text) {
  double libra_value = double.parse(text);
  realController.text = (libra_value * libra).toStringAsFixed(2);
  dolarController.text = (libra_value * libra / dolar).toStringAsFixed(2);
  euroController.text = (libra_value * libra / euro).toStringAsFixed(2);
}
```

5. Adicione o TextField:
```dart
TextField(
  controller: libraController,
  decoration: InputDecoration(
    labelText: "Libras",
    labelStyle: TextStyle(color: Colors.amber),
    border: OutlineInputBorder(),
    prefixText: "£ "
  ),
  style: TextStyle(color: Colors.amber, fontSize: 25.0),
  onChanged: _libraChanged,
  keyboardType: TextInputType.numberWithOptions(decimal: true),
)
```

---

### Desafio 2: Exibir Taxa de Câmbio ⭐⭐

**Objetivo**: Mostrar as taxas atuais na tela

**Dica:**
```dart
Text("1 USD = R\$ ${dolar.toStringAsFixed(2)}",
  style: TextStyle(color: Colors.amber),
)
```

---

### Desafio 3: Persistir Dados com SharedPreferences ⭐⭐⭐

**Objetivo**: Guardar o último valor convertido quando app fecha

**Passos:**
1. Adicione dependência em `pubspec.yaml`:
```yaml
dependencies:
  shared_preferences: ^2.0.0
```

2. Importe:
```dart
import 'package:shared_preferences/shared_preferences.dart';
```

3. Salve ao mudar:
```dart
void _realChanged(String text) async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString('last_real', text);
  // ... resto do código
}
```

4. Carregue ao iniciar:
```dart
@override
void initState() {
  super.initState();
  _loadLastValue();
}

void _loadLastValue() async {
  final prefs = await SharedPreferences.getInstance();
  String? lastReal = prefs.getString('last_real');
  if (lastReal != null) {
    realController.text = lastReal;
  }
}
```

---

### Desafio 4: Histórico de Conversões ⭐⭐⭐

**Objetivo**: Guardar as últimas 5 conversões

**Sugestão:**
```dart
List<String> historico = [];

void _salvarHistorico(double real, double dolar_value, double euro_value) {
  String entrada = "R\$ $real → US\$ $dolar_value | € $euro_value";
  historico.add(entrada);
  
  if (historico.length > 5) {
    historico.removeAt(0);  // Remove a mais antiga
  }
  setState(() {});
}
```

---

### Desafio 5: Tema Escuro e Claro ⭐⭐⭐

**Objetivo**: Permitir alternar entre temas

**Abordagem:**
```dart
bool isDarkMode = true;

Color bgColor = isDarkMode ? Colors.black : Colors.white;
Color textColor = isDarkMode ? Colors.amber : Colors.black;

// Adicione botão para alternar
FloatingActionButton(
  onPressed: () {
    setState(() {
      isDarkMode = !isDarkMode;
    });
  },
  child: Icon(isDarkMode ? Icons.light_mode : Icons.dark_mode),
)
```

---

### Desafio 6: Testes Unitários ⭐⭐⭐⭐

**Objetivo**: Validar cálculos de conversão

Crie arquivo: `test/conversor_test.dart`

```dart
import 'package:flutter_test/flutter_test.dart';

void main() {
  test('Conversão de Real para Dólar', () {
    double dolar = 5.0;
    double real = 100.0;
    double resultado = real / dolar;
    
    expect(resultado, 20.0);
  });

  test('Conversão com 2 casas decimais', () {
    double resultado = 33.333;
    String formatado = resultado.toStringAsFixed(2);
    
    expect(formatado, "33.33");
  });
}
```

Execute com:
```bash
flutter test
```

---

## 📝 Notas para o Educador

### Tempo Estimado
- ⏱️ Explicação teórica: 30-45 minutos
- ⏱️ Demonstração prática: 20-30 minutos
- ⏱️ Desafios em grupo: 45-60 minutos

### Pontos de Discussão

1. **Por que StatefulWidget?**
   - Necessidade de atualizar estado
   - Comparação com StatelessWidget

2. **Padrão FutureBuilder**
   - Tratamento de operações assíncronas
   - Estados de carregamento

3. **API e JSON**
   - Como acessar dados da web
   - Estrutura JSON e parsing

4. **Validação de Entrada**
   - Tratamento de `double.parse()` com valores inválidos
   - Adicione try-catch como extensão

### Erros Comuns

| Erro | Causa | Solução |
|------|-------|--------|
| "Invalid argument" ao converter | Texto vazio ou inválido | Valide antes de `double.parse()` |
| "No internet connection" | API indisponível | Use try-catch na função getData() |
| "Null" error na taxa de câmbio | JSON malformado | Verifique a estrutura da resposta |
| TextField não atualiza | Controller não sincronizado | Verifique o `controller:` no TextField |

---

## 📚 Recursos Adicionais

- **Flutter Docs**: https://flutter.dev/docs
- **Dart Language Tour**: https://dart.dev/guides/language/language-tour
- **Material Design**: https://material.io/design
- **HTTP Package**: https://pub.dev/packages/http
- **API HG Brasil**: https://hgbrasil.com/status/finance

---

## ✅ Checklist Final

Antes de usar em aula, verifique:

- [ ] Todos têm Flutter instalado
- [ ] Código foi testado em múltiplos emuladores/dispositivos
- [ ] Chave da API HG Brasil está válida
- [ ] Ejemplos de slides foram preparados
- [ ] Desafios têm respostas prontas
- [ ] Material de apoio foi impresso/compartilhado

---

**Criado para fins educacionais. Sinta-se livre para adaptar conforme sua necessidade!** 🎓
