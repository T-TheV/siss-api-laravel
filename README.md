# SISS - Sistema de Integração Saúde-Segurança

## Visão Geral
O SISS é uma plataforma desenvolvida para estabelecer um canal de comunicação seguro, auditável e de baixa latência entre unidades de triagem hospitalar e centros de comando da Segurança Pública.

O sistema automatiza a notificação de ocorrências sensíveis (como violência doméstica ou ferimentos por arma de fogo), garantindo o sigilo médico-paciente e a integridade dos dados, em estrita conformidade com as diretrizes de privacidade.

## Arquitetura e Soluções Técnicas
O projeto foi estruturado para suportar alta concorrência e assegurar governança no tratamento de dados críticos:

* **Controle de Acesso Baseado em Papéis (RBAC):** Isolamento rigoroso de visibilidade. Perfis hospitalares gerenciam dados clínicos, enquanto perfis de segurança pública consomem estritamente os metadados da ocorrência, sem acesso ao histórico médico.
* **Segurança (2FA):** Implementação de Autenticação de Dois Fatores (TOTP) no login, requisito obrigatório para a manipulação de dados governamentais classificados.
* **Controle de Concorrência (Optimistic Locking):** Prevenção de sobrescrita acidental de registros em cenários onde múltiplos despachantes atuam sobre a mesma ocorrência simultaneamente.
* **Arquitetura Orientada a Eventos:** Desacoplamento do processamento em tempo real. O motor de regras de negócio (Laravel) delega a emissão de notificações via publicação em filas no Redis.

## Stack Tecnológica
* **Backend:** PHP 8.x, Laravel
* **Frontend Interno:** Vue.js, Tailwind CSS
* **Banco de Dados:** PostgreSQL
* **Mensageria e Cache:** Redis
* **Segurança e Autenticação:** Laravel Sanctum, Laravel Fortify, Spatie Permission

## Ecossistema de Microsserviços
Este repositório contém a API REST e a interface de gestão central. O serviço responsável por manter as conexões WebSocket persistentes e distribuir os alertas em tempo real foi isolado em uma aplicação Node.js independente.

[Acessar repositório do serviço de mensageria em tempo real](https://github.com/T-TheV/siss-realtime-nodejs)

## Execução Local

### Pré-requisitos
* PHP 8.1+ e Composer
* PostgreSQL e Redis
* Node.js e NPM

### Instalação e Configuração Inicial
```bash
git clone [https://github.com/SEU-USUARIO/siss-api-laravel.git](https://github.com/SEU-USUARIO/siss-api-laravel.git)
cd siss-api-laravel
composer install
npm install
cp .env.example .env
php artisan key:generate

```

Após configurar as credenciais do banco de dados e do Redis no arquivo `.env`, execute as migrações:

```bash
php artisan migrate --seed

```

### Inicialização

```bash
npm run dev
php artisan serve

```
