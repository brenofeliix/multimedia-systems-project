# Delivery 4 – Architecture and Technologies

## Student Information
- Name: Eduardo Fernandes Mendonça
- ID: 20230079152

---

## System Architecture
O sistema utiliza arquitetura cliente-servidor.

A aplicação mobile desenvolvida em React Native funciona como cliente, responsável pela interface do usuário e interação com recursos multimídia. O backend utiliza Supabase para armazenamento de dados, autenticação e gerenciamento de arquivos de mídia.

A comunicação ocorre através de APIs e serviços de geolocalização.

---

## Components
- Interface mobile
- Sistema de mapas interativos
- Serviço de geolocalização GPS
- Banco de dados Supabase
- Sistema de upload de imagens
- Sistema de avaliações
- Armazenamento em nuvem
---

## Technologies Used
- React Native
- Expo
- React Native Maps
- Expo Location
- Expo Image Picker
- Supabase
- PostgreSQL
- JavaScript / TypeScript
- GitHub

---

## Media Processing
As imagens dos pontos de coleta são capturadas pelo dispositivo e enviadas para armazenamento em nuvem utilizando Supabase Storage.

Os mapas são carregados dinamicamente através da API de mapas e atualizados conforme a localização do usuário. As animações da interface são processadas localmente para melhorar a experiência do usuário.

---

