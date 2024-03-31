# Api Upload Youtube  Playlist Laravel

```


```php
<?php

use Google_Client;
use Google_Service_YouTube;
use Google_Service_YouTube_PlaylistItem;
use Google_Service_YouTube_PlaylistItemSnippet;
use Google_Service_YouTube_Video;
use Google_Service_YouTube_VideoSnippet;
use Google_Service_YouTube_VideoStatus;

class YouTubeUploader
{
    protected $client;
    protected $youtube;

    public function __construct()
    {
        $this->client = new Google_Client();
        $this->client->setDeveloperKey('YOUR_DEVELOPER_KEY'); // Ganti dengan kunci API Anda
        $this->youtube = new Google_Service_YouTube($this->client);
    }

    public function uploadVideoToPlaylist($videoPath, $title, $description, $playlistId)
    {
        // Upload video
        $snippet = new Google_Service_YouTube_VideoSnippet();
        $snippet->setTitle($title);
        $snippet->setDescription($description);

        $status = new Google_Service_YouTube_VideoStatus();
        $status->privacyStatus = 'private'; // Ganti sesuai preferensi Anda (public, private, atau unlisted)

        $video = new Google_Service_YouTube_Video();
        $video->setSnippet($snippet);
        $video->setStatus($status);

        $chunkSizeBytes = 1 * 1024 * 1024;
        $this->client->setDefer(true);

        $insertRequest = $this->youtube->videos->insert(
            'snippet,status',
            $video,
            array(
                'data' => file_get_contents($videoPath),
                'mimeType' => 'video/*',
                'uploadType' => 'resumable'
            )
        );

        $videoResponse = $insertRequest->execute();
        $this->client->setDefer(false);

        // Add video to playlist
        $playlistItemSnippet = new Google_Service_YouTube_PlaylistItemSnippet();
        $playlistItemSnippet->setPlaylistId($playlistId);
        $playlistItemSnippet->setResourceId($videoResponse['id']['videoId']);

        $playlistItem = new Google_Service_YouTube_PlaylistItem();
        $playlistItem->setSnippet($playlistItemSnippet);

        $this->youtube->playlistItems->insert('snippet', $playlistItem);
    }
}

// Gunakan YouTubeUploader untuk mengupload video ke dalam playlist
$uploader = new YouTubeUploader();
$uploader->uploadVideoToPlaylist('path/to/video.mp4', 'Judul Video', 'Deskripsi Video', 'PLAYLIST_ID');
```

Pastikan untuk mengganti 'YOUR_DEVELOPER_KEY' dengan kunci API yang sudah Anda dapatkan, dan 'PLAYLIST_ID' dengan ID playlist yang sudah ada atau yang ingin Anda buat. Script ini akan mengupload video dan menambahkannya ke dalam playlist yang telah ditentukan.

```

```
