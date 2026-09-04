# Cartão de Ponto

Site estático (`index.html`) para bater ponto (entrada, pausa, retorno, saída)
e gerar relatórios (CSV/PDF) por dia, semana ou mês. Os dados ficam no
Firestore (Firebase), então qualquer pessoa com o link vê os mesmos dados,
de qualquer aparelho, na hora que quiser — sem precisar de login.

## 1. Criar o projeto Firebase (gratuito)

1. Acesse https://console.firebase.google.com e crie um projeto novo.
2. No menu **Build → Firestore Database**, clique em *Create database*,
   escolha um local (ex.: `southamerica-east1`) e inicie em **modo produção**.
3. Ainda em Firestore, aba **Regras**, cole o conteúdo de [`firestore.rules`](./firestore.rules)
   e publique. (Isso libera leitura/escrita sem exigir login — o link é a
   única barreira de acesso, então só compartilhe com quem deve usar o sistema.)
4. No menu **⚙️ Configurações do projeto → Seus apps**, clique no ícone `</>`
   para registrar um "app da Web", dê um nome qualquer e copie o objeto
   `firebaseConfig` que aparece.
5. Cole esses valores em [`firebase-config.js`](./firebase-config.js), substituindo os placeholders.

## 2. Publicar no GitHub

```bash
git add -A
git commit -m "Configura Firebase"
git push
```

## 3. Publicar no Render

1. No painel do Render: **New → Static Site**.
2. Conecte este repositório.
3. **Build command:** deixe em branco.
4. **Publish directory:** `.`
5. Crie o site. O Render te dá uma URL pública (`https://seu-site.onrender.com`) —
   esse é o link que você compartilha com quem for bater o ponto.

## Como funciona

- Na primeira visita, o app pede um nome — ele fica salvo no navegador daquele
  aparelho e identifica as marcações dessa pessoa dali em diante.
- O relatório tem um seletor "Ver registros de": mostra os seus, os de outra
  pessoa específica, ou "Todos" (todo mundo que já bateu ponto pelo link).
- O horário de cada marcação é sempre o relógio do aparelho que bateu o
  ponto — gravado como texto (`HH:mm:ss`), sem conversão de fuso horário.
- "Carga horária diária esperada" (padrão 8h) é usada para calcular o saldo
  (banco de horas) de cada dia/período; é uma configuração global do sistema.

## Segurança

Sem login: qualquer pessoa com o link do site lê e grava os dados. É
suficiente para uso pessoal/familiar/pequena equipe combinada, mas não use
esse link em lugar público. Se precisar de autenticação de verdade depois,
dá para evoluir com Firebase Authentication — me avise.
