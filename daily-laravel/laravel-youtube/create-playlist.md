# create playlist Youtube

```

```php
<?php

use Google_Client;
use Google_Service_YouTube;
use Google_Service_YouTube_Playlist;
use Google_Service_YouTube_PlaylistSnippet;
use Google_Service_YouTube_PlaylistStatus;

class YouTubePlaylistCreator
{
    protected $client;
    protected $youtube;

    public function __construct()
    {
        $this->client = new Google_Client();
        $this->client->setDeveloperKey('YOUR_DEVELOPER_KEY'); // Ganti dengan kunci API Anda
        $this->youtube = new Google_Service_YouTube($this->client);
    }

    public function createPlaylist($title, $description)
    {
        $playlistSnippet = new Google_Service_YouTube_PlaylistSnippet();
        $playlistSnippet->setTitle($title);
        $playlistSnippet->setDescription($description);

        $playlistStatus = new Google_Service_YouTube_PlaylistStatus();
        $playlistStatus->setPrivacyStatus('private'); // Ganti sesuai preferensi Anda (public, private, atau unlisted)

        $playlist = new Google_Service_YouTube_Playlist();
        $playlist->setSnippet($playlistSnippet);
        $playlist->setStatus($playlistStatus);

        $createdPlaylist = $this->youtube->playlists->insert('snippet,status', $playlist, ['part' => 'snippet,status']);

        return $createdPlaylist;
    }
}

// Gunakan YouTubePlaylistCreator untuk membuat playlist baru
$playlistCreator = new YouTubePlaylistCreator();
$createdPlaylist = $playlistCreator->createPlaylist('Judul Playlist', 'Deskripsi Playlist');

echo 'Playlist berhasil dibuat dengan ID: ' . $createdPlaylist->getId();
```

Pastikan untuk mengganti 'YOUR_DEVELOPER_KEY' dengan kunci API yang sudah Anda dapatkan. Script ini akan membuat playlist baru dengan judul dan deskripsi yang ditentukan, kemudian mengembalikan objek playlist yang baru dibuat, termasuk ID-nya. Anda dapat menggunakan ID ini untuk mengelola playlist lebih lanjut, seperti menambahkan video ke dalamnya.

```

```
