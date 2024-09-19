
```mermaid
erDiagram
    
    Musica {
        id_musica INT PK
        titulo VARCHAR(255)
        duracao INT
        id_disco INT FK
    }

    Artista {
        id_artista INT PK
        nome VARCHAR(255)
        data_nascimento DATE
    }

    Disco {
        id_disco INT PK
        titulo VARCHAR(255)
        data_lancamento DATE
        id_artista INT FK
    }

    Usuario {
        id_usuario INT PK
        nome VARCHAR(255)
        email VARCHAR(255) UNIQUE
        data_registro DATE
    }

    Playlist {
        id_playlist INT PK
        titulo VARCHAR(255)
        id_usuario INT FK 
    }

    Musica_Artista {
        id_musica INT FK
        id_artista INT FK
    }

    Playlist_Musica {
        id_playlist INT FK
        id_musica INT FK
    }

    Musica ||--|| Disco : pertence a
    Disco ||--|| Artista : pertence a
    Playlist ||--|| Usuario : pertence a
    Musica ||--|{ Artista : é interpretada por
    Playlist ||--|{ Musica : contém
    
```
