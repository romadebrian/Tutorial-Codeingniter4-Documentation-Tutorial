# Tutorial 2 - Codeingniter4

# News section

Ringkasan daro tutorial 1 Codeigniter Dokumentasi

1. Bikin database
    - Nama database 'ci4tutorial'
    - Bikin Table news dengan menjalankan query ini
        ```SQL Query
        CREATE TABLE news 
        (
            id int(11) NOT NULL AUTO_INCREMENT,
            title varchar(128) NOT NULL,
            slug varchar(128) NOT NULL,
            body text NOT NULL,
            PRIMARY KEY (id),
            KEY slug (slug)
        );
        ```
    
    - Isi table tersebut dengan queri ini 

        INSERT INTO news VALUES
        (1,'Elvis sighted','elvis-sighted','Elvis was sighted at the Podunk internet cafe. It looked like he was writing a CodeIgniter app.'),
        (2,'Say it isn\'t so!','say-it-isnt-so','Scientists conclude that some programmers have a sense of humor.'),
        (3,'Caffeination, Yes!','caffeination-yes','World\'s largest coffee shop open onsite nested coffee shop for staff only.');

2. Atur konfigurasi database
    - Buka app/Config/Database.php
    - Atur pada
        'hostname' => 'localhost',
		'username' => 'root',
		'password' => '',
		'database' => 'ci4tutorial',

3. Membuat model
    - Membuat file baru di app/Models/ dengan nama NewsModel.php
    - Isi file tersebut dengan code di bawah ini
        
        ```PHP
        <?php namespace App\Models;
        use CodeIgniter\Model;
        
        class NewsModel extends Model
        {
            protected $table = 'news';

            public function getNews($slug = false)
            {
                if ($slug === false) 
                {
                    return $this->findAll();
                }

                return $this->asArray()
                            ->where(['slug' => $slug])
                            ->first();
            }
        }
        ```
