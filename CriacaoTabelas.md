# Criação das tabelas necessárias:

```sql


CREATE TABLE ARTISTA (
    artista_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255) NOT NULL,
    data_nascimento DATE
);


CREATE TABLE DISCO (
    disco_id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255) NOT NULL,
    data_lancamento DATE,
    artista_id INT,
    FOREIGN KEY (artista_id) REFERENCES ARTISTA(artista_id)
);


CREATE TABLE MUSICA (
    musica_id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255) NOT NULL,
    duracao TIME NOT NULL,
    disco_id INT,
    FOREIGN KEY (disco_id) REFERENCES DISCO(disco_id)
);


CREATE TABLE USUARIO (
    usuario_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    data_registro DATE NOT NULL
);


CREATE TABLE PLAYLIST (
    playlist_id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255) NOT NULL,
    usuario_id INT,
    FOREIGN KEY (usuario_id) REFERENCES USUARIO(usuario_id)
);


CREATE TABLE MUSICA_ARTISTA (
    musica_id INT,
    artista_id INT,
    PRIMARY KEY (musica_id, artista_id),
    FOREIGN KEY (musica_id) REFERENCES MUSICA(musica_id),
    FOREIGN KEY (artista_id) REFERENCES ARTISTA(artista_id)
);


CREATE TABLE PLAYLIST_MUSICA (
    playlist_id INT,
    musica_id INT,
    PRIMARY KEY (playlist_id, musica_id),
    FOREIGN KEY (playlist_id) REFERENCES PLAYLIST(playlist_id),
    FOREIGN KEY (musica_id) REFERENCES MUSICA(musica_id)
);
