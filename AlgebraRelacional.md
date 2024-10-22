# Inserções de Usuários:
- insert into usuario values ('001', 'leo', 'leonardo@gmail.com', '02/19/2022')
- insert into usuario values ('003', 'luiis', 'luizinho@gmail.com', '2016-03-24')
- insert into usuario values ('004', 'maizinha', 'maizinha@gmail.com', '2018-07-24')
- insert into usuario values ('007', 'stellar', 'stellinha@gmail.com', '2017-02-01')
- insert into usuario values ('010', 'anna', 'anna@gmail.com', '2018-06-21')	
- insert into usuario values ('014', 'humberto', 'humberto@gmail.com', '2020-05-09')	
- insert into usuario values ('018', 'iago', 'iago@gmail.com', '2024-09-04')	
- insert into usuario values ('023', 'mariah', 'mariah@gmail.com', '2019-05-31')	
- insert into usuario values ('029', 'bruno', 'bruno@gmail.com', '2022-11-14')

# Inserção de Artista
- insert into artista (nome,data_nascimento) values ('Chappel Roan',NULL)

# Inserção de Música em playlists
- insert into playlist_musica values ('193','1013693681422696449')



## 1. Liste o título de todas as músicas e suas durações. 

    select
    titulo,  
    duracao 
    from musica

## 2. Encontre o nome de todos os artistas que têm mais de 5 músicas em seu repertório.
    SELECT 
    art.nome, 
    art.artista_id, 
    COUNT(ma.artista_id) AS quantidade 
    FROM 
        artista art 
    JOIN 
        musica_artista ma ON art.artista_id = ma.artista_id 
    GROUP BY 
        art.nome, art.artista_id 
    HAVING 
        COUNT(ma.artista_id) > 1;
        
## 3. Quais são os títulos dos discos lançados após 2020?

    SELECT DISTINCT titulo
    FROM disco d 
    WHERE data_lancamento > '12/31/2020'

## 4. Liste os títulos das músicas e os nomes dos artistas que as interpretam, ordenados pelo título da música.

    select * from musica
    select * from artista 
    select * from disco 
    select * from musica_artista 

    select 
        mus.titulo,
        art.nome
    from disco d
    join artista art on d.artista_id = art.artista_id 
    join musica mus on mus.disco_id  = d.disco_id 
    order by 
    mus.titulo;

## 5. Encontre os títulos das playlists que contêm a música com o título 'Clarão'.

    select distinct p.titulo
    from playlist p 
    join playlist_musica pm on p.playlist_id = pm.playlist_id 
    join musica m on pm.musica_id = m.musica_id 
    where m.titulo LIKE 'Clarão'    

## 6. Liste os usuários que criaram playlists que contêm músicas do disco 'AM'.

    
    select distinct u.nome
    from usuario u 
    join playlist p on u.usuario_id = p.usuario_id 
    join playlist_musica pm  on p.playlist_id = pm.playlist_id 
    join musica m on pm.musica_id = m.musica_id 
    join disco d on m.disco_id = d.disco_id 
    where d.titulo like 'AM'

## 7. Qual é a duração média das músicas de um artista específico?
    SELECT 
    art.nome,
    AVG(EXTRACT(EPOCH FROM mus.duracao)) AS duracao_media_sec
    FROM 
        artista art
    JOIN 
        musica_artista ma ON art.artista_id = ma.artista_id 
    JOIN 
        musica mus ON ma.musica_id = mus.musica_id
    GROUP BY 
        art.nome;

    
## 8. Encontre todos os artistas que não têm músicas.

    select nome 
    from artista 
    left join musica_artista on artista.artista_id = musica_artista.artista_id 
    where musica_artista.artista_id is NULL    

## 9. Liste todos os discos que contêm mais de 10 músicas.

    
    select  d.titulo ,
    count(m.disco_id) AS quantidade_musica 
    from disco d 
    join musica m on d.disco_id  = m.disco_id  
    group by d.titulo 
    having count(m.disco_id) >= 10 

## 10. Quais são os nomes dos artistas que têm discos lançados antes de 2010 e que têm músicas na playlist 'Alma Indie'?

    select distinct a.nome
    from artista  a
    join disco d ON a.artista_id = d.artista_id
    join musica m ON d.disco_id = m.disco_id
    join playlist_musica pm ON m.musica_id = pm.musica_id 
    join playlist p on pm.playlist_id  = p.playlist_id 
    where d.data_lancamento < '12/31/2010'
    and p.titulo = 'Alma Indie'

## 11. Quais músicas são interpretadas por mais de um artista?

    select titulo
    from musica m 
    join musica_artista ma on m.musica_id = ma.musica_id 
    join artista a on ma.artista_id = a.artista_id 
    group by m.titulo 
    having count(a.artista_id) > 1

## 12. Liste os títulos das músicas que aparecem em mais de uma playlist.

    select titulo
    from musica m 
    join playlist_musica pm on m.musica_id = pm.musica_id
    group by m.titulo 
    having count(pm.playlist_id) > 1

## 13. Encontre os nomes dos usuários que têm playlists que incluem a música 'Alive'.

    select distinct nome
    from usuario u 
    join playlist p on u.usuario_id = p.usuario_id 
    join playlist_musica pm on p.playlist_id = pm.playlist_id 
    join musica m on pm.musica_id = m.musica_id 
    where m.titulo LIKE 'Alive'    
    
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
    WHERE data_lancamento < '12/31/2021' and data_lancamento > '01/01/2021' and artista.nome = 'Jão'
    
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

## 18. Liste os títulos das músicas e os nomes dos artistas que têm mais de 3 músicas em uma mesma playlist.
    
    select m.titulo , a.nome from artista a 
    join musica_artista ma on a.artista_id  = ma.artista_id 
    join musica m on ma.musica_id  = m.musica_id 
    join playlist_musica pm on m.musica_id = pm.musica_id 
    group by m.titulo , a.nome 
    having  count(pm.musica_id) > 3
    
## 19. Quais são os discos que contêm músicas de artistas que têm pelo menos 2 discos lançados?

    select d.titulo
    from disco d
    where d.artista_id in (
    select a.artista_id
    from disco a
    group by a.artista_id
    having count(a.artista_id) >= 2

## 20. Liste todos os usuários e suas playlists, mas apenas para playlists que contêm pelo menos 5 músicas?

    select titulo, u.nome
    from playlist p
    join playlist_musica pm on p.playlist_id = pm.playlist_id
    join usuario u on p.usuario_id = u.usuario_id 
    where p.playlist_id is not null 
    group by titulo, u.nome
    having count(pm.musica_id) > 5







    
    
