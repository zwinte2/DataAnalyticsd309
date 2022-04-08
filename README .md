#SPARKIFY DATABASE README

##DATABASE OVERVIEW:

This database has been created for Sparkify for the following analytical purposes:
'''

- To orgainze user demographics such as first name, last name, gender, and to capture user agent used for accessing Sparkify
    
- To organize listening information of users such as song, artist, and start times of each listening session
    
- To organize music information such as song titles and info about the artist such as artist name and location
'''

Through the implementation of a Postgres database to utilize and organize the data goals above, **Sparkify can now query the database to obtain analytics on what songs their users are listening to.** This, above all else, has been prioritized through this database with built in queries supplying data to the 'songplays' table, which contains info on what song the user is listening to via multiple fact tables. 

##ETL PIPELINE DESCRIPTION:

This postgres database utilizes an ETL pipeline to fill fact tables to support a main dimensional table (songplays). Below I will outline the database schema and how it opperates in this pipe line.
'''
- Two JSON datafiles are being accessed. Song_data and Log_data. The first being handled is Song_data.
- Four fact tables exist: songs, artists, time, users
- One dimensional table exists: songplays 

'''

from sqlalchemy_schemadisplay import create_schema_graph
from sqlalchemy import MetaData

def main():
    graph = create_schema_graph(metadata=MetaData('postgresql://student:student@127.0.0.1/sparkifydb'))
    graph.write_png('sparkifydb_erd.png')

if __name__ == "__main__":
    main()

    1. Song_data is loaded into database as df 
    
    2. columns 'song_id','title','artist_id','year', and 'duration' from each row are placed into the fact 'songs' table using insert query 'song_table_insert'
    
    3. columns 'artist_id','artist_name', 'artist_location', 'artist_latitude', and 'artist_longitude' from each row are placed into the fact 'artists' table using insert query 'artist_table_insert'
    
    4.Log_data is loaded into database as df
    
    5. df is orgainized by entries that are toggled 'NextSong' in the page column to sort by listeners who are in a listening session
    
    6. The timestamp (ts) column is converted from miliseconds to standard dateTime format for easier reading
    
    7. 'start_time', 'hour', 'day', 'week', 'month', 'year', and 'weekday' from the 'ts' converted entries are entered into  'time_df'
    
    8. Entries from 'time_df' are placed into fact 'time' table using insert query 'time_table_insert'

    9. columns 'userId','firstName','lastName','gender', and 'level' from each row are placed into the fact 'user' table using insert query 'user_table_insert'
    
    10. A forloop performs the query "song_select" for each row of the database where data from all prior tables is used to populate the dimensional 'songplays' table. 
    
    11. A data processing forloop occurs to commit every new row in 'songplays' table for future query purposes. 
    
    12. Connection to database is closed
    
    
######REFERENCE DISCLOSURE: 
Through out different points of this project I had to reference the Udacity knowledge base of this project for guidance when specific errors were reached. In notable areas this is mentioned in code markup, but for small simple fixes I did not feel the need to reference the material (as most of the time they were just syntax errors). Any likeness to other project submissions is completely unintentional, as mentor knowledge base replies were used as guidance as I am sure is done on many other submissions. 
    
