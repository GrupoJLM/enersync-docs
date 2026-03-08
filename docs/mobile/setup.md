# Mobile — Setup

## Pré-requisitos

- Flutter 3.38+
- Dart 3.10+
- Android Studio ou Xcode (para emulador)

## Stack

| Tecnologia | Uso |
|------------|-----|
| **Riverpod** | State management |
| **GoRouter** | Navegação |
| **Dio** | HTTP client com JWT interceptor |
| **Flutter Secure Storage** | Armazenamento seguro de tokens |
| **json_serializable** | Serialização JSON |

## Instalação

```bash
cd enersync-mobile

# Instalar dependências
flutter pub get

# Gerar código (json_serializable, riverpod_generator)
dart run build_runner build --delete-conflicting-outputs

# Rodar no emulador/dispositivo
flutter run
```

## Configuração da API

Por padrão, o app conecta em `http://10.0.2.2:8000` (localhost do emulador Android).

Para alterar:

```bash
flutter run --dart-define=API_BASE_URL=http://seu-servidor:8000
```

## Análise e Testes

```bash
flutter analyze
flutter test
```

## Build

=== "Android"

    ```bash
    flutter build apk --release
    ```

=== "iOS"

    ```bash
    flutter build ios --release
    ```

## Status Atual

O app mobile está em fase inicial com:

- Login com JWT
- Dashboard básico
- Estrutura de navegação com GoRouter
- Interceptor Dio para refresh token automático
