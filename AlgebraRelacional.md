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


## 12. Liste os títulos das músicas que aparecem em mais de uma playlist.

    select titulo
    from musica m 
    join playlist_musica pm on m.musica_id = pm.musica_id
    group by m.titulo 
    having count(pm.playlist_id) > 1
    
## 14. Qual é o título da música mais longa do disco 'PIRATA'?.

    SELECT m.titulo
    FROM musica m
    JOIN disco d ON m.disco_id = d.disco_id
    WHERE d.titulo = 'PIRATA'
    ORDER BY m.duracao DESC
    LIMIT 1
    
## 15. Liste todos os discos lançados por um artista específico em um determinado ano.

    SELECT DISTINCT titulo
    FROM disco 
    left join artista on disco.artista_id = artista.artista_id 
    WHERE data_lancamento < '12/31/2021' and data_lancamento > '01/01/2021' and      artista.nome = 'Jão'
    
## 16. Quais são os nomes dos artistas que têm músicas em playlists criadas por um usuário específico?

    select distinct a.nome
    from artista  a
    join disco d ON a.artista_id = d.artista_id
    join musica m on d.disco_id = m.disco_id 
    join playlist_musica pm ON m.musica_id = pm.musica_id 
    join playlist p on pm.playlist_id  = p.playlist_id 
    join usuario u on p.usuario_id = u.usuario_id 
    where u.nome = 'stellar'

## 17. Encontre a lista de músicas que não estão em nenhuma playlist.

    select titulo
    from musica
    left join playlist_musica on musica.musica_id = playlist_musica.musica_id 
    where playlist_musica.playlist_id is NULL    

## 20. Liste todos os usuários e suas playlists, mas apenas para playlists que contêm pelo menos 5 músicas?

    select titulo, u.nome
    from playlist p
    join playlist_musica pm on p.playlist_id = pm.playlist_id
    join usuario u on p.usuario_id = u.usuario_id 
    where p.playlist_id is not null 
    group by titulo, u.nome
    having count(pm.musica_id) > 5
    

    
    
