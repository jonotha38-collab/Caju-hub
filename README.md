# 🏢 Caju Hub - Sistema de Salas de Reunião

Sistema web para reserva de salas de reunião com mapa interativo e painel administrativo.

## 🚀 Tecnologias

- **Frontend:** React + TypeScript + Vite
- **Autenticação:** Firebase Auth (Email/Password)
- **Banco de Dados:** Firestore
- **Estilização:** Tailwind CSS + shadcn/ui
- **Roteamento:** React Router v6

## 📋 Pré-requisitos

- Node.js 18+
- npm ou yarn
- Conta Firebase configurada

## ⚙️ Instalação

```bash
# Instalar dependências
npm install

# Criar arquivo .env com as credenciais Firebase
# (copiar de .env.example e preencher com seus dados)
```

## 🏃 Executar Projeto

```bash
# Modo desenvolvimento (hot reload)
npm run dev

# Build para produção
npm run build

# Preview do build
npm run preview
```

O site abrirá em **http://localhost:8080**

## 📁 Estrutura do Projeto

```
src/
├── pages/           # Páginas principais
│   ├── Index.tsx
│   ├── MapPage.tsx
│   ├── RoomsPage.tsx
│   ├── RoomDetailPage.tsx
│   ├── BookingsPage.tsx
│   ├── AuthPage.tsx
│   └── AdminPage.tsx
├── components/      # Componentes reutilizáveis
├── lib/             # Funções utilitárias
├── contexts/        # Context API
├── integrations/    # Firebase config
└── App.tsx          # Roteamento principal
```

## 🔑 Variáveis de Ambiente

Criar arquivo `.env` na raiz com:

```env
VITE_FIREBASE_API_KEY=xxx
VITE_FIREBASE_AUTH_DOMAIN=xxx.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=xxx
VITE_FIREBASE_STORAGE_BUCKET=xxx.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=xxx
VITE_FIREBASE_APP_ID=xxx
```

## 📍 Páginas Principais

- **`/`** - Home com mapa interativo
- **`/rooms`** - Lista de todas as salas
- **`/rooms/:roomId`** - Detalhes e reserva da sala
- **`/map`** - Mapa em tela cheia
- **`/bookings`** - Minhas reservas
- **`/auth`** - Login/Cadastro
- **`/admin`** - Painel administrativo

## 👤 Autenticação

### Login
- Email + Senha
- Recuperação de conta via Firebase

### Admin
- Criar documento em `user_roles` no Firestore com:
  ```json
  {
    "user_id": "seu_uid_aqui",
    "role": "admin"
  }
  ```

## 🗂️ Firestore Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAdmin() {
      let userRole = get(/databases/$(database)/documents/user_roles/$(request.auth.uid));
      return userRole.data.role == 'admin';
    }
    
    match /rooms/{document=**} {
      allow read: if true;
      allow write: if request.auth != null && isAdmin();
    }
    match /bookings/{document=**} {
      allow read, write: if request.auth != null;
    }
    match /user_profiles/{document=**} {
      allow read, write: if request.auth != null;
    }
    match /user_roles/{document=**} {
      allow read: if request.auth != null;
      allow write: if isAdmin();
    }
  }
}
```

## 📦 Build & Deploy

```bash
# Build para produção
npm run build

# Arquivos prontos em ./dist
```

## 🐛 Troubleshooting

### Mapa não aparece
- Verificar console (F12) para erros
- Limpar cache do navegador (Ctrl+Shift+R)

### Erro "Permission denied"
- Verificar Firestore Rules
- Confirmar que user_roles foi criado corretamente

### Salas não carregam
- Verificar conexão Firebase
- Confirmar .env com credenciais corretas

## 📄 Licença

© 2025 Caju Hub - Todos os direitos reservados
