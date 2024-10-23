# Abaixo o código usado para inserção de dados no banco. 
---

### ALBUNS 💿
➡️ <b>Thriller</b> : *2ANVost0y2y52ema1E9xAZ*

➡️ <b>Doce</b> : *2LeCiUHBSmUMyrclDEEBly*

➡️ <b>Depois do fim</b> : *4Rfk2LiqBbz0FH97L9WWoy*

➡️ <b>Julie and the Phantoms</b> : *3rxj1eHjamXQyJHLVPOpHm8*

➡️ <b>Midnight Memories</b> : *7p1fX8aUySrBdx4WSYspOu*

➡️ <b>Made in The AM</b> : *1gMxiQQSg5zeu4htBosASY*

➡️ <b>Hot Fuss</b> : *6TJmQnO44YE5BtTxH8pop1*

➡️ <b>The Killers</b> : *4cJ2Up81K2vyvPHFQ62iw1*

➡️ <b>All This Nad Blood</b> : *2r1iXEfyFZaYSmthwnKGGp*

➡️ <b>Midnight Memories</b> : *7p1fX8aUySrBdx4WSYspOu*


``` python

import psycopg2
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

# Autenticação na API do Spotify
def spotify_auth():
    client_credentials_manager = SpotifyClientCredentials(client_id='6a5a3eeebbd9404ca8900cd6a7d3ff51',
                                                          client_secret='6621b56cbc95428086955c8ba8bec4bc')
    return spotipy.Spotify(client_credentials_manager=client_credentials_manager)

def connect_to_db():
    try:
        conn = psycopg2.connect(
            dbname="NOME DATA BASE COCKROACH DB",
            user="USUARIO COCKROACH DB",
            password="SENHA COCKROACH DB",
            host="HOST COCKROACH DB",
            port="PORT COCKROACH DB"
        )
        return conn
    except Exception as e:
        print(f"Error connecting to CockroachDB: {e}")
        return None

# Função para buscar dados do Spotify e inserir no banco de dados
def insert_data_from_spotify():
    conn = connect_to_db()
    if conn is None:
        return

    sp = spotify_auth()

    try:
        # Exemplo: Buscar informações sobre um álbum específico no Spotify
        album = sp.album('SUBSTITUIR')  # Coloque o ID do álbum aqui
        album_title = album['name']
        album_release_date = album['release_date']
        artist_name = album['artists'][0]['name']
        tracks = album['tracks']['items']

        with conn.cursor() as cursor:
            # Inserir ARTISTA (verificar se o artista já existe antes de inserir)
            cursor.execute("""
                INSERT INTO ARTISTA (nome, data_nascimento)
                VALUES (%s, NULL)  -- Sem data de nascimento disponível na API
                ON CONFLICT (nome) DO NOTHING
                RETURNING artista_id;
            """, (artist_name,))

            artist_id = cursor.fetchone()
            if artist_id is None:
                cursor.execute("SELECT artista_id FROM ARTISTA WHERE nome = %s", (artist_name,))
                artist_id = cursor.fetchone()[0]
            else:
                artist_id = artist_id[0]

            # Inserir DISCO (verificar se o disco já existe antes de inserir)
            cursor.execute("""
                INSERT INTO DISCO (titulo, data_lancamento, artista_id)
                VALUES (%s, %s, %s)
                ON CONFLICT (titulo) DO NOTHING
                RETURNING disco_id;
            """, (album_title, album_release_date, artist_id))

            disco_id = cursor.fetchone()
            if disco_id is None:
                cursor.execute("SELECT disco_id FROM DISCO WHERE titulo = %s", (album_title,))
                disco_id = cursor.fetchone()[0]
            else:
                disco_id = disco_id[0]

            # Inserir cada música do álbum
            for i, track in enumerate(tracks):
                track_title = track['name']
                track_duration = track['duration_ms'] // 1000  # Duração em segundos

                cursor.execute("""
                    INSERT INTO MUSICA (titulo, duracao, disco_id)
                    VALUES (%s, to_timestamp(%s)::time, %s)
                    ON CONFLICT (titulo) DO NOTHING
                    RETURNING musica_id;
                """, (track_title, track_duration, disco_id))

                musica_id = cursor.fetchone()
                if musica_id is None:
                    cursor.execute("SELECT musica_id FROM MUSICA WHERE titulo = %s", (track_title,))
                    musica_id = cursor.fetchone()[0]
                else:
                    musica_id = musica_id[0]

                # Relacionar MUSICA com ARTISTA na tabela MUSICA_ARTISTA
                cursor.execute("""
                    INSERT INTO MUSICA_ARTISTA (musica_id, artista_id)
                    VALUES (%s, %s)
                    ON CONFLICT DO NOTHING;
                """, (musica_id, artist_id))

        conn.commit()
        print("Dados inseridos com sucesso!")
    except Exception as e:
        print(f"Erro ao inserir dados: {e}")
        conn.rollback()
    finally:
        conn.close()

if __name__ == "__main__":
    insert_data_from_spotify()


````
