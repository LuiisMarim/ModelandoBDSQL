# Inserções
- insert into artista (nome,data_nascimento) values ('Chappel Roan',NULL)
- insert into playlist_musica values ('193','1013693681422696449')
- insert into usuario values ('001','leo','leonardo@gmail.com','02/19/2022')


## 3. Quais são os títulos dos discos lançados após 2020?

    SELECT DISTINCT titulo
    FROM disco d 
    WHERE data_lancamento > '12/31/2020'
## 8. Encontre todos os artistas que não têm músicas.

    select nome 
    from artista 
    left join musica_artista on artista.artista_id = musica_artista.artista_id 
    where musica_artista.artista_id is NULL    

## 10. Quais são os nomes dos artistas que têm discos lançados antes de 2010 e que têm músicas na playlist 'Alma Indie'?

    select distinct a.nome
    from artista  a
    join disco d ON a.artista_id = d.artista_id
    join musica m ON d.disco_id = m.disco_id
    join playlist_musica pm ON m.musica_id = pm.musica_id 
    join playlist p on pm.playlist_id  = p.playlist_id 
    where d.data_lancamento < '12/31/2010'
    and p.titulo = 'Alma Indie'
   
    
