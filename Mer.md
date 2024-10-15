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
  

- **MUSICA**
  - `musica_id` (PK)
  - `titulo`
  - `duracao`
  

- **USUARIO**
  - `usuario_id` (PK)
  - `nome`
  - `email` (único)
  - `data_registro`

- **PLAYLIST**
  - `playlist_id` (PK)
  - `titulo`
 
## Diagrama MER

```mermaid
---
title: Modelo Entidade Relacional
---

classDiagram
    class Usuário{
          usuario_id PK
          nome char(50)          
          email (único) char(50)  
          data_registro  int(8)
      }

   
     class Música{
        musica_id PK
        titulo char(50)
        duracao float(5)
    }
    class Playlist{
         playlist_id PK
         titulo char(50)
    }
   
    class Artista{
        artista_id PK
        nome char(50)
        data_nasc int(8)
    }
     
    
     class Disco{
        disco_id PK     
        titulo char(50)
        data_lancamento  int(8)
    }
     

     


    Música "1" --> "1" Disco : Pertence
    Música "1" --> "n" Artista : Interpretada
    Música "1" --> "1..n" Playlist : Aparece

    Disco "1" --> "n" Música : Contém 
    Disco "1" --> "1" Artista : Pertence

    
    Playlist "1" --> "1..n" Música : Contém
    Usuário "1" --> "0..n" Playlist : Cria
    








``` 
