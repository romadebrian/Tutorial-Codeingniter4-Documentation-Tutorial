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
        ```SQL Query
        INSERT INTO news VALUES
        (1,'Elvis sighted','elvis-sighted','Elvis was sighted at the Podunk internet cafe. It looked like he was writing a CodeIgniter app.'),
        (2,'Say it isn\'t so!','say-it-isnt-so','Scientists conclude that some programmers have a sense of humor.'),
        (3,'Caffeination, Yes!','caffeination-yes','World\'s largest coffee shop open onsite nested coffee shop for staff only.');
        ```

2. Atur konfigurasi database
    - Buka app/Config/Database.php
    - Atur pada
        ```
        'hostname' => 'localhost',
		'username' => 'root',
		'password' => '',
		'database' => 'ci4tutorial',
        ```

3. Membuat model
    - Buatlah file baru di app/Models/ dengan nama NewsModel.php
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
    
4. Membuat Controller
    - Buatlah file baru di app/Controllers/ dengan nama News.php
    - Lalu isi dengan code berikut
        ```PHP
        <?php namespace App\Controllers;

        use App\Models\NewsModel;
        use CodeIgniter\Controller;

        class News extends Controller
        {
            public function index()
            {
                $model = new NewsModel();

                $data = [
                    'news' => $model->getNews(),
                    'title' => 'News archive',
                ];

                echo view('templates/header', $data);
                echo view('news/overview', $data);
                echo view('templates/footer');
            }

            public function view($slug = null)
            {
                $model = new NewsModel();

                $data['news'] = $model->getNews($slug);

                if (empty($data['news'])) {
                    throw new \CodeIgniter\Exceptions\PageNotFoundException('Cannot find the news item: ' . $slug);
                }

                $data['title'] = $data['news']['title'];

                echo view('templates/header', $data);
                echo view('news/view', $data);
                echo view('templates/footer');
            }
        }
        ```

5. Membuat tampilan untuk daftar berita (overview)
    - Buatlah folder 'news' di app/Views/
    - Buatlah file di app/Views/news/ dengan nama overview.php
    - Isi file tersebut dengan code di bawah sini
        ```PHP
        <h2><?= $title ?></h2>

        <?php if (! empty($news) && is_array($news)) : ?>

                <?php foreach ($news as $news_item): ?>

                        <h3><?= $news_item['title'] ?></h3>

                        <div class="main">
                                <?= $news_item['body'] ?>
                        </div>
                        <p><a href="<?= '/news/'.$news_item['slug'] ?>">View article</a></p>

                <?php endforeach; ?>

        <?php else : ?>

                <h3>No News</h3>

                <p>Unable to find any news for you.</p>

        <?php endif ?>
        ```

6. Membuat view untuk menampilkan berita 1 judul saja / untuk 'View article'
    - Membuat file di app/Views/news/ dengan nama view.php
    - Isi file tersebut dengan code di bawah ini
        ```PHP
        <?php
        echo '<h2>'.$news['title'].'</h2>';
        echo $news['body'];
        ```
7. Mengatur Routing (Penting!!!)
    - Buka file app/config/routes.php
    - lalu cari code $routes->get('/', 'Home::index'); 
      Code tersebut berada di bawah 
      ```PHP
      /**
        * --------------------------------------------------------------------
        * Route Definitions
        * --------------------------------------------------------------------
        */
      ```
    - lalu letakan code berikut di bawah code $routes->get('/', 'Home::index');
        ```
        $routes->get('news/(:segment)', 'News::view/$1');
        $routes->get('news', 'News::index');
        $routes->get('(:any)', 'Pages::showme/$1');
        ```

8. Jalankan 

Note : localhost:8080/news bisa di jalankan dengan menjalankan perintah 'php spark serve' di cmd berlokasi di file codeigniter di jalankan

Jika ada pertanyaan bisa tanya ke https://www.facebook.com/romadebrian

#Terimakasih