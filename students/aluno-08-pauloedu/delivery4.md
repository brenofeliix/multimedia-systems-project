# Delivery 4 – Architecture and Technologies

## Student Information
- Name:Paulo Eduardo de Leon Fernandes Alves
- ID:20230079170

---

## System Architecture
graph TD
    A[Professor / Aluno] -->|1. Faz Upload da Videoaula e define cortes| B(Interface Web / Frontend)
    B -->|2. Envia arquivo e marcações de tempo| C(Servidor API / Backend)
    C -->|3. Salva metadados dos clipes| D[(Banco de Dados)]
    C -->|4. Envia vídeo bruto e comandos de corte| E[Motor de Mídia - FFmpeg]
    E -->|5. Processa, comprime e corta o vídeo| F[Armazenamento de Arquivos / Storage]
    F -->|6. Disponibiliza link do clipe final| B
---

## Components
FrontEnd: aqui é onde será onde seram marcados os pontos de interesse dos videos  
do inicio ao fim  

BackEndAPI: Sera o serviço principal para rotear as requisições e gerenciar a  
logica de negocios e enfileirar tarefas de processamento de videos

Banco de Dados: Armazenamento relacional usado para acompanhar as contas dos usuários, metadados dos vídeos  
 e os carimbos de hora exatos de cada clipe.

Motor de Mídia (FFmpeg Worker): Um módulo de processamento isolado dedicado inteiramente à execução de  comandos pesados de divisão e corte de vídeo.

Sistema de Armazenamento (Storage): Um diretório seguro no sistema de arquivos onde são armazenadas as  videoaulas brutas enviadas e os clipes finais processados.

---

## Technologies Used
Frontend: HTML5 Video API, CSS3 e JavaScript (ou React) para a clipagem interativa na linha do tempo.

Backend (API REST): Python com o framework FastAPI. pela sua alta performance e suporte nativo a requisições assíncronas.

Banco de Dados: MySQL para armazenar metadados relacionais e logs do sistema.

Processamento de Mídia: FFmpeg (ferramenta de linha de comando) para corte e compressão de vídeo rápidos e sem perda de qualidade.

---

## Media Processing
Codecs de Vídeo/Áudio: O sistema padroniza os uploads em H.264 (AVC) para vídeo e AAC para áudio para garantir compatibilidade de reprodução nativa em todos os navegadores web modernos.

Formato do Container: Todos os clipes de saída são gerados no formato .mp4.

Mecanismo de Clipagem: Quando um usuário solicita um corte, o backend executa um comando em segundo plano não bloqueante usando o FFmpeg. Ele extrai o segmento utilizando os carimbos de hora de início e fim fornecidos, minimizando os tempos de re-codificação para entregar o clipe quase instantaneamente.

--- eu der deixado isso assim vai ser aceitavel?
