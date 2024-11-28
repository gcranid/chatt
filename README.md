# Chatt

`Chatt` is an open source chatbot based on LLMs. It supports a wide range of language models including:

- Ollama served models
- OpenAI
- Azure OpenAI
- Anthropic
- Moonshot
- Gemini
- Groq

`Chatt` supports multiple types of chat:

- Free chat with LLMs
- Chat with LLMs based on knowledge base

`Chatt` feature list:

- Ollama models management
- Knowledge bases management
- Chat
- Commercial LLMs API keys management

## Quick Start

As a user of `Chatt`, please walk through the document below, to make sure you get all the components up and running before starting using `Chatt`.

### Supported Vector Databases

`Chatt` supported 2 types of vector databases: Milvus and Chroma.

Please refer to the `.env.example` for how to work with your vector database setup.

```
# Supported values: chroma, milvus
VECTOR_STORE=chroma
CHROMADB_URL=http://localhost:8000
MILVUS_URL=http://localhost:19530
```

By default `Chatt` is using Chroma. If you'd like to use Milvus, set `VECTOR_STORE` to `milvus` and specify the corresponding URL. It works both in the development server and Docker containers.

### Use with Nuxt 3 Development Server

If you'd like to run with the latest code base and apply changes as needed, you can clone this repository and follow the steps below.

1. Install and run Ollama server

   You will need an Ollama server running. Follow the installation guide of [Ollama](https://github.com/ollama/ollama). By default, it's running on http://localhost:11434.

2. Install Chroma

   Please refer to [https://docs.trychroma.com/getting-started](https://docs.trychroma.com/getting-started) for Chroma installation.

   We recommend you run it in a docker container:

   ```bash
   #https://hub.docker.com/r/chromadb/chroma/tags

   docker pull chromadb/chroma
   docker run -d -p 8000:8000 chromadb/chroma
   ```

   Now, ChromaDB is running on http://localhost:8000

3. Chatt Setup

   Now, we can complete the necessary setup to run Chatt.

   3.1 Copy the `.env.example` file to `.env` file:

   ```bash
   cp .env.example .env
   ```

   3.2 Make sure to install the dependencies:

   ```bash
   pnpm install
   ```

   3.3 Run a migration to create your database tables with Prisma Migrate

   ```bash
   pnpm prisma-migrate
   ```

4. Launch Development Server

   > Make sure both **[Ollama Server](#ollama-server)** and **[ChromaDB](#install-chromadb-and-startup)** are running.

   Start the development server on `http://localhost:3000`:

   ```bash
   pnpm dev
   ```

### Use with Docker

This is the easist way to use `Chatt`.

The only thing you need is a copy of [docker-compose.yaml](./docker-compose.yaml). Please download it and execute the command below to launch `Chatt`.

```shell
$ docker compose up
```

As `Chatt` is running within a docker container, you should set Ollama server to `http://host.docker.internal:11434` in the Settings section, assuming your Ollama server is running locally with default port.

Make sure you initialize the SQLite database as below if you are launching the dockerized `Chatt` for the first time:

```shell
$ docker compose exec Chatt npx prisma migrate dev
```

#### Prerequisites for knowledge bases

When using KnowledgeBases, we need a valid embedding model in place. It can be one of the models downloaded by Ollama or from 3rd party service provider for example, OpenAI.

**Ollama Managed Embedding Model**

We recommend you download `nomic-embed-text` model for embedding purpose.

You can do so on Models page http://localhost:3000/models, or via CLI as below if you are using Docker.

```shell
# In the folder of docker-compose.yaml

$ docker compose exec ollama ollama pull nomic-embed-text:latest
```

**OpenAI Embedding Model**

If you prefer to use OpenAI, please make sure you set a valid OpenAI API Key in Settings, and fill with one of the OpenAI embedding models listed below:

- `text-embedding-3-large`
- `text-embedding-3-small`
- `text-embedding-ada-002`

#### Data Storage with Docker Containers

There are 2 types of data storage, vector data and relational data. See the summary below and for more details, please refer to [docker-compose.yaml](./docker-compose.yaml) for the settings.

##### Vector data

With `docker-compose.yaml`, a dockerized Chroma database is run side by side with `Chatt`. The data is persisted in a docker volume.

##### Relational data

The relational data including knowledge base records and their associated files are stored in a SQLite database file persisted and mounted from `~/.Chatt/Chatt.sqlite`.

#### Proxy

We have provided a proxy configuration feature. For specific usage, please click [here](docs/proxy-usage.md).

## Developers Guide

As Chatt is still under active development, features, interfaces and database schema may be changed. Please follow the instructions below in your every `git pull` to make sure your dependencies and database schema are always in sync.

1. Install the latest dependencies
   - `pnpm install`
2. Prisma migrate
   - `pnpm prisma-migrate`
