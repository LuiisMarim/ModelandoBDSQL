## 3. Quais são os títulos dos discos lançados após 2020?

    SELECT DISTINCT titulo
    FROM disco d 
    WHERE data_lancamento > '12/31/2020'
## 8. Encontre todos os artistas que não têm músicas.

    select nome 
    from artista 
    left join musica_artista on artista.artista_id = musica_artista.artista_id 
    where musica_artista.artista_id is NULL    
    
