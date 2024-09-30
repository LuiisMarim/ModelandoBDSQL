# Modelo Entidade-Relacional

## Entidades

- **ARTISTA**
  - `artista_id` (PK)
  - `nome`
  - `data_nascimento`

- **DISCO**
  - `disco_id` (PK)
  - `titulo`
  - `data_lancamento`
  - `artista_id` (FK)

- **MUSICA**
  - `musica_id` (PK)
  - `titulo`
  - `duracao`
  - `disco_id` (FK)

- **USUARIO**
  - `usuario_id` (PK)
  - `nome`
  - `email` (único)
  - `data_registro`

- **PLAYLIST**
  - `playlist_id` (PK)
  - `titulo`
  - `usuario_id` (FK)

## Relacionamentos

- **Música-Artista** (associação)
  - Muitos-para-Muitos
  - `musica_id` (FK)
  - `artista_id` (FK)

- **Playlist-Música** (associação)
  - Muitos-para-Muitos
  - `playlist_id` (FK)
  - `musica_id` (FK)

## Diagrama MER

```plaintext
+---------------+        +------------------+
|   ARTISTA     |        |     DISCO        |
|---------------|        |------------------|
| artista_id PK |<-------| disco_id PK      |
| nome          |        | titulo           |
| data_nasc     |        | data_lancamento  |
+---------------+        | artista_id FK    |
                        +------------------+
                               |
                               |
                               |
+----------------+          +------------------+
|    MUSICA      |<--------|   MUSICA_ARTISTA |
|----------------|          |------------------|
| musica_id PK   |          | musica_id FK     |
| titulo         |          | artista_id FK    |
| duracao       |          +------------------+
| disco_id FK    |
+----------------+
                               |
                               |
                               |
+----------------+          +------------------+
|    PLAYLIST    |<--------|  PLAYLIST_MUSICA |
|----------------|          |------------------|
| playlist_id PK |          | playlist_id FK   |
| titulo         |          | musica_id FK     |
| usuario_id FK  |          +------------------+
+----------------+
                               |
                               |
                               |
                            +----------------+           
                            |    USUARIO     |
                            |----------------|
                            | usuario_id PK  |
                            | nome           |
                            | email (único)  |
                            | data_registro  |
                            +----------------+
